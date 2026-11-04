# Manual de operación: despliegue, respaldo y recuperación

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | MOP-001 · Manual de operación |
| **Versión** | 0.2 |
| **Fecha** | 3 de noviembre de 2026 |
| **Estado** | **Procedimiento propuesto para producción.** Staging está desplegado en una VM (§1.1); la restauración solo se ensayó en una pila local desechable. El alojamiento productivo sigue abierto (TA-09) |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

La instalación local está en `07_arquitectura/MTE-001`.

---

## 1. Ambiente objetivo

Propuesta técnica (`DEC-001` DE-12): un host Linux con Docker Compose que ejecuta
PostgreSQL, Redis, Django, los workers de Celery y Gotenberg, y un proxy HTTPS
que sirve el frontend compilado y deriva `/api` al backend.

`docker-compose.production.yml` del backend define `django`, `postgres`,
`traefik`, `redis`, `gotenberg`, `celeryworker`, `celerybeat`, `flower` y
`nginx`, con volúmenes separados para la base de datos, sus respaldos, los
archivos generados y los archivos privados. **No incluye el PSC de pruebas**, y
así debe seguir (`ARQ-001` AD-16). Tampoco incluye todavía el frontend: falta
documentar cómo se publica y cómo se resuelven las rutas de React.

| Dato operativo | Estado |
|---|---|
| Proveedor y sistema operativo del host | No definido (TA-09) |
| Dominio y responsable del DNS | No definido |
| URL del frontend y de la API | No publicada |
| Commits o imágenes desplegadas | Se registran al desplegar |
| Operador principal y de respaldo | Por asignar en el equipo |
| Modo de pago y de firma | Se indica por componente: simulado, sandbox o real |
| Almacenamiento de respaldos | Por elegir y probar |
| Costo y quién lo paga | No se asume sin registro |

Una demostración local no reemplaza el despliegue que figura en el alcance. Si
la entrega se limita a una demostración, se registra como ajuste de alcance.

### 1.1 Staging

Ambiente en internet, solo con datos ficticios, donde funciona todo lo que
funciona en local (`ARQ-001` §6.1). Lo crearon el backend PR #73 (issue #72) y el
frontend PR #29 (issue #28). La guía paso a paso, en nueve secciones más el plan
alternativo para Let's Encrypt, es `docs/staging.md` del backend.

| Dato operativo | Staging |
|---|---|
| Host | VM de Compute Engine en `southamerica-west1`, Ubuntu 24.04, 4 GB de RAM, 2 GB de swap; puertos 22, 80 y 443 |
| Dominio | `<ip-con-guiones>.sslip.io`, `firma.` y `correo.` del mismo; un certificado de Let's Encrypt |
| Acceso | Un usuario `basicAuth` (`equipo`) en los tres hosts; contraseña fuera del repositorio |
| Servicios | `django`, `postgres`, `redis`, `celeryworker`, `celerybeat`, `gotenberg`, `frontend`, `firma-sandbox`, `firma-sandbox-worker`, `mailpit`, `nginx`, `traefik` |
| Modo de pago y de firma | Klap y Flow en sandbox; firma avanzada con el PSC de pruebas, sin validez legal; todo el correo va a Mailpit |
| Usuarios de demostración | `seed_demo_usuarios --force`: un Administrador, un Ejecutivo y dos clientes empresa del portal, con contraseñas del entorno |
| Secretos de GitHub | `STAGING_HOST`, `STAGING_USER`, `STAGING_SSH_KEY` en el ambiente `staging` de ambos repositorios. **Todavía sin configurar** |

**Despliegue.** Cada merge a `main` que pasa la CI ejecuta `deploy.sh` por SSH
con una clave restringida (`restrict,command=`) del usuario `deploy`. El script
compila en la VM, migra, revisa la salud y, si falla, restaura el código y las
imágenes anteriores. No revierte migraciones, pero respalda la base antes de
migrar si hay migraciones pendientes. Sin los secretos, el flujo termina sin
desplegar y deja un aviso.

**Respaldos.** Diarios a las 03:00, hora de Chile, con 7 días de retención: base de
datos, archivos privados y datos del PSC. Quedan en la misma VM, así que todavía
no cumplen la copia fuera del host de §3.

**Copia de la base local.** Solo a pedido, con `scripts/staging/copy_local_db.sh`,
nunca en un despliegue. Reemplaza lo que hay en staging, salvo los datos del PSC.
Antes de salir del computador, `anonimizar_datos` anonimiza una copia desechable:

