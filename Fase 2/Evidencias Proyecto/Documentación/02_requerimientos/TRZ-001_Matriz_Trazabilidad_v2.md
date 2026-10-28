# Matriz de trazabilidad

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | TRZ-001 · Matriz de trazabilidad |
| **Versión** | 2.14 |
| **Fecha** | 27 de octubre de 2026 |
| **Reemplaza a** | Versión 1.0 (10-09-2026) |
| **Motivo** | La versión 1.0 referenciaba los identificadores de `MRQ-001 v1.0`, que la versión 2.0 reutilizó para requerimientos distintos. Esta versión está reconstruida contra el catálogo vigente `CAT-001` |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## Propósito

Demostrar la cadena completa del proyecto:

**Requerimiento → Historia de usuario → Desarrollo → Prueba → Evidencia**

Las columnas de issue, commit, prueba y evidencia se completan a medida que
avanza el desarrollo, de modo que la matriz registra el avance real del proyecto
sprint a sprint. «Implementado» significa que el código está fusionado en la rama
principal con pruebas automatizadas; «Implementado parcial», que cubre solo parte
del requerimiento, y lo que falta se indica en la misma fila. Ningún requerimiento
está todavía «Verificado» con evidencia archivada ni «Validado con la
contraparte».

## Objetivos específicos referenciados

Según la versión reducida de objetivos (cinco específicos).

| Código | Objetivo específico |
|---|---|
| OE-1 | Especificar requerimientos, reglas de negocio y diseño funcional, validados con Easy Office |
| OE-2 | Implementar el CRM interno con base relacional, control de acceso por roles, trazabilidad y migración desde Excel |
| OE-3 | Implementar un motor configurable de servicios, formularios, plantillas y flujos documentales |
| OE-4 | Automatizar el servicio piloto de domicilio tributario de extremo a extremo, hasta el seguimiento de su vencimiento |
| OE-5 | Verificar y desplegar la solución, con pruebas y documentación técnica de operación |

---

## Matriz · CRM interno

