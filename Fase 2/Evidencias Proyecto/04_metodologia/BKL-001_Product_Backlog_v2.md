# Product Backlog

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | BKL-001 · Product Backlog |
| **Versión** | 2.0 |
| **Fecha** | 24 de septiembre de 2026 |
| **Reemplaza a** | Versión 1.0 (10-09-2026) |
| **Estado** | Actualizado con `MRQ-001 v2`. Identificadores según `CAT-001`. Los criterios de aceptación son iniciales y se refinan en el Sprint Planning |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## Convenciones

**Prioridad:** MoSCoW · **Estado:** Confirmado / Propuesto / Pendiente de
validación · **Fuente:** referencia al requerimiento de `MRQ-001` o al acta.

---

## Épica 1 · Autenticación y usuarios

| ID | Historia | Prioridad | Estado | Dependencias | Fuente |
|---|---|---|---|---|---|
| HU-01 | Como ejecutivo, quiero iniciar sesión con mis credenciales, para acceder al sistema con mi identidad | Must | Confirmado | — | RF-01 |
| HU-02 | Como administrador, quiero crear y desactivar usuarios internos, para controlar quién accede al sistema | Must | Confirmado | HU-01 | RF-01 |
| HU-03 | Como cliente, quiero registrarme y acceder al portal, para iniciar y seguir mis trámites | Must | Propuesto | — | RF-19 |

**Criterios de aceptación preliminares (HU-01):** el usuario ingresa con
credenciales válidas; las credenciales inválidas no revelan si el usuario existe;
la sesión expira tras un periodo de inactividad `[VALOR PENDIENTE]`; el ingreso
queda registrado en auditoría.

---

## Épica 2 · Roles y permisos

| ID | Historia | Prioridad | Estado | Dependencias | Fuente |
|---|---|---|---|---|---|
| HU-04 | Como administrador, quiero asignar un rol a cada usuario, para que acceda solo a lo que le corresponde | Must | Confirmado | HU-02 | RF-02, RS-02 |
| HU-05 | Como supervisor, quiero que los ejecutivos no puedan eliminar registros de clientes, para evitar pérdidas de información | Must | Pendiente de validación | HU-04 | RS-02 |

**Criterios de aceptación preliminares (HU-04):** existe un catálogo de roles con
sus facultades; un usuario sin la facultad correspondiente recibe un error de
autorización; el cambio de rol queda registrado en auditoría.

> Los roles concretos y sus facultades están `[PENDIENTE DE VALIDAR CON EASY
> OFFICE]`. La contraparte confirmó que debe haber distintos roles y facultades,
> pero no los enumeró.

---

## Épica 3 · Clientes y empresas

| ID | Historia | Prioridad | Estado | Dependencias | Fuente |
|---|---|---|---|---|---|
| HU-06 | Como ejecutivo, quiero registrar un cliente con sus datos de contacto, para tenerlo disponible en el sistema | Must | Confirmado | HU-01 | RF-05 |
| HU-07 | Como ejecutivo, quiero buscar un cliente y ver su ficha, para atenderlo sin revisar planillas | Must | Confirmado | HU-06 | RF-05 |
| HU-08 | Como ejecutivo, quiero registrar una empresa con su RUT, razón social y representante legal, para emitir documentos a su nombre | Must | Confirmado | HU-06 | RF-05 |
| HU-09 | Como ejecutivo, quiero registrar la vigencia de la representación legal, para saber si quien firma está facultado | Should | Propuesto | HU-08 | RN-03 |

**Criterios de aceptación preliminares (HU-08):** el RUT se valida en formato y
dígito verificador; no se admiten dos empresas con el mismo RUT; los cambios
quedan en auditoría.

---

## Épica 4 · Gestión de trámites

| ID | Historia | Prioridad | Estado | Dependencias | Fuente |
|---|---|---|---|---|---|
| HU-10 | Como ejecutivo, quiero crear un trámite asociado a un cliente y a un tipo de servicio, para gestionarlo en el sistema | Must | Confirmado | HU-06, HU-14 | RF-11 |
| HU-11 | Como ejecutivo, quiero ver el estado de un trámite y su historial, para saber qué falta | Must | Confirmado | HU-10 | RF-11 |
| HU-12 | Como ejecutivo, quiero que el sistema impida transiciones de estado no válidas, para que el proceso se cumpla en orden | Must | Propuesto | HU-10 | RN-08 |
| HU-13 | Como ejecutivo, quiero atender un trámite asistido que requiere mi intervención, para los casos no automatizables | Must | Confirmado | HU-10 | RF-18 |

> Los estados del trámite y sus transiciones válidas están
> `[PENDIENTE DE VALIDAR CON EASY OFFICE]`.

---