- se niega sin `--es-copia`, con configuración de producción o de staging, y si la
  base no se llama `crm_copia`;
- genera nombres, correos, teléfonos, direcciones y RUT; cada RUT tiene dígito
  verificador válido, es único y es consistente entre tablas, desde 91.000.000;
- de `Documento.datos_snapshot` solo conserva `folio`, fechas, duración y
  `servicios`; el resto se reemplaza, incluidos valores que ya ningún registro
  tiene;
- deja las contraseñas inutilizables y vacía tokens, MFA, sesiones, `LogEntry` y
  los payloads de proveedores (`Pago.respuesta`, `EventoPago.payload`,
  `EventoFirma.payload`);
- reemplaza las cédulas por una imagen PNG de relleno, y cada Word y PDF por un
  archivo "Documento no disponible en la copia de staging", con su SHA-256
  recalculado;
- cierra las `SolicitudFirma` y `OrdenPago` abiertas;
- borra las tablas que ningún modelo conoce, y avisa cuáles;
- al final busca los valores originales en todas las tablas, salvo los catálogos
  del framework y las columnas clasificadas como no identificatorias. Las columnas
  sobrescritas se comparan por valor exacto y las demás por subcadena. Si queda
  alguno, falla y no confirma nada.

Cada campo de texto o JSON está clasificado en `ANONIMIZADOS`, `CONSERVADOS` o
`REVISADOS`; una prueba falla si aparece uno nuevo sin clasificar.

**Comprobado en la VM:** el primer despliegue con `deploy.sh`; el certificado para
los tres hosts; la contraseña en los tres hosts y los puertos internos cerrados
desde afuera; el primer respaldo automático de las 03:00; una orden creada en Klap
con su webhook de validación respondido con 200; y una solicitud de firma enviada
al PSC, con su correo en Mailpit y el enlace público.

**Sin confirmar todavía en la VM:** el pago con Flow; que el webhook de
confirmación de pago se haya registrado; la firma completa de los tres firmantes;
la copia real de la base; y las variantes de la demostración. **Sin ejecutar:** el
despliegue automático desde GitHub Actions, porque el ambiente `staging` aún no
tiene secretos.

---

## 2. Publicación

1. Elegir el candidato y anotar los commits de backend, frontend y
   documentación. Confirmar pruebas, variables y migraciones.
2. Configurar fuera de Git los secretos propios del ambiente, los hosts y
   orígenes, el correo, los certificados y las URL de retorno de pago. Ningún
   `CHANGEME` puede quedar.
3. Crear los volúmenes y comprobar que persisten la base de datos, los archivos
   generados y los privados. Los privados nunca se sirven por `/media/`.
4. Configurar el frontend compilado, el proxy hacia la API y los certificados.
   Probar cookies y CSRF con el dominio real.
5. Si el ambiente ya tiene datos, respaldar antes de migrar. Levantar la versión
   y registrar los comandos efectivamente usados en el host.
6. Verificar ingreso por rol, una solicitud sintética, el PDF, el pago en el modo
   indicado y la consulta. Revisar el reinicio de los workers, los errores y el
   espacio en disco.
7. Registrar resultado, limitaciones y URL. El estado pasa a *desplegado* solo
   con esa evidencia.

---

## 3. Qué se respalda

| Componente | Por qué | Cómo se comprueba |
|---|---|---|
| PostgreSQL | Clientes, contratos, ventas, configuración y relaciones | Restaurar y comparar conteos |
| Archivos generados y plantillas | La base guarda la referencia, no los bytes | Abrir el PDF o Word de un expediente recuperado |
| Archivos privados | Adjuntos sensibles, como documentos de identidad del autorizante | Recuperarlos con permisos; el acceso directo público debe fallar |
| Configuración y secretos | Permiten reconstruir el ambiente | Copia protegida y de acceso restringido; nunca en evidencia pública |
| Versión de la aplicación | Indica el esquema compatible | Commits, imágenes y migraciones incluidas |

Propuesta (`DEC-001` DE-14): respaldo diario con copia fuera del host, 7 diarios y
4 semanales, pérdida máxima de 24 horas y recuperación en 4 horas. **Son metas,
no resultados.** La base y los archivos se respaldan de forma coordinada, con una
pausa breve de escritura o un mecanismo equivalente.

El backend trae los scripts `backup`, `backups`, `restore` y `rmbackup` en
`compose/production/postgres/maintenance/`. Para un ensayo local con datos
sintéticos:

