# Plan de evidencias, casos de prueba y aceptación técnica

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | EVI-001 · Plan de evidencias |
| **Versión** | 0.4 |
| **Fecha** | 3 de noviembre de 2026 |
| **Estado** | **Plan y formatos.** Solo EVI-T11 se ha ejecutado: el 02-11-2026 sobre las ramas antes de fusionar y el 03-11-2026 sobre el código fusionado, ambas veces en local |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

Complementa `PLA-001` (qué se prueba y con qué herramientas) y
`12_evidencias/README.md` (qué datos registra cada evidencia). Este documento
define los casos de punta a punta, la estructura de las carpetas y el acta de
aceptación técnica interna.

---

## 1. Regla de evidencia

Cada afirmación de funcionamiento se puede ubicar en una versión, un caso y un
resultado. Un conteo de pruebas, una captura del prototipo y un PR fusionado no
son intercambiables.

Las cifras de pruebas son de cortes distintos y se citan con el suyo:

| Corte | Backend | Frontend | Fuente |
|---|---|---|---|
| Guía 2.4, 20-10-2026 | 545 | 131 | Entrega de la Fase 2 |
| Rama principal, 27-10-2026 | 685 (más 62 del PSC de pruebas) | Ver `PLA-001` | `PLA-001` v1.11 |
| Rama de auditoría antes de fusionar, 22-10-2026 | 696 | — | `PLA-001` v1.11 |
| Pantallas de auditoría, rama del frontend antes de fusionar, 02-11-2026 | — | 223 (32 archivos) | `PLA-001` v1.12 |
| Rama principal con staging (PR #73 y #29), 01-11-2026 | 715 (más 64 del PSC de pruebas) | 165 | `PLA-001` v1.13 |
| Rama principal con la auditoría (PR #62 y #31), 03-11-2026 | 799, según el PR | 223 | `PLA-001` v1.14 |

Este documento no ejecutó esas suites ni revisó sus registros en GitHub Actions.

---

## 2. Estructura de carpetas

Una carpeta por candidato: `12_evidencias/AAAA-MM-DD_candidato/`, con:

- `manifest.md`: commits de los tres repositorios, modo de cada proveedor,
  ambiente y datos de prueba;
- `resultados.md`: casos, resultado y defectos;
- `capturas/`: imágenes nombradas con el ID del caso;
- `logs/`: salidas saneadas y códigos de retorno;
- `documentos/`: PDF y Word generados solo con datos sintéticos.

Los informes de cobertura, rendimiento y seguridad siguen en `09_pruebas/` según
`PLA-001` §6; el `manifest.md` los enlaza.

### Manifest

| Campo | Valor |
|---|---|
| Candidato y fecha real | Pendiente |
| Commit del backend, del frontend y de la documentación | Pendiente |
| Ambiente, navegador y versiones | Pendiente |
| Pago, PDF y firma | El modo de cada uno; nunca suponer *real* |
| Datos | Sintéticos; indicar el comando o lote |
| Quién ejecutó y quién revisó | Pendiente |
| Casos ejecutados, aprobados, fallidos y bloqueados | No medido |
| Defectos y decisión | Pendiente |

---

## 3. Casos de punta a punta

Todos comienzan en **No ejecutado**.

| ID | Caso | Resultado esperado | Referencias |
|---|---|---|---|
| EVI-T01 | El cliente A contrata domicilio, revisa el borrador, paga en el modo elegido y un usuario autorizado sube el PDF firmado | Expediente coherente, sin duplicados; se indica que la firma fue asistida | RF-19 a RF-32, UC-01 |
| EVI-T02 | El cliente B intenta ver, editar o pagar una solicitud de A por URL y por API | Rechazo, sin exponer ni alterar datos | RS-02, RF-49 |
| EVI-T03 | Abrir el panel como Administrador, como Ejecutivo y como cliente | Administrador y Ejecutivo lo ven; el cliente no entra al backoffice | RF-14, RN-31, RN-32, D2-06 |
| EVI-T04 | Aviso de pago repetido, inválido o con otro monto | Idempotencia y rechazo; nada se marca pagado por la página de retorno | RF-22, RF-23 |
| EVI-T05 | Cambiar la ficha del cliente después de generar el documento | La versión anterior conserva sus datos congelados | RN-15 |
| EVI-T06 | Contratos con fechas normales, de fin de mes y en año bisiesto | Vencimiento según RN-37 en base de datos, pantalla y PDF | RN-37 |
| EVI-T07 | Subir un PDF firmado válido y archivos inválidos, con y sin permiso | Tipo, tamaño (10 MB) y permisos controlados; no reemplaza el archivo de otro trámite | RF-31, RF-39, `DEC-001` DE-09 |
| EVI-T08 | Bodega en el portal | Plan, inmueble, documento, pago y seguimiento coherentes | `PLT-02` |
| EVI-T09 | Bodega en el backoffice | Flujo interno sin venta en línea; el panel no suma ingresos ficticios | `PLT-02` |
| EVI-T10 | Firma avanzada contra el PSC de pruebas: todos firman, un firmante rechaza y una solicitud vence | Completado con PDF verificado contra la raíz de pruebas; en el rechazo y el vencimiento, *falta de información* con el motivo | RF-25, RF-39, RF-40, IN-01, `ARQ-001` AD-16, `just e2e-firma` |
| EVI-T11 | Integrar la auditoría, modificar un cliente, consultar el registro e intentar alterarlo | Actor, fecha y cambio registrados; el historial no se puede editar | RF-15 a RF-17, HU-30, HU-31 |
| EVI-T12 | Configurar un tipo de documento de prueba dentro de las capacidades del motor | Funciona, y se lista lo que fue dato, plantilla o código | RF-33 a RF-37, `DEC-001` DE-04 |
| EVI-T13 | Importar un lote sintético dos veces y con errores | Conciliación, cuarentena, sin duplicados | RF-43, RF-44, `MIG-001` |
| EVI-T14 | Servicios que vencen en 60, 30, 15 y 7 días | Alertas según la regla, sin omisiones ni duplicados | RF-12, RF-13, RN-09 (no implementado: HU-42, HU-53) |
| EVI-T15 | Restaurar base y archivos en un ambiente aislado | Conteos, relaciones y documentos recuperados; tiempos medidos | `MOP-001` §4 |
| EVI-T16 | Comparar `PLT-01` y `PLT-02` con el PDF generado | Campos, cláusulas, anexos, precios, firmantes y saltos de página correctos | R-10, `PLT-000` |

### Ficha de resultado

| Campo | Valor |
|---|---|
| Caso | EVI-T__ |
| Estado | No ejecutado |
| Fecha, persona, ambiente y commits | Pendiente |
| Precondición y datos exactos | Completar antes de ejecutar |
| Resultado esperado | El de la tabla, concretado |
| Resultado observado | Lo que ocurrió |
| Evidencia | Archivo o enlace |
| Defecto, severidad y reejecución | Si aplica; el primer resultado no se borra |
| Revisor y decisión | Pendiente |

### Fichas registradas

**EVI-T11 · Auditoría**

| Campo | Valor |
|---|---|
| Caso | EVI-T11 |
| Estado | **Ejecutado sobre el código fusionado el 03-11-2026, en local** (backend `cfa51fa`, frontend `f37c58d`; `12_evidencias/2026-11-03_auditoria/`). Antes, el 02-11-2026, se ejecutó sobre las ramas antes de fusionar, cuyas cabezas (`2acf8b5` y `48d541d`) son las que se fusionaron. Primer registro (02-11): "ejecutado en local contra ramas sin fusionar; el paso «integrar la auditoría» no está cumplido" |
| Fecha, persona, ambiente y commits | **03-11-2026:** ejecutado por Claude (Claude in Chrome) a pedido de Fernando Cartagena, que hizo los ingresos con cuentas de demostración; equipo Windows, Docker local y Vite; backend `cfa51fa`, frontend `f37c58d` (`manifest.md`). **02-11-2026:** ejecutado por Claude Code a pedido del equipo. Docker local y Vite. Backend `2acf8b5` (PR #62, rama `feature/HU-30-auditoria`). Frontend: recorrido sobre los cambios de la rama `feature/HU-31-auditoria` antes de confirmarlos, con el mismo contenido de `48d541d` |
| Precondición y datos exactos | Datos sintéticos: cuentas de `seed_demo_usuarios` y un cliente de prueba `CLI-000023`. El 03-11, además, el correo inventado `prueba.limite@example.test` para el límite de intentos |
| Resultado esperado | Actor, fecha y cambio registrados; el historial no se puede editar; solo el Administrador consulta |
| Resultado observado | **03-11-2026** (`resultados.md`): la modificación del teléfono del cliente de prueba aparece en la ficha y en el registro filtrado, con usuario, fecha, canal, IP y "Telefono: (vacío) → +56 9 1234 5678"; la fecha final anterior a la inicial muestra el error junto al filtro; el intento 11 responde con el mensaje de límite; como Ejecutivo no hay entrada en el menú, la URL directa muestra "Sin acceso" y la API responde `403`. No se repitieron los métodos de escritura, que el intento 11 no deje fila ni el ingreso y cierre en el registro. **02-11-2026:** crear y editar un cliente, ingresar, cerrar sesión e intentar un ingreso fallido aparecen en la pantalla con actor, fecha y cambio. Como Ejecutivo no hay entrada en el menú, la URL directa muestra "Sin acceso" y la API responde `403`. Como Administrador, `POST`, `PUT`, `PATCH` y `DELETE` responden `405` o `404`, y el total de filas no cambia. Con el límite de intentos, el intento 11 responde `429` y no deja fila |
| Límite | En local la IP registrada es la de la red de Docker (`172.18.0.1`); no prueba la IP real detrás del proxy. Se revisa en staging |
| Evidencia | `01-registro-filtrado.jpg`, `02-detalle-modificacion.jpg`, `03-historial-en-ficha.jpg`, `04-error-hasta-antes-de-desde.jpg`, `05-ejecutivo-sin-acceso.jpg`, `06-ingreso-429.jpg`. Las del 03-11-2026 están en `12_evidencias/2026-11-03_auditoria/capturas/`, con el prefijo `EVI-T11_`; las del 02-11-2026 no se archivaron. Solo muestran cuentas de demostración y datos sintéticos |
| Defecto, severidad y reejecución | Ninguno abierto en la pantalla. Reejecutado sobre el código fusionado el 03-11-2026; queda comprobar en staging la pantalla y la IP registrada |
| Revisor y decisión | Nicolás Zapata (informado por Fernando Cartagena el 03-11-2026). Decisión: sin registrar |

---

## 4. Rendimiento

Metas de `PLA-001`, adoptadas como objetivos de prueba en `DEC-001` DE-13. **No
son resultados ni un acuerdo de servicio.**

| Escenario | Carga | Meta |
|---|---|---|
| Búsqueda de clientes | 5.000 clientes sintéticos | Percentil 95 bajo 1 s |
| Lista de servicios con filtros | 10.000 servicios | Percentil 95 bajo 2 s |
| Panel | El mismo conjunto | Percentil 95 bajo 3 s |
| Contratación concurrente | 10 operaciones simultáneas | Sin errores inesperados ni folios duplicados |
| Detección de vencimientos | 10.000 servicios | Lote completo bajo 5 min |

Antes de medir se fijan hardware, workers, volumen, calentamiento, duración y
proveedores en modo simulado. Se informan mínimo, mediana, percentiles 95 y 99,
tasa de errores y uso de recursos. Estado: **No ejecutado**.

---

## 5. Seguridad

Se revisan sesión y CSRF, permisos por rol, aislamiento entre clientes, descarga
privada, validación de archivos, secretos, dependencias y la firma del webhook
del PSC (HMAC, ventana de 5 minutos). Cada herramienta de `PLA-001` se informa
con su versión y salida real.

| ID | Hallazgo | Evidencia saneada | Severidad | Responsable | Corrección o PR | Reejecución | Estado |
|---|---|---|---|---|---|---|---|
| Por completar | No se presume ausencia de hallazgos | — | — | — | — | — | No ejecutado |

---

## 6. Capturas del anexo A de la guía 2.4

| Anexo | Captura | Estado |
|---|---|---|
| A.1 | Catálogo y planes | Tomada el 27-10-2026 (pendiente de agregar a la guía) |
| A.2 | Formulario de domicilio con datos sintéticos | Pendiente |
| A.3 | Vista previa del PDF de `PLT-01` | Pendiente |
| A.4 | Pago y su estado, con el modo indicado | Pendiente |
| A.5 | Panel con datos sintéticos | Pendiente |
| A.6 | Detalle del trámite en el backoffice: firmantes o carga del PDF firmado | Pendiente |
| A.7 | Esquema de PostgreSQL del commit entregado | Pendiente |
| A.8 | Resultado de las pruebas, con fecha y commit, por repositorio | Pendiente |
| A.9 | Contribuciones por integrante (gráfico de GitHub más la tabla de `GES-001` §5) | Tomada el 27-10-2026 para backend y frontend |

Los mockups no reemplazan estas capturas. Cada una lleva fecha, versión, rol y
escenario en su pie.

---

## 7. Trazabilidad de los casos

| Requisito | Historia | PR | Caso | Resultado y fecha | Estado |
|---|---|---|---|---|---|
| RF-15 a RF-17 | HU-30, HU-31 | Backend #62 y frontend #31, fusionados el 03-11-2026 | EVI-T11 | Ejecutado sobre el código fusionado el 03-11-2026, en local (`12_evidencias/2026-11-03_auditoria/`) | Fusionado; revisado por Nicolás Zapata |
| RF-25, RF-39, RF-40 | Firma avanzada | Backend #68, frontend #27 | EVI-T10 | No ejecutado aquí | Fusionado; aceptación pendiente |
| RF-43, RF-44 | Verificar historia vigente en `BKL-001` | — | EVI-T13, `MIG-T` | No ejecutado | Pendiente |
| RF-19 a RF-32 | HU-23 a HU-29 | Backend #51, frontend #18 | EVI-T01 | No ejecutado aquí | Fusionado; aceptación pendiente |

La trazabilidad individual de cada requisito está en `TRZ-001`.

---

## 8. Acta de aceptación técnica interna

**Modalidad (`DEC-001` DE-02):** revisa el incremento un integrante que no lo
implementó. No es aceptación de Easy Office.

- Fecha y medio: pendiente.
- Evaluadores y versión: pendiente.
- Casos para el piloto: EVI-T01, T02, T03, T05, T10 y T16.
- Resultado por caso: no ejecutado.
- Defectos, restricciones y decisiones: se completan en la revisión.
- Decisión: aceptado técnicamente, aceptado con restricciones o requiere
  corrección, solo después de evaluar.
- Confirmación de Easy Office: campo aparte, vacío si no existe.

---

## 9. Medición del beneficio

Las 3 a 4 horas del proceso actual son una declaración de Easy Office, no una
medición del equipo. Antes de calcular una reducción se define si se mide el
tiempo total o el de trabajo activo, y se registran al menos diez operaciones de
prueba con su modo de pago y de firma. Una firma de pruebas instantánea no se
compara con la espera real de una firma con validez legal sin advertirlo.

---

## Historial de versiones

| Versión | Fecha | Cambios |
|---|---|---|
| 0.1 | 27-10-2026 | Versión inicial: regla de evidencia, carpetas, 16 casos de punta a punta (incluida la firma avanzada), rendimiento, seguridad, capturas del anexo A y acta de aceptación técnica |
| 0.2 | 02-11-2026 | Ficha de EVI-T11, ejecutado en local sobre ramas sin fusionar (backend PR #62, frontend `feature/HU-31-auditoria`); corte de pruebas del frontend de esa rama |
| 0.3 | 03-11-2026 | Corte de pruebas de la rama principal con staging (PR #73 y #29) |
| 0.4 | 03-11-2026 | EVI-T11: auditoría fusionada (backend PR #62, frontend PR #31); reejecutado sobre el código fusionado en local, con capturas en `12_evidencias/2026-11-03_auditoria/`; el resultado del 02-11 se conserva; revisor Nicolás Zapata; corte de pruebas de la rama principal con la auditoría |