| Requerimiento | Historia | Caso de uso | Objetivo | Módulo | Issue | Commit / PR | Prueba | Evidencia | Estado |
|---|---|---|---|---|---|---|---|---|---|
| RF-01 Autenticación individual | HU-01 | — | OE-2 | core, backoffice | Backend #14 · Frontend #2 | Backend PR #15 · correcciones de sesión: backend PR #52 y frontend PR #19, fusionados | 5 pruebas de convivencia de las sesiones del panel y de la aplicación web; pruebas de guardias, ingreso y registro (128 frontend) | — | Implementado |
| RF-02 Permisos por roles | HU-04 | UC-10 | OE-2 | core | Backend #14 | Backend PR #15 · permisos por módulo en los PR siguientes | Roles sembrados como grupos y pruebas de permisos en cada API; la suite completa por rol sigue pendiente (HU-45) | — | Implementado · Administrador y Ejecutivo (D-02 cerrada en `ACTA-002`) |
| RF-03 Administración de usuarios | HU-02 | UC-10 | OE-2 | core | — | Backend PR #15 (roles) | Sin pruebas propias | — | Implementado parcial · desde el panel de administración de Django; sin pantalla propia en el backoffice |
| RF-04 Restricción de eliminación | HU-05 | UC-10 | OE-2 | core | #17 | Backend PR #18 (la API de clientes no expone eliminación) · PR #42 (el panel no permite eliminar) | `clientes/tests/test_admin.py` y pruebas de la API de clientes | — | Implementado |
| RF-05 Formulario de ingreso | HU-06, HU-08 | UC-04 | OE-2 | clientes | #17 · #35 (HU-08) | PR #18 · PR #40 (HU-08) | Pruebas de la API de clientes y de los datos civiles y la personería | — | Implementado |
| RF-06 Validación previa al guardado | HU-52 | UC-04 | OE-2 | clientes | #17 | Backend PR #18 · Frontend PR #5 | Pruebas de la API de clientes: RUT con dígito verificador y RUT ya registrado | — | Implementado parcial · duplicidad por RUT; sin coincidencia aproximada por nombre |
| RF-07 Identificador único y folio de contrato | HU-51, HU-60 | UC-04 | OE-2 | clientes, contratos | #17 · #38 (HU-60) | PR #18 (código del cliente) · PR #45 (HU-60) | Folio común entre sucursales y marcas, reinicio anual, ancho configurable, creación concurrente, alias `folio` | — | Implementado |
| RF-08 Búsqueda de clientes | HU-47 | UC-05 | OE-2 | clientes | #17 | Backend PR #18 · Frontend PR #5 | Búsqueda por cada palabra, RUT con o sin puntos; pruebas de `ListaClientes` | — | Implementado parcial · RUT, nombre, razón social, nombre de fantasía, correo y teléfono; faltan folio y servicio contratado |
| RF-09 Filtros de listado | HU-50 | UC-05 | OE-2 | clientes | #17 | Backend PR #18 · Frontend PR #5 | Filtros por tipo (persona o empresa) y por estado activo | — | Implementado parcial · faltan fechas y ejecutivo |
| RF-10 Ficha del cliente | HU-49 | UC-05 | OE-2 | clientes | #17 | Backend PR #18 · Frontend PR #5 | Pruebas de `FichaCliente` | — | Implementado parcial · datos del cliente y representantes legales; servicios y documentos pendientes (HU-48); el historial ya tiene API (`/api/auditoria/historial/`, PR #62 abierto) y falta la pantalla (HU-31) |
| RF-11 Gestión de servicios contratados | HU-48 | UC-06 | OE-2 | contratos | #38 | Backend PR #45 | Pruebas de contratos, líneas y folio | Falta pantalla en prototipo | Implementado parcial · contrato con líneas, vigencia y estado; falta gestionar cada servicio por separado (estado, responsable, características) |
| RF-12 Detección de vencimientos | HU-53 | UC-14 | OE-4 | servicios | Pendiente | Pendiente | Pendiente | — | Especificado |
| RF-13 Alertas configurables | HU-42 | UC-09 | OE-4 | servicios | Pendiente | Pendiente | Pendiente | Falta pantalla en prototipo | Especificado |
| RF-14 Dashboard | HU-41 | UC-08 | OE-2 | panel, backoffice | Backend #53 · Frontend #20 | Backend PR #54 · Frontend PR #21 | Indicadores con datos reales, fecha de referencia, acceso por rol (Administrador y Ejecutivo), Excel sin valores de ejemplo, número de consultas acotado (545 pruebas backend); panel sin rótulos de ejemplo, desglose por motivo, nota de ventas y estado vacío (131 pruebas frontend) | Prototipo pantalla 7 | Implementado |
| RF-47 Motivo de pendiente | HU-61 | UC-11 | OE-2 | tramites | Backend #50 · Frontend #17 · Backend #53 y Frontend #20 (dashboard) | Backend PR #51 · Frontend PR #18 | Motivo solo mientras está Pendiente; 123 pruebas frontend; pendientes por motivo en el dashboard | — | Implementado · motivos provisionales (TA-13) |
| RF-50 Consulta de ventas | HU-64 | UC-17 | OE-2 | ventas, backoffice | Frontend #7 | Frontend #9, #10 | Pruebas de ListaVentas y DetalleVenta | — | Implementado en pruebas |
| RF-15 Registro de auditoría | HU-30 | UC-13 | OE-2 | core | Backend #61 | Backend PR #62, abierto | `core/tests/test_auditoria_*.py`: creación, modificación y eliminación con usuario, IP y origen desde la API, el portal, el admin y Celery; sin fila si no hay cambios; campos excluidos (59 pruebas nuevas, 696 en total) | Prototipo pantalla 8 (interfaz pendiente, HU-31) | Implementado en el backend · PR #62 abierto |
| RF-16 Protección del historial | HU-30 | UC-13 | OE-2 | core | Backend #61 | Backend PR #62, abierto | `UPDATE`, `DELETE` y `TRUNCATE` directos fallan en la base; el modelo y el admin no permiten modificar ni eliminar | — | Implementado en el backend · PR #62 abierto |
| RF-17 Consulta de auditoría | HU-31 | UC-13 | OE-2 | core | Backend #61 | Backend PR #62, abierto | Matriz de acceso (Administrador 200; Ejecutivo, cliente y anónimo 403), filtros, historial agrupado y número de consultas acotado | Prototipo pantalla 8 (pantallas pendientes) | API implementada · pantallas pendientes |
| RF-18 Procesos asistidos | HU-13 | UC-07 | OE-2 | tramites | Backend #59 · Frontend #22 | Backend PR #60 · Frontend PR #23 | Trámite de bodega iniciado por un ejecutivo desde el backoffice | — | Implementado parcial · el ejecutivo inicia y gestiona trámites y registra la confirmación del cliente; sin cola de trámites asistidos |

