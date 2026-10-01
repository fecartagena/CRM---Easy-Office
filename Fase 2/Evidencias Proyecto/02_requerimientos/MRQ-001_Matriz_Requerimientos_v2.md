# Matriz de requerimientos

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | MRQ-001 · Matriz de requerimientos |
| **Versión** | 2.0 |
| **Fecha** | 24 de septiembre de 2026 |
| **Reemplaza a** | Versión 1.0 (10-09-2026) |
| **Motivo de la revisión** | Incorporación de las especificaciones formales entregadas por Easy Office |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## Convenciones

**Tipo:** RF requerimiento funcional · RNF requerimiento no funcional ·
RN regla de negocio · RE restricción · IN integración · RS requerimiento de
seguridad

**Prioridad (MoSCoW):** Must · Should · Could · Won't para este MVP

**Estado:** Confirmado (declarado o escrito por la contraparte) · Propuesto
(decisión del equipo) · Pendiente de validación

**Fuente:** `ESP §n` especificaciones formales de Easy Office · `T1` sesión 1 de
levantamiento · `T2` sesión 2 de levantamiento · `EQ` decisión del equipo

---

## Resumen de cambios respecto de la versión 1.0

| # | Cambio | Origen |
|---|---|---|
| CR-01 | El motor configurable pasa de *Propuesto* a *Confirmado* | ESP §5.2, §8.2, §8.3, §11 |
| CR-02 | Roles y permisos quedan especificados con sus facultades | ESP §4.2 |
| CR-03 | Alertas de vencimiento pasan de *Could / Pendiente* a *Must / Confirmado* | ESP §4.8 |
| CR-04 | Dashboard pasa de *Could / Propuesto* a *Must / Confirmado* | ESP §4.9 |
| CR-05 | Búsqueda y filtros quedan especificados con sus criterios | ESP §4.5 |
| CR-06 | Ficha del cliente queda especificada con su contenido | ESP §4.6 |
| CR-07 | El registro de auditoría queda confirmado campo por campo | ESP §4.3 |
| CR-08 | La detección de duplicidades al guardar pasa a *Confirmado* | ESP §4.4 |
| CR-09 | Se incorpora la recomendación técnica como entregable | ESP §10 |
| CR-10 | Se incorporan respaldos periódicos como requerimiento | ESP §4.3 |
| CR-11 | Se incorpora la verificación del pago antes de continuar | ESP §5.3 |
| CR-12 | Se precisa el registro automático de la operación en el CRM | ESP §9 |
| CR-13 | Los documentos iniciales se precisan: declaraciones juradas de domicilio y de soltería | ESP §8.2 |
| CR-14 | Se agregan requerimientos de integridad documental y trazabilidad ya diseñados | EQ |

---

## Requerimientos funcionales · CRM interno