## Épica 5 · Motor configurable

| ID | Historia | Prioridad | Estado | Dependencias | Fuente |
|---|---|---|---|---|---|
| HU-14 | Como administrador, quiero definir un tipo de trámite con sus campos y validaciones, para incorporar servicios sin desarrollo | Must | Propuesto | HU-04 | RF-33 |
| HU-15 | Como administrador, quiero asociar una plantilla documental a un tipo de trámite, para que el sistema genere el documento correcto | Must | Propuesto | HU-14, HU-18 | RF-33, RF-36 |
| HU-16 | Como administrador, quiero indicar si un tipo de trámite requiere pago y si requiere firma, para que el sistema construya el flujo | Must | Propuesto | HU-14 | RF-33 |
| HU-17 | Como administrador, quiero definir los estados de un tipo de trámite, para adaptar el flujo a cada servicio | Should | Propuesto | HU-14 | RF-33 |

**Criterios de aceptación preliminares (HU-14):** se puede crear un tipo de
trámite indicando nombre, campos con su tipo y obligatoriedad, y validaciones; al
crear un trámite de ese tipo el formulario se construye a partir de la
configuración; ningún cambio en la configuración altera trámites ya emitidos.

> Esta épica es la propuesta diferenciadora del equipo. **No fue solicitada por
> la contraparte** y se presentará para validación en la reunión de validación
> (hito H2). Se apoya en que
> la empresa declaró querer incorporar cada documento que la ley habilite.

---

## Épica 6 · Gestión documental

| ID | Historia | Prioridad | Estado | Dependencias | Fuente |
|---|---|---|---|---|---|
| HU-18 | Como administrador, quiero cargar una plantilla documental y versionarla, para controlar qué formato se usa | Must | Propuesto | HU-04 | RF-36, RN-06 |
| HU-19 | Como sistema, quiero generar el documento a partir de la plantilla y los datos del trámite, para eliminar el llenado manual | Must | Confirmado | HU-15 | RF-24 |
| HU-20 | Como ejecutivo, quiero que cada documento emitido quede asociado a la versión de plantilla con que se generó, para poder reconstruir su origen | Must | Propuesto | HU-18, HU-19 | RN-06 |
| HU-21 | Como ejecutivo, quiero que los datos usados al emitir un documento queden congelados, para que el documento no cambie si el cliente actualiza su ficha | Must | Propuesto | HU-19 | RN-07 |
| HU-22 | Como supervisor, quiero verificar que el archivo firmado corresponde al generado, para asegurar su integridad | Should | Propuesto | HU-19 | RF-39 |

---

## Épica 7 · Portal cliente

| ID | Historia | Prioridad | Estado | Dependencias | Fuente |
|---|---|---|---|---|---|
| HU-23 | Como cliente, quiero ver el catálogo de servicios disponibles, para elegir el que necesito | Should | Propuesto | HU-03 | RF-19 |
| HU-24 | Como cliente, quiero completar el formulario de domicilio tributario, para solicitar el trámite sin contactar a un ejecutivo | Must | Confirmado | HU-14, HU-03 | RF-19 |
| HU-25 | Como cliente, quiero que el sistema valide mis datos antes de continuar, para no generar un documento con errores | Must | Propuesto | HU-24 | RF-06 |
| HU-26 | Como cliente, quiero ver una vista previa del documento antes de firmarlo, para revisar que los datos estén correctos | Must | Confirmado | HU-19 | RF-21 |
| HU-27 | Como cliente, quiero confirmar que la información es correcta, para que el trámite avance a firma | Must | Confirmado | HU-26 | RF-21 |
| HU-28 | Como cliente, quiero seguir el estado de mi trámite, para saber en qué etapa está | Should | Propuesto | HU-24 | RF-27 |
| HU-29 | Como cliente, quiero descargar el documento final firmado, para disponer de él | Must | Confirmado | HU-37 | RF-26 |

---

## Épica 8 · Auditoría

| ID | Historia | Prioridad | Estado | Dependencias | Fuente |
|---|---|---|---|---|---|
| HU-30 | Como sistema, quiero registrar qué usuario realizó cada acción sobre la información, con fecha, valor anterior y valor nuevo, para dejar trazabilidad | Must | Confirmado | HU-01 | RF-15, RS-04 |
| HU-31 | Como supervisor, quiero consultar el historial de acciones de un registro, para saber quién lo modificó | Should | Propuesto | HU-30 | RF-17 |

**Criterios de aceptación preliminares (HU-30):** cada creación, modificación y
eliminación queda registrada; el registro no admite modificación ni eliminación;
incluye usuario, fecha, entidad afectada, valor anterior y valor nuevo.

---

## Épica 9 · Migración de datos