## Matriz · Portal de contratación

| Requerimiento | Historia | Caso de uso | Objetivo | Módulo | Issue | Commit / PR | Prueba | Evidencia | Estado |
|---|---|---|---|---|---|---|---|---|---|
| RF-19 Catálogo de servicios | HU-23 | UC-01 | OE-4 | portal, ventas | Backend #39 | Backend PR #44 (catálogo del formulario) y PR #48 (servicios visibles y contratables) · Frontend PR #16 (landing desde la API) | Semillas e idempotencia; servicio contratable con precio; visibles no contratables; la landing lee la API | Prototipo pantalla 1 | Implementado · precios sin confirmar (IVA) · bodega vendible en el portal con tres planes (PR #60, fusionado) |
| RF-20 Formularios dinámicos | HU-24 | UC-01 | OE-3 | portal, tramites | Backend #50, #59 · Frontend #17, #22 | Backend PR #51, #60 · Frontend PR #18, #23 | 498 pruebas backend; flujo verificado en el navegador con datos ficticios; 123 pruebas frontend | Prototipo pantalla 2 | Implementado parcial · formulario genérico para los dos tipos de trámite, que sale de su configuración; la pantalla para configurarlos sigue pendiente (HU-14) |
| RF-21 Revisión y confirmación | HU-26, HU-27 | UC-01 | OE-4 | portal, tramites | Backend #50 · Frontend #17 | Backend PR #51 · Frontend PR #18 | Vista previa del PDF real, edición con versión nueva y confirmación; 123 pruebas frontend | Prototipo pantalla 3 | Implementado |
| RF-22 Pago en línea | HU-39, HU-62 | UC-17 | OE-4 | integrations, ventas, portal | Backend #23, #27 · Frontend #7, #11 | Backend #25, #26, #28 · Frontend #9, #10, #12 | Pruebas unitarias y de integración (214 backend, 97 frontend); pagos de prueba con Klap y Flow en sandbox | Prototipo pantalla 4 | Implementado en pruebas · proveedor y precios por confirmar |
| RF-23 Verificación del pago | HU-57 | UC-17 | OE-4 | integrations, ventas | Backend #23, #27 | Backend #25, #26, #28 | Pruebas de webhooks (duplicados, firma falsa, monto distinto, rechazo tardío) y de sincronización | — | Implementado en pruebas |
| RF-24 Generación del documento | HU-19, HU-65, HU-66 | UC-15 | OE-4 | documentos, configuracion, inmuebles | #49 (prueba técnica) · #50 · #36 · #37 | Backend PR #51 · HU-65 y HU-66 fusionadas | Cada campo de `PLT-01` desde una sola fuente; sin campos sin llenar; sucursal incompleta rechazada | — | Implementado · `PLT-01` · `PLT-02` (backend #59, PR #60 fusionado) |
| RF-25 Envío a firma | HU-35 | UC-16 | OE-4 | integrations, documentos | Backend #66, #67 · Frontend #26 | Backend PR #68 · Frontend PR #27 | `test_firma_avanzada.py`, `test_signature_http.py`: envío en Celery con reintentos; prueba de punta a punta (`just e2e-firma`) | Prototipo pantalla 5 | Implementado contra el PSC de pruebas · proveedor real pendiente (TA-07) |
| RF-26 Entrega del documento final | HU-29 | UC-03 | OE-4 | portal, documentos | Backend #50 · Frontend #17 | Backend PR #51 · Frontend PR #18 | Descarga del PDF firmado, sin caché; subida del firmado por un ejecutivo (firma asistida); 123 pruebas frontend | Prototipo pantalla 6 | Implementado |
| RF-27 Seguimiento del estado | HU-28 | UC-02 | OE-4 | portal, tramites | Backend #50 · Frontend #17 | Backend PR #51 · Frontend PR #18 | Estados y motivo de pendiente en el portal; 123 pruebas frontend | Prototipo pantallas 5 y 6 | Implementado |
| RF-45 Cuenta del cliente | HU-03 | UC-01 | OE-4 | clientes, portal | Backend #22 · Frontend #6 | Backend #24 · Frontend #8 · correcciones de sesión: backend PR #52 y frontend PR #19, fusionados | Pruebas de registro, vinculación por RUT y guardias de ruta | — | Implementado |
| RF-46 Acceso desde el sitio web | HU-58 | UC-01 | OE-4 | portal | Pendiente | Pendiente | Pendiente | — | Especificado |
| RF-48 Elección del medio de pago | HU-62 | UC-17 | OE-4 | portal | Frontend #11 | Frontend #12 | Pruebas del selector y del checkout de Klap y Flow | — | Implementado en pruebas |
| RF-49 Compras del cliente | HU-63 | UC-01 | OE-4 | portal | Frontend #7 | Frontend #9, #10 | Pruebas de pantallas de pago | — | Implementado en pruebas |
| RNF-12 Identidad visual | HU-59 | — | OE-4 | frontend | Frontend #24 | Frontend PR #25 | Pruebas de los tokens de marca y del contraste de cada par de colores | — | Implementado · logo entregado por Easy Office; colores tomados del sitio web `[supuesto hasta TA-06]` |