| ID | Nombre | Descripción | Fuente | Prioridad | Estado | Observaciones |
|---|---|---|---|---|---|---|
| RF-01 | Autenticación individual | Acceso mediante usuario y contraseña propios de cada usuario interno | ESP §4.3 | Must | Confirmado | Sin cuentas compartidas |
| RF-02 | Modelo de permisos por roles | Perfiles de acceso diferenciados, con posibilidad de crear nuevos perfiles sin reconstruir el sistema | ESP §4.2 | Must | Confirmado | Roles iniciales: Dueño/Administrador y Ejecutivo |
| RF-03 | Administración de usuarios | Crear usuarios y asignarles permisos | ESP §4.2 | Must | Confirmado | Facultad exclusiva del perfil administrador |
| RF-04 | Restricción de eliminación | Los ejecutivos no pueden eliminar registros de la base de datos | ESP §4.2 | Must | Confirmado | Se expresa como permiso del rol |
| RF-05 | Formulario de ingreso de clientes | Interfaz web sencilla para que los ejecutivos ingresen la información del cliente | ESP §4.4 | Must | Confirmado | Toma como referencia el formulario Excel con macros |
| RF-06 | Validación previa al guardado | Validar campos obligatorios, formatos y posibles duplicidades antes de guardar | ESP §4.4 | Must | Confirmado | La acción de guardar debe ser claramente identificable |
| RF-07 | Identificador único de cliente | Cada cliente cuenta con un identificador único | ESP §4.5 | Must | Confirmado | Folio |
| RF-08 | Búsqueda de clientes | Búsqueda por RUT, nombre o razón social, teléfono, correo, folio y servicio contratado | ESP §4.5 | Must | Confirmado | — |
| RF-09 | Filtros de listado | Filtros por estado, fechas, ejecutivo y otros criterios relevantes | ESP §4.5 | Must | Confirmado | — |
| RF-10 | Ficha del cliente | Vista centralizada con datos, contacto, servicios, fechas, estados, documentos, historial de gestiones, ejecutivo responsable e historial de modificaciones | ESP §4.6 | Must | Confirmado | — |
| RF-11 | Gestión de servicios contratados | Un cliente puede tener uno o varios servicios, con tipo, fechas, estado, precio, características, documentos, observaciones y responsable | ESP §4.7 | Must | Confirmado | — |
| RF-12 | Detección de vencimientos | El sistema detecta automáticamente los servicios con fecha de vencimiento próxima | ESP §4.8 | Must | Confirmado | Proceso programado |
| RF-13 | Alertas internas configurables | Generación de alertas con anticipación configurable, inicialmente 60, 30, 15 y 7 días | ESP §4.8 | Must | Confirmado | Ejemplo dado por la contraparte: renovación de domicilio tributario |
| RF-14 | Dashboard de indicadores | Total de clientes, nuevos por período, servicios activos, próximos a vencer, vencidos, ventas, ventas por servicio, ventas por ejecutivo, trámites pendientes y documentos pendientes de firma | ESP §4.9 | Must | Confirmado | Con filtros por período y otros criterios |
| RF-15 | Registro de auditoría | Registrar usuario, fecha y hora, registro afectado, acción realizada, y valor anterior y nuevo cuando corresponda | ESP §4.3 | Must | Confirmado | — |
| RF-16 | Protección del historial de auditoría | El historial debe estar protegido para impedir que usuarios normales lo alteren o eliminen | ESP §4.3 | Must | Confirmado | Se implementa como tabla append-only |
| RF-17 | Consulta de auditoría | Los perfiles autorizados pueden consultar auditoría e historial | ESP §4.2 | Must | Confirmado | Facultad del perfil administrador |
| RF-18 | Procesos asistidos | El sistema soporta servicios que requieren intervención de un ejecutivo | T2 | Must | Confirmado | Aplica a la creación de empresas |

---

## Requerimientos funcionales · Portal de contratación

| ID | Nombre | Descripción | Fuente | Prioridad | Estado | Observaciones |
|---|---|---|---|---|---|---|
| RF-19 | Catálogo de servicios contratables | El cliente puede conocer y seleccionar los servicios disponibles | ESP §5.1 | Must | Confirmado | — |
| RF-20 | Formularios dinámicos por servicio | Cada servicio solicita únicamente los datos necesarios; el formulario se adapta sin requerir un desarrollo nuevo para cada servicio | ESP §5.2 | Must | Confirmado | Requerimiento que sustenta el motor configurable |
| RF-21 | Revisión y confirmación de datos | El cliente revisa y confirma sus antecedentes antes de continuar | ESP §5.1 | Must | Confirmado | — |
| RF-22 | Pago en línea | El cliente paga el servicio dentro del flujo de contratación | ESP §5.1, §5.3 | Must | Confirmado | Proveedor pendiente |
| RF-23 | Verificación del resultado del pago | El sistema verifica el resultado del pago antes de continuar con las etapas posteriores | ESP §5.3 | Must | Confirmado | — |
| RF-24 | Generación automática del documento | El sistema genera la documentación correspondiente a partir de la plantilla y los datos ingresados | ESP §5.1, §6 | Must | Confirmado | — |
| RF-25 | Envío al proceso de firma | El documento generado se envía a la plataforma de firma electrónica | ESP §5.1, §7 | Must | Confirmado | Sujeto a IN-01 |
| RF-26 | Entrega del documento final | El cliente recibe el documento firmado | ESP §8.1 | Must | Confirmado | — |
| RF-27 | Seguimiento del estado | El cliente puede consultar el estado de su contratación | EQ | Should | Propuesto | No solicitado explícitamente; se deriva del flujo |

---

## Requerimientos funcionales · Integración portal ↔ CRM