| ID | Historia | Prioridad | Estado | Dependencias | Fuente |
|---|---|---|---|---|---|
| HU-32 | Como equipo, quiero cargar la información de clientes desde la planilla Excel a la base de datos, para reemplazar Excel como sistema de registro | Must | Confirmado | HU-06, HU-08 | RF-43 |
| HU-33 | Como equipo, quiero obtener un reporte de los registros que no pudieron migrarse y su motivo, para corregirlos con la contraparte | Should | Propuesto | HU-32 | RF-44 |

**Criterios de aceptación preliminares (HU-32):** el proceso es repetible y no
duplica registros al ejecutarse más de una vez; los registros con datos
inconsistentes quedan en cuarentena y no se cargan a medias; el proceso no se
ejecuta con datos personales reales en entornos de desarrollo.

> Bloqueada por la entrega de la planilla.
> `[PENDIENTE DE VALIDAR: estructura y calidad de los datos]`

---

## Épica 10 · Firma electrónica

| ID | Historia | Prioridad | Estado | Dependencias | Fuente |
|---|---|---|---|---|---|
| HU-34 | Como equipo, quiero definir una interfaz `SignatureService` con una implementación simulada, para construir el flujo sin depender de credenciales externas | Must | Propuesto | — | IN-01 |
| HU-35 | Como sistema, quiero enviar el documento al proveedor de firma, para iniciar el proceso de firma | Must | Confirmado | HU-34, HU-19 | RF-25 |
| HU-36 | Como sistema, quiero registrar a los firmantes de un documento y el estado de cada firma, para reflejar documentos con más de un firmante | Must | Confirmado | HU-35 | RF-40 |
| HU-37 | Como sistema, quiero recibir el documento firmado y asociarlo al trámite, para completarlo | Must | Confirmado | HU-35 | RF-25 |

> HU-35 y HU-37 dependen de `[CREDENCIALES PENDIENTES]`. Con la implementación
> simulada de HU-34, el flujo queda construido y probado igualmente.

---

## Épica 11 · Pagos

| ID | Historia | Prioridad | Estado | Dependencias | Fuente |
|---|---|---|---|---|---|
| HU-38 | Como equipo, quiero definir una interfaz `PaymentService` con una implementación de referencia, para no depender de la decisión pendiente del proveedor | Must | Propuesto | — | IN-02 |
| HU-39 | Como cliente, quiero pagar el servicio dentro del flujo del trámite, para no gestionar la transferencia por separado | Must | Confirmado | HU-38, HU-27 | RF-22 |
| HU-40 | Como sistema, quiero registrar el resultado del pago de forma idempotente, para no duplicar cobros ante notificaciones repetidas | Should | Propuesto | HU-39 | RF-22 |

> El momento del pago dentro del flujo está `[PENDIENTE DE VALIDAR]`. La
> contraparte declaró que hoy opera con modalidad 50/50 por transferencia, pero
> no indicó cómo se traslada eso al flujo automatizado.

---

## Épica 12 · Administración

| ID | Historia | Prioridad | Estado | Dependencias | Fuente |
|---|---|---|---|---|---|
| HU-41 | Como supervisor, quiero ver un dashboard con clientes, servicios activos, próximos a vencer, vencidos, ventas totales, por servicio y por ejecutivo, trámites pendientes y documentos pendientes de firma, para dar seguimiento a la operación | Must | Confirmado | HU-10, HU-48 | RF-14 |
| HU-42 | Como ejecutivo, quiero recibir alertas de los servicios próximos a vencer, para gestionar su renovación oportunamente | Must | Confirmado | HU-48, HU-53 | RF-13 |

**Criterios de aceptación preliminares (HU-42):** la anticipación es configurable
por tipo de servicio, con valores iniciales de 60, 30, 15 y 7 días; cada alerta
queda registrada con su estado; el ejecutivo puede marcarla como gestionada.

> Ambas historias pasaron de *Could / Propuesto* a *Must / Confirmado* en esta
> versión: las especificaciones formales las incorporan en sus §4.8 y §4.9.

---

## Épica 14 · Ficha del cliente, búsqueda y servicios

Historias incorporadas en la versión 2.0 a partir de las especificaciones
formales.