```bash
docker compose -f docker-compose.local.yml exec postgres backup
docker compose -f docker-compose.local.yml exec postgres backups
```

`backup` hace un `pg_dump` comprimido en el volumen de respaldos. Solo cubre
PostgreSQL: no copia archivos, no saca la copia del host ni prueba la
recuperación.

---

## 4. Ensayo de recuperación

1. Preparar un ambiente aislado, distinto del de operación, con credenciales de
   prueba y la misma versión de la aplicación.
2. Tomar el respaldo autorizado (base, archivos y registro de versión) y anotar la
   hora de inicio.
3. Restaurar la base con `restore`. **Ese script ejecuta `dropdb` sobre la base
   destino**: confirmar antes que el destino es el ambiente de ensayo.
4. Restaurar los volúmenes de archivos con sus rutas y permisos. Usar
   configuración de prueba para que no salgan pagos, correos ni avisos reales.
5. Comparar conteos de clientes, contratos, trámites y documentos. Abrir un PDF y
   un archivo privado por la ruta autorizada y comprobar que otro cliente no
   accede a ellos.
6. Anotar la hora de término, el tiempo de recuperación, la fecha del último dato
   recuperado, las desviaciones y la decisión.

| Fecha y versión | Ambiente aislado | Respaldo usado | Conteos antes y después | Archivo recuperado | Tiempo medido | Resultado | Operador y revisor |
|---|---|---|---|---|---|---|---|
| Antes del merge del PR #73, pila local desechable | Base separada | Respaldo de `scripts/staging/backup.sh` | No registrados | No registrado | No medido | Restauró; ensayo parcial, sin conteos ni tiempos | No registrado |
| Producción: no ejecutado | — | — | — | — | — | Pendiente | — |

---

## 5. Actualizaciones, reversión e incidentes

- **Antes de actualizar:** respaldo comprobado, lista de migraciones y si son
  reversibles. Se conserva la versión anterior. En staging, `deploy.sh` respalda
  la base si hay migraciones pendientes y, si la revisión de salud falla, restaura
  el código y las imágenes anteriores, no las migraciones (§1.1).
- **Si la verificación falla:** detener nuevas operaciones y conservar los
  registros. Volver a la versión anterior solo si el esquema sigue siendo
  compatible; si no, aplicar la recuperación ensayada y documentar lo que se pierde
  después del respaldo.
- **PDF detenido:** revisar worker, Redis y Gotenberg y reintentar desde la
  aplicación. No duplicar contratos ni pagos a mano.
- **Pago inconsistente:** conservar orden, venta, evento y respuesta saneada, y
  conciliar con el proveedor antes de cambiar estados.
- **Acceso indebido:** restringir el acceso afectado, conservar la evidencia y
  registrar la corrección, sin datos reales en un issue público.

---

## 6. Entrega operativa

- [ ] Inventario de servicios, versiones, dominio y costos reales.
- [ ] Operador principal y de respaldo, que acepten la responsabilidad.
- [ ] Accesos entregados por un canal autorizado, sin secretos en un acta pública.
- [ ] Respaldo y restauración probados; retención y espacio revisados.
- [ ] Limitaciones conocidas y modo de cada integración.
- [ ] Manual de usuario (`MUS-001`) revisado y sesión de traspaso, si corresponde.
- [ ] Acta de entrega, solo cuando la entrega ocurra.

**Pendientes de staging**

- [ ] Configurar los secretos del ambiente `staging` en ambos repositorios y
      comprobar el primer despliegue automático.
- [ ] Comprobar en la VM lo que quedó sin confirmar (§1.1).
- [ ] `docs/staging.md`, sección 3: en un clon nuevo no existe `.envs/`; falta
      crearla antes de copiar a `.envs/.staging`.
- [ ] Confirmar si Flow sandbox acepta correos `@example.test`.
- [ ] Al terminar el semestre, eliminar la VM y liberar la IP estática, que sigue
      cobrando si queda reservada.

---

## Historial de versiones

| Versión | Fecha | Cambios |
|---|---|---|
| 0.1 | 27-10-2026 | Versión inicial: ambiente objetivo, publicación, respaldo, ensayo de recuperación, incidentes y entrega operativa |
| 0.2 | 03-11-2026 | Staging (backend PR #73, frontend PR #29): ambiente, despliegue continuo, respaldos, copia anonimizada de la base, lo comprobado en la VM y pendientes |