| ID | Nombre | Descripción | Fuente | Prioridad | Estado | Observaciones |
|---|---|---|---|---|---|---|
| RF-28 | Registro automático de la operación | La operación realizada en el portal se registra automáticamente en el CRM | ESP §5.1, §9 | Must | Confirmado | Requisito central del encargo |
| RF-29 | Creación o actualización del cliente | Los datos ingresados por el cliente crean o actualizan su registro sin redigitación interna | ESP §9 | Must | Confirmado | — |
| RF-30 | Registro del servicio contratado | Se registra el servicio con su fecha de inicio y de vencimiento | ESP §9 | Must | Confirmado | Habilita RF-12 y RF-13 |
| RF-31 | Asociación del documento firmado | El documento firmado queda asociado al cliente y a la operación | ESP §7, §9 | Must | Confirmado | — |
| RF-32 | Inicio del seguimiento | El CRM comienza el seguimiento del servicio y sus vencimientos | ESP §9 | Must | Confirmado | — |

---

## Requerimientos funcionales · Motor configurable y gestión documental

| ID | Nombre | Descripción | Fuente | Prioridad | Estado | Observaciones |
|---|---|---|---|---|---|---|
| RF-33 | Definición configurable de tipos de trámite | Un tipo de trámite se define mediante campos, validaciones, plantilla, estados y necesidad de pago y firma | ESP §5.2, §8.3, §11 | Must | Confirmado | Corazón del motor |
| RF-34 | Incorporación modular de documentos | El sistema permite incorporar nuevos tipos de documentos de forma modular | ESP §8.2 | Must | Confirmado | — |
| RF-35 | Incorporación de nuevos servicios | El sistema permite incorporar nuevos servicios sin reconstruirse | ESP §11 | Must | Confirmado | — |
| RF-36 | Sistema de plantillas | Cada documento cuenta con una plantilla configurable donde los datos del cliente se insertan automáticamente | ESP §6, §8.3 | Must | Confirmado | Easy Office ya tiene los formatos definidos |
| RF-37 | Versionado de plantillas | Cada documento emitido queda asociado a la versión de plantilla con que se generó | EQ | Must | Propuesto | Necesario para reconstruir el origen de un documento |
| RF-38 | Copia congelada de los datos | Los datos usados al emitir un documento no cambian si la ficha del cliente se actualiza después | EQ | Must | Propuesto | — |
| RF-39 | Verificación de integridad | Verificación mediante hash de que el archivo firmado corresponde al generado | EQ | Should | Propuesto | — |
| RF-40 | Múltiples firmantes | Un documento puede requerir la firma de más de una parte | T1 | Must | Confirmado | — |
| RF-41 | Máquina de estados por tipo de trámite | Un trámite solo transita entre estados según las transiciones definidas | EQ | Must | Propuesto | Estados pendientes de validación |

---

## Requerimientos funcionales · Datos

| ID | Nombre | Descripción | Fuente | Prioridad | Estado | Observaciones |
|---|---|---|---|---|---|---|
| RF-42 | Base de datos centralizada | Una sola base de datos centralizada y segura para toda la información | ESP §4.1 | Must | Confirmado | Aproximadamente 30 campos por cliente |
| RF-43 | Migración desde Excel | Reemplazar progresivamente los procesos realizados en Excel con macros | ESP §2 | Must | Confirmado | El formulario Excel es la referencia funcional |
| RF-44 | Reporte de inconsistencias de migración | El proceso de migración entrega un reporte de los registros no cargados y su motivo | EQ | Should | Propuesto | — |

---

## Requerimientos no funcionales