| ID | Historia | Prioridad | Estado | Dependencias | Fuente |
|---|---|---|---|---|---|
| HU-47 | Como ejecutivo, quiero buscar clientes por RUT, nombre o razón social, teléfono, correo, folio o servicio contratado, para encontrarlos sin revisar planillas | Must | Confirmado | HU-06 | RF-08 |
| HU-48 | Como ejecutivo, quiero registrar un servicio contratado con su tipo, fechas de inicio y término, estado, precio, características y responsable, para dar seguimiento a lo que el cliente tiene vigente | Must | Confirmado | HU-06 | RF-11, RF-30 |
| HU-49 | Como ejecutivo, quiero ver la ficha del cliente con sus datos, servicios, fechas, documentos, historial de gestiones y de modificaciones, para atenderlo con toda la información a la vista | Must | Confirmado | HU-47, HU-48 | RF-10 |
| HU-50 | Como ejecutivo, quiero filtrar el listado de clientes por estado, fechas y ejecutivo, para trabajar sobre un subconjunto | Should | Confirmado | HU-47 | RF-09 |
| HU-51 | Como sistema, quiero asignar un identificador único a cada cliente, para poder referenciarlo de forma inequívoca | Must | Confirmado | HU-06 | RF-07 |
| HU-52 | Como sistema, quiero detectar posibles duplicidades antes de guardar un cliente, para evitar registros repetidos | Must | Confirmado | HU-06 | RF-06 |
| HU-53 | Como sistema, quiero detectar diariamente los servicios próximos a vencer, para generar las alertas correspondientes | Must | Confirmado | HU-48 | RF-12 |

---

## Épica 15 · Integración portal y CRM

| ID | Historia | Prioridad | Estado | Dependencias | Fuente |
|---|---|---|---|---|---|
| HU-54 | Como sistema, quiero crear o actualizar el registro del cliente con los datos que ingresó en el portal, para que ningún ejecutivo deba redigitarlos | Must | Confirmado | HU-24, HU-06 | RF-28, RF-29 |
| HU-55 | Como sistema, quiero registrar automáticamente el servicio contratado con su vigencia al completarse la contratación, para iniciar su seguimiento | Must | Confirmado | HU-54, HU-48 | RF-30, RF-32 |
| HU-56 | Como sistema, quiero asociar el documento firmado al cliente y a la operación, para que quede disponible en su ficha | Must | Confirmado | HU-37, HU-54 | RF-31 |
| HU-57 | Como sistema, quiero verificar el resultado del pago antes de avanzar a la generación o la firma, para no emitir documentos de operaciones no pagadas | Must | Confirmado | HU-39 | RF-23 |

**Nota.** Esta épica materializa lo que el §9 del encargo identifica como
requisito central: evitar la duplicación de digitación entre el portal y el CRM.

---

## Épica 13 · Pruebas y calidad

| ID | Historia | Prioridad | Estado | Dependencias | Fuente |
|---|---|---|---|---|---|
| HU-43 | Como equipo, quiero pruebas unitarias sobre las validaciones y la generación documental, para detectar regresiones | Must | Propuesto | HU-19 | RNF-06 |
| HU-44 | Como equipo, quiero una prueba de integración del flujo completo del trámite prioritario, para verificar que funciona de extremo a extremo | Must | Propuesto | HU-29 | — |
| HU-45 | Como equipo, quiero pruebas de control de acceso por rol, para verificar que los permisos se respetan | Must | Propuesto | HU-04 | RS-02 |
| HU-46 | Como equipo, quiero medir el tiempo del trámite en la plataforma, para compararlo con la línea base manual | Should | Propuesto | HU-44 | PV-001 |

---

## Won't para este MVP

| Elemento | Razón |
|---|---|
| Automatización de la creación de empresas | Confirmado por la contraparte como proceso asistido |
| Integración con notarías | Confirmado fuera de alcance |
| Integración con el Servicio de Impuestos Internos | La contraparte indicó que no es necesaria |
| Catálogo documental completo | Fuera del alcance del MVP |
| Aplicación móvil | El alcance acordado es una plataforma web |
| Reemplazo del sitio web actual | La plataforma se enlaza, no sustituye |

---

## Estado del backlog

| Estado | Historias |
|---|---|
| Confirmado | 34 |
| Propuesto | 23 |
| Pendiente de validación | 0 |

**Total: 57 historias.**

Este backlog está clasificado con MoSCoW pero **no está ordenado ni estimado para
sprint**. Ese orden se define en el Sprint Planning.

Las historias de las épicas 1, 2, 3 y 14 no dependen de información pendiente de
la contraparte y son las candidatas naturales para el primer sprint de
construcción. Las de las épicas 6, 9, 10 y 11 dependen de las plantillas, la
planilla Excel o las credenciales externas.

---

## Historial de versiones

| Versión | Fecha | Cambios |
|---|---|---|
| 1.0 | 10-09-2026 | Backlog inicial a partir del levantamiento |
| 2.0 | 24-09-2026 | Códigos de requerimiento remapeados según `CAT-001`; corregida la dependencia de HU-29; incorporadas las épicas 14 y 15 con once historias nuevas; HU-41 y HU-42 reclasificadas a Must / Confirmado |