## Matriz · Integración portal y CRM

| Requerimiento | Historia | Caso de uso | Objetivo | Módulo | Issue | Commit / PR | Prueba | Evidencia | Estado |
|---|---|---|---|---|---|---|---|---|---|
| RF-28 Registro automático de la operación | HU-54 | UC-01 | OE-4 | tramites | Backend #50 | Backend PR #51 | Pruebas del trámite iniciado en el portal: crea trámite, contrato en borrador y venta | — | Implementado |
| RF-29 Creación o actualización del cliente | HU-54 | UC-01 | OE-4 | clientes | Backend #50 | Backend PR #51 (`guardar_datos_contratante`) | `clientes/tests/test_contratante.py` | — | Implementado · la empresa y su representante se guardan desde el formulario del portal |
| RF-30 Registro del servicio contratado | HU-55 | UC-01 | OE-4 | contratos | Backend #50 | Backend PR #51 | El contrato y sus líneas se crean como borrador y pasan a vigente al completarse | — | Implementado |
| RF-31 Asociación del documento firmado | HU-56 | UC-16 | OE-4 | documentos | Backend #50 · Frontend #17 | Backend PR #51 · Frontend PR #18 | Subida del PDF firmado en el backoffice, con su permiso y SHA-256 | Prototipo pantalla 8 | Implementado: firma asistida (por defecto) y firma avanzada mediante un proveedor (PR #68), ambas por `guardar_firmado` |
| RF-32 Inicio del seguimiento | HU-55 | UC-01 | OE-4 | contratos | Backend #50 | Backend PR #51 | El contrato vigente queda con su fecha de vencimiento calculada (RN-37) | — | Implementado parcial · faltan la detección de vencimientos y las alertas (RF-12, RF-13) |