| ID | Nombre | Descripción | Fuente | Prioridad | Estado |
|---|---|---|---|---|---|
| RNF-01 | Persistencia relacional | Base de datos relacional. PostgreSQL según `REC-001` | ESP §4.1 | Must | Confirmado el motor relacional; PostgreSQL es recomendación del equipo |
| RNF-02 | Plataforma web | Solución web accesible desde navegador | ESP §3 | Must | Confirmado |
| RNF-03 | Escalabilidad de datos | Permitir aumentar considerablemente clientes y registros sin reemplazar la arquitectura principal | ESP §4.1 | Must | Confirmado |
| RNF-04 | Escalabilidad funcional | Permitir incorporar servicios, documentos, usuarios, roles, sucursales, formas de pago, plataformas de firma y automatizaciones | ESP §11 | Must | Confirmado |
| RNF-05 | Respaldo periódico | Respaldo periódico de la base de datos | ESP §4.3 | Must | Confirmado |
| RNF-06 | Usabilidad para no técnicos | Los ejecutivos deben poder usar el CRM sin conocimientos técnicos | ESP §13 | Must | Confirmado |
| RNF-07 | Portabilidad | Entorno reproducible mediante contenedores | EQ | Must | Propuesto |
| RNF-08 | Mantenibilidad | La configuración de servicios, documentos, roles y flujos se administra como datos, no como código | ESP §10, §11 | Must | Confirmado |
| RNF-09 | Documentación técnica | README, configuración, variables de entorno e instrucciones de despliegue | EQ | Must | Propuesto |
| RNF-10 | Rendimiento | El sistema debe responder adecuadamente al volumen declarado: 200 a 250 clientes mensuales, meta de 50 operaciones diarias | T2, ESP §11 | Should | Propuesto |
| RNF-11 | Disponibilidad | Alojamiento con disponibilidad adecuada y plan de recuperación | ESP §10 | Should | Confirmado el criterio; la solución concreta es recomendación del equipo |

---

## Requerimientos de seguridad

| ID | Nombre | Descripción | Fuente | Prioridad | Estado |
|---|---|---|---|---|---|
| RS-01 | Acceso individual | Usuario y contraseña individuales | ESP §4.3 | Must | Confirmado |
| RS-02 | Control de permisos por rol | Los permisos impiden acciones no autorizadas | ESP §4.3, §13 | Must | Confirmado |
| RS-03 | Protección de la información almacenada | Medidas de protección sobre los datos almacenados | ESP §4.3 | Must | Confirmado |
| RS-04 | Registro de accesos | Registro de accesos y acciones relevantes | ESP §4.3 | Must | Confirmado |
| RS-05 | Medidas de infraestructura y aplicación | Medidas de seguridad recomendadas por el equipo para infraestructura y aplicación | ESP §4.3 | Must | Confirmado el requerimiento; las medidas son recomendación del equipo |
| RS-06 | Gestión de secretos | Credenciales fuera del código, en variables de entorno | EQ | Must | Propuesto |
| RS-07 | Minimización de datos | Solicitar únicamente los campos que cada servicio requiere | ESP §5.2 | Should | Confirmado |
| RS-08 | Datos reales fuera del repositorio | Ningún dato personal real se versiona; el repositorio es público | EQ | Must | Propuesto |
| RS-09 | Anonimización en desarrollo | Los entornos de desarrollo y prueba operan con datos anonimizados | EQ | Must | Propuesto |

---

## Restricciones

| ID | Restricción | Fuente | Estado |
|---|---|---|---|
| RE-01 | La creación de empresas no se automatiza: varía entre clientes y requiere ingreso manual a plataformas externas | T2 | Confirmado |
| RE-02 | Los finiquitos no forman parte del flujo: se tramitan con la Dirección del Trabajo | T1 | Confirmado |
| RE-03 | Los documentos automatizables son los que la legislación chilena permite emitir y firmar electrónicamente | ESP §8.2 | Confirmado |
| RE-04 | La integración de firma está sujeta a las capacidades técnicas y la API disponibles del proveedor | ESP §7 | Confirmado |
| RE-05 | La relación con notarías se mantiene manual | T2 | Confirmado |
| RE-06 | El proyecto se desarrolla en 18 semanas con un equipo de tres personas | Plan del proyecto | Confirmado |
| RE-07 | El proveedor de alojamiento no está definido | EQ | Pendiente de validación |

---

## Integraciones

