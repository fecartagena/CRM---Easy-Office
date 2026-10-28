# Manual de operación: despliegue, respaldo y recuperación

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | MOP-001 · Manual de operación |
| **Versión** | 0.1 |
| **Fecha** | 27 de octubre de 2026 |
| **Estado** | **Procedimiento propuesto.** No hay un despliegue ni una restauración ejecutados. El alojamiento productivo sigue abierto (TA-09) |
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
| No ejecutado | — | — | — | — | — | Pendiente | — |

---

## 5. Actualizaciones, reversión e incidentes

- **Antes de actualizar:** respaldo comprobado, lista de migraciones y si son
  reversibles. Se conserva la versión anterior.
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

---

## Historial de versiones

| Versión | Fecha | Cambios |
|---|---|---|
| 0.1 | 27-10-2026 | Versión inicial: ambiente objetivo, publicación, respaldo, ensayo de recuperación, incidentes y entrega operativa |