## Matriz · Motor configurable y documentos

| Requerimiento | Historia | Caso de uso | Objetivo | Módulo | Issue | Commit / PR | Prueba | Evidencia | Estado |
|---|---|---|---|---|---|---|---|---|---|
| RF-33 Tipos de trámite configurables | HU-14, HU-15, HU-16 | UC-11 | OE-3 | tramites | Backend #59 · Frontend #22 (HU-15) | Backend PR #60 · Frontend PR #23, fusionados el 21-10-2026 | Segundo tipo de trámite (`bodega`) sobre las mismas funciones genéricas que el domicilio; claves de `TipoTramite.configuracion` documentadas en el modelo; botones "Nuevo trámite" según los tipos iniciables; 637 pruebas del backend y 150 del frontend | Falta pantalla en prototipo · `docs/variation-points.md` del backend | En desarrollo · dos tipos de trámite como datos; sin pantalla de configuración ni motor genérico |
| RF-34 Incorporación modular de documentos | HU-15 | UC-11 | OE-3 | documentos | Backend #59 · Frontend #22 | Backend PR #60 · Frontend PR #23, fusionados el 21-10-2026 | Cada campo de `PLT-02` desde una sola fuente; duración del plan impresa en el documento; bodega o autorizante incompletos rechazados; formulario del portal y pantalla "Nuevo trámite" del backoffice (150 pruebas frontend) | `PLT-02` anonimizado en `13_plantillas` | En desarrollo · segundo documento (`PLT-02`); cada plantilla aún requiere su convertidor |
| RF-35 Incorporación de nuevos servicios | HU-14 | UC-11 | OE-3 | ventas, tramites | Backend #39, #59 | Backend PR #44, #48, #60 | El catálogo y los planes se editan en el panel de administración | — | En desarrollo · un servicio nuevo se agrega como dato; un documento nuevo todavía requiere su conversión de plantilla (`docs/variation-points.md`) |
| RF-36 Sistema de plantillas | HU-15, HU-18 | UC-12 | OE-3 | documentos | Backend #59 | Backend PR #60, fusionado | Pasos de conversión compartidos entre `PLT-01` y `PLT-02`; comando `convertir_plantilla` | Falta pantalla en prototipo | En desarrollo · conversión común para dos plantillas; sin administración de plantillas |
| RF-37 Versionado de plantillas | HU-18, HU-20 | UC-12 | OE-3 | documentos | Backend #50 | Backend PR #51 | Versión inmutable una vez usada | Prototipo pantalla 3 | Implementado |
| RF-38 Copia congelada de los datos | HU-21 | UC-15 | OE-4 | documentos | Backend #50 | Backend PR #51 | HU-43 · copia congelada y SHA-256 por archivo | — | Implementado |
| RF-39 Verificación de integridad | HU-22 | UC-16 | OE-4 | documentos | Backend #67 | Backend PR #68 | El hash original informado coincide con `sha256_pdf`, el documento enviado es prefijo exacto del firmado y el hash descargado coincide; pruebas con prefijo y hash distintos | — | Implementado en la firma avanzada · en la asistida solo se guarda el SHA-256 del archivo subido |
| RF-40 Múltiples firmantes | HU-36 | UC-16 | OE-4 | documentos | Backend #67 · Frontend #26 | Backend PR #68 · Frontend PR #27 | Firmantes en orden con estado (pendiente, firmado, rechazado); rechazo y expiración; el portal solo ve nombres y estados | Prototipo pantallas 5 y 8 | Implementado · firmantes y orden `[supuesto]` |
| RF-41 Máquina de estados | HU-12, HU-17 | UC-11 | OE-3 | tramites | Backend #50 | Backend PR #51 | Pruebas de transiciones válidas e inválidas | — | Implementado parcial · estados y motivos con transiciones en una tabla del código (MD-17); no se configuran por tipo de trámite |