| ID | Integración | Descripción | Fuente | Prioridad | Estado | Observaciones |
|---|---|---|---|---|---|---|
| IN-01 | Firma electrónica | Envío del documento, ejecución del proceso de firma, recepción del resultado y actualización automática del estado en el CRM | ESP §7 | Must | Confirmado el requerimiento; **API y credenciales pendientes** | El §7 menciona modalidad de firma desasistida y subordina la integración a las capacidades técnicas disponibles. El §14 compromete la documentación de la API "si está disponible" |
| IN-02 | Pasarela de pago | Cobro en línea con confirmación automática del resultado | ESP §5.3 | Must | Confirmado el requerimiento; **proveedor pendiente de decisión** | El equipo debe recomendar la solución considerando seguridad, comisiones, medios de pago nacionales, facilidad de integración y confirmación automática |
| IN-03 | Notificaciones a clientes por correo o mensajería | Envío de recordatorios por canales externos | ESP §4.8 | Could | Confirmado como extensión futura | La arquitectura queda preparada; fuera del MVP |
| IN-04 | Otras herramientas o sistemas | Conexión posterior con otros sistemas | ESP §11 | Won't | Confirmado fuera del MVP | Escalabilidad futura |
| IN-05 | Notarías | — | T2 | Won't | Confirmado fuera de alcance | Contacto manual |
| IN-06 | Servicio de Impuestos Internos | — | T2 | Won't | Confirmado fuera de alcance | La contraparte indicó que no es necesaria |

---

## Reglas de negocio

Se detallan en `RN-001 v2.0`, que incorpora las reglas confirmadas por las
especificaciones formales: la restricción de eliminación para el perfil
ejecutivo, la protección del historial de auditoría, la anticipación configurable
de las alertas y la verificación del pago previa a la generación del documento.

**Nota sobre los identificadores.** La renumeración de esta versión respecto de
la 1.0 está documentada en `CAT-001 Catálogo de identificadores`, que incluye la
tabla de equivalencias. Los códigos de esta versión quedan congelados: las
versiones siguientes solo agregan identificadores nuevos.

---

## Criterios de éxito declarados por la contraparte

El §13 del encargo enumera diez criterios. Se registran acá porque son la vara
con la que Easy Office evaluará el resultado.

| # | Criterio | Requerimientos que lo cubren |
|---|---|---|
| 1 | Información centralizada y trazable | RF-42, RF-15, RF-16 |
| 2 | Los ejecutivos pueden usar el CRM sin conocimientos técnicos | RNF-06, RF-05 |
| 3 | Los permisos impiden acciones no autorizadas | RF-02, RF-04, RS-02 |
| 4 | Registro de las acciones relevantes de los usuarios | RF-15, RS-04 |
| 5 | La información del cliente no requiere nueva digitación interna | RF-28, RF-29 |
| 6 | Los servicios con vencimiento generan alertas | RF-12, RF-13 |
| 7 | Los contratos y documentos definidos se generan automáticamente | RF-24, RF-36 |
| 8 | El sistema de firma se integra con el flujo de contratación | RF-25, IN-01 |
| 9 | El sistema permite incorporar nuevos servicios y documentos | RF-33, RF-34, RF-35 |
| 10 | Mecanismos adecuados de seguridad y respaldo | RS-01 a RS-09, RNF-05 |

---

## Resumen por estado

**Total: 77 registros** · 44 RF · 11 RNF · 9 RS · 7 RE · 6 IN

| Estado | Cantidad |
|---|---|
| Confirmado por la contraparte | 59 |
| Confirmado con condición | 5 |
| Propuesto por el equipo | 12 |
| Pendiente de validación | 1 |

*Confirmado con condición* corresponde a registros donde la contraparte confirmó
el requerimiento pero un aspecto queda abierto: RNF-01 (confirma el motor
relacional, PostgreSQL es recomendación del equipo), RNF-11, RS-05, IN-01 (la API
del proveedor de firma) e IN-02 (el proveedor de pago).

La proporción cambió sustancialmente respecto de la versión 1.0, donde 24 eran
confirmados y 25 propuestos o pendientes. Las especificaciones formales
convirtieron en requerimiento del cliente varias cosas que el equipo había
planteado como propuesta, en particular el motor configurable.

---

## Historial de versiones

| Versión | Fecha | Cambios | Responsable |
|---|---|---|---|
| 1.0 | 10-09-2026 | Versión inicial a partir de las dos sesiones de levantamiento | Equipo |
| 2.0 | 24-09-2026 | Incorporación de las especificaciones formales de Easy Office; reorganización por áreas del sistema; incorporación de vencimientos, dashboard, búsqueda y ficha del cliente; mapeo de los criterios de éxito | Equipo |