## Matriz · Datos, calidad y entrega

| Requerimiento | Historia | Caso de uso | Objetivo | Módulo | Issue | Commit / PR | Prueba | Evidencia | Estado |
|---|---|---|---|---|---|---|---|---|---|
| RF-42 Base de datos centralizada | — | — | OE-2 | datos | — | Todos los PR del backend | Suite del backend sobre PostgreSQL | `MOD-001`, `UML-002` | Implementado · una sola base para CRM y portal (AD-02) |
| RF-43 Migración desde Excel | HU-32 | — | OE-2 | migracion | Pendiente | Pendiente | Pendiente | — | Bloqueado por entrega de la planilla |
| RF-44 Reporte de inconsistencias | HU-33 | — | OE-2 | migracion | Pendiente | Pendiente | Pendiente | — | Propuesto |
| RNF-01 Persistencia relacional | — | — | OE-2 | datos | — | Estructura inicial del backend (20-09) | Suite del backend sobre PostgreSQL 17 | `REC-001`, `MOD-001` | Implementado |
| RNF-03 Escalabilidad de datos | — | — | OE-2 | datos | Pendiente | Pendiente | Pendiente | `MOD-001 §5` | Diseñado |
| RNF-04 Escalabilidad funcional | HU-14 | UC-11 | OE-3 | tramites | Backend #59 | Backend PR #60 | Segundo tipo de trámite sobre las mismas funciones genéricas | `docs/variation-points.md` del backend | En desarrollo · dos tipos por configuración; cada plantilla nueva aún requiere código de conversión |
| RNF-05 Respaldo periódico | — | — | OE-5 | infra | — | Imagen de PostgreSQL de producción con scripts de respaldo y restauración | Sin prueba de restauración | `REC-001 §7` | Diseñado · falta la política y el destino de los respaldos (TA-09) |
| RNF-07 Portabilidad | — | — | OE-5 | infra | — | `docker-compose.local.yml` y `docker-compose.production.yml` | El entorno completo se levanta desde cero con Docker | — | Implementado en local · el despliegue en un servidor no se ha probado |
| RNF-09 Documentación técnica | — | — | OE-5 | docs | — | README y `CLAUDE.md` de cada repositorio · `firma_sandbox/README.md` | — | Esta documentación | En desarrollo · falta el manual técnico de despliegue y operación |
| RNF-10 Rendimiento | HU-46 | — | OE-5 | — | Pendiente | Pendiente | Pendiente | — | Propuesto |
| RS-01 Acceso individual | HU-01 | — | OE-2 | core | Backend #14 | Backend PR #15 | Pruebas de inicio de sesión del personal y del cliente | — | Implementado · cuentas individuales; sin cuentas compartidas |
| RS-02 Control de permisos | HU-04 | UC-10 | OE-2 | core | Backend #14 | Backend PR #15 y siguientes | Permisos probados en cada API; la suite completa por rol sigue pendiente (HU-45) | — | Implementado parcial |
| RS-04 Registro de accesos | HU-30 | UC-13 | OE-2 | core | Backend #61 | Backend PR #62, abierto | Ingreso, ingreso fallido y cierre de sesión (personal, portal y admin); descarga de documentos; acceso a la cédula del autorizante | — | Implementado en el backend · PR #62 abierto |
| RS-08 Datos reales fuera del repositorio | — | — | OE-2 | — | — | `DOD-001` | Revisión en PR | `DOD-001` | Vigente |
| RS-09 Anonimización en desarrollo | — | — | OE-2 | migracion | — | Comandos `seed_demo_*` del backend · plantillas anonimizadas en `13_plantillas` | — | `13_plantillas/README.md` | Implementado parcial · desarrollo con datos ficticios; falta la anonimización de la planilla para la migración |
| IN-01 Firma electrónica | HU-34, HU-35, HU-37 | UC-16 | OE-4 | integrations | Backend #66, #67 · Frontend #26 | Backend PR #68 · Frontend PR #27 | 685 pruebas del backend en `main` (48 nuevas), 62 del PSC de pruebas y la prueba de punta a punta con tres firmantes | — | Implementado contra el PSC de pruebas, sin validez legal (AD-16) · adaptador de tufirma.digital pendiente (TA-07) |
| IN-02 Pasarela de pago | HU-38, HU-39, HU-62 | UC-17 | OE-4 | integrations | Backend #23, #27, #29 | Backend #25, #26, #28, #30 | Adaptadores probados con transporte simulado y pagos de prueba | — | Implementado con Klap y Flow · decisión de Easy Office pendiente |

---

## Cobertura por objetivo

| Objetivo | Requerimientos | Historias | Estado al 27-10-2026 |
|---|---|---|---|
| OE-1 | Todos, mediante `MRQ-001 v2` y `RN-001 v2` | — | Validado en la reunión del 08-10-2026 (`ACTA-002`); quedan temas abiertos con Easy Office |
| OE-2 | RF-01 a RF-18, RF-42 a RF-44, RF-50, RNF-01, RNF-03, RNF-05, RS-01 a RS-09 | HU-01 a HU-13, HU-30 a HU-33, HU-47 a HU-53, HU-64 | En desarrollo: acceso, roles, clientes, contratos y dashboard implementados; auditoría en el PR #62, abierto; vencimientos, alertas y migración pendientes |
| OE-3 | RF-20, RF-33 a RF-37, RF-41, RNF-04 | HU-14 a HU-18, HU-20, HU-25 | En desarrollo: dos tipos de trámite y dos documentos por configuración, con nivel de firma por tipo; falta la pantalla de configuración y cada plantilla nueva aún requiere código |
| OE-4 | RF-19 a RF-32, RF-38 a RF-40, IN-01, IN-02 | HU-19, HU-21 a HU-29, HU-34 a HU-40, HU-54 a HU-57, HU-62, HU-63 | Implementado de punta a punta en ambiente de pruebas: catálogo, formulario, documento, pago con Klap o Flow y firma asistida o avanzada contra el PSC de pruebas; faltan el seguimiento del vencimiento y el proveedor de firma real |
| OE-5 | RNF-07, RNF-09, RNF-10, RNF-11 | HU-43 a HU-46 | En desarrollo: 685 pruebas del backend en `main` y CI en el backend; faltan pruebas de rendimiento y de seguridad completas, CI del frontend, despliegue y manual técnico |

---

## Requerimientos sin historia asociada

Se registran para que la brecha sea visible, no para disimularla.

| Requerimiento | Motivo |
|---|---|
| RF-42 Base de datos centralizada | Es una condición del sistema, se satisface con el modelo de datos completo |
| RNF-01, RNF-03, RNF-05, RNF-07, RNF-09, RNF-11 | Atributos de calidad transversales, no funcionalidades. Se verifican en el plan de pruebas y en el despliegue |
| RS-03, RS-05, RS-06, RS-07 | Transversales. Se incorporan en la Definition of Done y en las pruebas de seguridad |
| RE-01 a RE-07 | Restricciones, no funcionalidades |
| IN-03 a IN-06 | Fuera del MVP |

---

## Cómo se mantiene

1. Al crear una historia en GitHub Projects, se registra el número de issue.
2. Los commits y Pull Requests referencian el issue, de modo que la columna
   *Commit / PR* se completa desde el historial del repositorio.
3. Al escribir la prueba que cubre la historia, se anota su identificador.
4. Al cerrar la historia, la columna *Evidencia* apunta al artefacto que la
   demuestra.

El estado avanza así: **Especificado → En desarrollo → Implementado → Verificado
→ Validado con la contraparte.**

Los identificadores provienen de `CAT-001` y no se reutilizan.

---

## Historial de versiones

| Versión | Fecha | Cambios |
|---|---|---|
| 1.0 | 10-09-2026 | Versión inicial |
| 2.0 | 24-09-2026 | Reconstruida contra `MRQ-001 v2` y `CAT-001`; incorporada la columna de caso de uso; agregados los requerimientos nuevos y la tabla de requerimientos sin historia |
| 2.1 | 09-10-2026 | Filas de RF-45, RF-46 y RNF-12; RF-27 especificado; HU-60 asociada a RF-07 (`ACTA-002`) |
| 2.2 | 12-10-2026 | Issues y PR de la cuenta del cliente y de los pagos (backend #22 a #30, frontend #6 a #12); filas de RF-48 a RF-50 |
| 2.3 | 14-10-2026 | Issues #35 a #39 (HU-08, HU-60, HU-65, HU-66 y HU-23) en RF-05, RF-07, RF-19 y RF-24; todas en ramas locales, sin PR |
| 2.4 | 17-10-2026 | Domicilio tributario de punta a punta (issue #50, PR #51) en RF-20, RF-21, RF-24, RF-26, RF-27, RF-37, RF-38 y RF-47; prueba técnica #49 en RF-24 |
| 2.5 | 17-10-2026 | Domicilio tributario fusionado: backend PR #51 y frontend PR #18 (issue #17) en RF-20, RF-21, RF-24, RF-26, RF-27, RF-31, RF-37, RF-38 y RF-47 |
| 2.6 | 17-10-2026 | RF-01 y RF-45: corrección de la sesión del panel de administración (PR #52, abierto); RF-01 con su issue y PR originales (#14, PR #15) |
| 2.7 | 17-10-2026 | RF-01 y RF-45: corrección del cambio de sesión entre personal y cliente (frontend, issues #2 y #6, PR #19, abierto) |
| 2.8 | 18-10-2026 | RF-14 con datos reales (backend #53 y frontend #20, en desarrollo); RF-47 con el desglose por motivo en el dashboard; RF-01 y RF-45 cerrados (PR #52 y #19 fusionados) |
| 2.9 | 20-10-2026 | Estados al día: RF-05 (PR #40), RF-07 (PR #45), RF-19 (PR #44, #48 y frontend #16) y RF-14 (PR #54 y #21) implementados; IN-02 con el PR #30 fusionado |
| 2.10 | 21-10-2026 | HU-15, bodega virtual con `PLT-02` (backend #59 y PR #60, frontend #22 y PR #23, abiertos) en RF-19, RF-24, RF-33, RF-34 y RF-36; cobertura de OE-3 |
| 2.11 | 22-10-2026 | HU-15, bodega virtual: backend PR #60 y frontend PR #23 fusionados el 21-10-2026 (RF-24, RF-33, RF-34, RF-36 y OE-3). RF-06, RF-08, RF-09 y RF-10 al día con lo que entregó el PR #18 del backend y el #5 del frontend (10-10-2026), que no se había registrado |
| 2.12 | 22-10-2026 | Auditoría (HU-30 y API de HU-31) implementada en el backend, en la rama `feature/HU-30-auditoria`, issue #61 y PR #62 abierto: RF-15, RF-16, RF-17, RS-04 y el historial de RF-10 |
| 2.13 | 27-10-2026 | Firma avanzada mediante un proveedor (backend issues #66 y #67, PR #68; frontend issue #26, PR #27): IN-01, RF-25, RF-31, RF-39 y RF-40 |
| 2.14 | 27-10-2026 | Revisión de estados contra el código: RF-02, RF-03, RF-04, RF-11, RF-18, RF-20, RF-28 a RF-30, RF-32, RF-35, RF-41, RF-42, RNF-01, RNF-04, RNF-05, RNF-07, RNF-09, RNF-12, RS-01, RS-02 y RS-09; cobertura por objetivo al 27-10-2026; definición de los estados |
