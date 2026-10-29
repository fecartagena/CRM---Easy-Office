# Matriz de trazabilidad

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | TRZ-001 · Matriz de trazabilidad |
| **Versión** | 2.13 |
| **Fecha** | 28 de octubre de 2026 |
| **Reemplaza a** | Versión 1.0 (10-09-2026) |
| **Motivo** | La versión 1.0 referenciaba los identificadores de `MRQ-001 v1.0`, que la versión 2.0 reutilizó para requerimientos distintos. Esta versión está reconstruida contra el catálogo vigente `CAT-001` |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## Propósito

Demostrar la cadena completa del proyecto:

**Requerimiento → Historia de usuario → Desarrollo → Prueba → Evidencia**

A la fecha de esta versión están poblados los tres primeros niveles y, para lo ya
fusionado, el issue, el PR y la prueba. La columna *Evidencia* sigue pendiente:
ninguna fila alcanza todavía el estado *Verificado*. Los prototipos que figuran en
esa columna muestran el diseño, no el funcionamiento del sistema. Ver «Evidencia
pendiente».

El estado de cada fila se contrasta con el código de la rama principal de cada
repositorio (ver «Registro de fusiones»). Los estados *Implementado* de esta
matriz significan que el código existe y está fusionado; no significan que Easy
Office lo haya validado.

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
| RF-02 Permisos por roles | HU-04 | UC-10 | OE-2 | core | Pendiente | Backend PR #15 | Roles Administrador y Ejecutivo sembrados como grupos; un usuario tiene un solo rol. Falta la suite por rol y por recurso (HU-45) | — | Implementado parcial · roles como grupos (`SPR-001`) |
| RF-03 Administración de usuarios | HU-02 | UC-10 | OE-2 | core | Pendiente | Pendiente | Pendiente | — | Implementado parcial · se hace desde el panel de administración, sin pantalla propia `[CONFIRMAR si basta para Easy Office]` (`SPR-001`) |
| RF-04 Restricción de eliminación | HU-05 | UC-10 | OE-2 | core | #17 | Backend PR #18 · bloqueo en el panel (14-10) | `test_put_and_delete_are_not_allowed` (API de clientes) y `test_parties_cannot_be_deleted_from_the_admin` (panel). Falta la prueba con el rol Ejecutivo (HU-45) | — | Implementado · la API de clientes no admite eliminar |
| RF-05 Formulario de ingreso | HU-06, HU-08 | UC-04 | OE-2 | clientes | #17 · #35 (HU-08) | PR #18 · PR #40 (HU-08) | Pruebas de la API de clientes y de los datos civiles y la personería | — | Implementado |
| RF-06 Validación previa al guardado | HU-52 | UC-04 | OE-2 | clientes | #17 | Backend PR #18 · Frontend PR #5 | Pruebas de la API de clientes: RUT con dígito verificador y RUT ya registrado | — | Implementado parcial · duplicidad por RUT; sin coincidencia aproximada por nombre |
| RF-07 Identificador único y folio de contrato | HU-51, HU-60 | UC-04 | OE-2 | clientes, contratos | #17 · #38 (HU-60) | PR #18 (código del cliente) · PR #45 (HU-60) | Folio común entre sucursales y marcas, reinicio anual, ancho configurable, creación concurrente, alias `folio` | — | Implementado |
| RF-08 Búsqueda de clientes | HU-47 | UC-05 | OE-2 | clientes | #17 | Backend PR #18 · Frontend PR #5 | Búsqueda por cada palabra, RUT con o sin puntos; pruebas de `ListaClientes` | — | Implementado parcial · RUT, nombre, razón social, nombre de fantasía, correo y teléfono; faltan folio y servicio contratado |
| RF-09 Filtros de listado | HU-50 | UC-05 | OE-2 | clientes | #17 | Backend PR #18 · Frontend PR #5 | Filtros por tipo (persona o empresa) y por estado activo | — | Implementado parcial · faltan fechas y ejecutivo |
| RF-10 Ficha del cliente | HU-49 | UC-05 | OE-2 | clientes | #17 | Backend PR #18 · Frontend PR #5 | Pruebas de `FichaCliente` | — | Implementado parcial · datos del cliente y representantes legales; servicios y documentos pendientes (HU-48); el historial ya tiene API (`/api/auditoria/historial/`, sin fusionar) y falta la pantalla (HU-31) |
| RF-11 Gestión de servicios contratados | HU-48 | UC-06 | OE-2 | servicios | #38 | Backend PR #45 | Pruebas de contratos y folio (RF-07) | Falta pantalla en prototipo | En desarrollo · modelo `ServicioContratado`, API de contratos y panel de administración; la ficha del cliente muestra un marcador y falta la pantalla de gestión |
| RF-12 Detección de vencimientos | HU-53 | UC-14 | OE-4 | servicios | Pendiente | Pendiente | Pendiente | — | Especificado |
| RF-13 Alertas configurables | HU-42 | UC-09 | OE-4 | servicios | Pendiente | Pendiente | Pendiente | Falta pantalla en prototipo | Especificado |
| RF-14 Dashboard | HU-41 | UC-08 | OE-2 | panel, backoffice | Backend #53 · Frontend #20 | Backend PR #54 · Frontend PR #21 | Indicadores con datos reales, fecha de referencia, acceso por rol (Administrador y Ejecutivo), Excel sin valores de ejemplo, número de consultas acotado (545 pruebas backend); panel sin rótulos de ejemplo, desglose por motivo, nota de ventas y estado vacío (131 pruebas frontend) | Prototipo pantalla 7 | Implementado |
| RF-47 Motivo de pendiente | HU-61 | UC-11 | OE-2 | tramites | Backend #50 · Frontend #17 · Backend #53 y Frontend #20 (dashboard) | Backend PR #51 · Frontend PR #18 | Motivo solo mientras está Pendiente; 123 pruebas frontend; pendientes por motivo en el dashboard | — | Implementado · motivos provisionales (TA-13) |
| RF-50 Consulta de ventas | HU-64 | UC-17 | OE-2 | ventas, backoffice | Frontend #7 | Frontend #9, #10 | Pruebas de ListaVentas y DetalleVenta | — | Implementado en pruebas |
| RF-15 Registro de auditoría | HU-30 | UC-13 | OE-2 | core | Sin issue todavía | Backend, rama `feature/HU-30-auditoria` publicada en GitHub (último cambio c2bbc1c, 22-10-2026), sin PR ni fusión | `core/tests/test_auditoria_*.py`: creación, modificación y eliminación con usuario, IP y origen desde la API, el portal, el admin y Celery; sin fila si no hay cambios; campos excluidos (59 pruebas nuevas, 696 en total) | Prototipo pantalla 8 (interfaz pendiente, HU-31) | Implementado en el backend · sin fusionar |
| RF-16 Protección del historial | HU-30 | UC-13 | OE-2 | core | Sin issue todavía | Backend, rama `feature/HU-30-auditoria` publicada en GitHub (último cambio c2bbc1c, 22-10-2026), sin PR ni fusión | `UPDATE`, `DELETE` y `TRUNCATE` directos fallan en la base; el modelo y el admin no permiten modificar ni eliminar | — | Implementado en el backend · sin fusionar |
| RF-17 Consulta de auditoría | HU-31 | UC-13 | OE-2 | core | Sin issue todavía | Backend, rama `feature/HU-30-auditoria` publicada en GitHub (último cambio c2bbc1c, 22-10-2026), sin PR ni fusión | Matriz de acceso (Administrador 200; Ejecutivo, cliente y anónimo 403), filtros, historial agrupado y número de consultas acotado | Prototipo pantalla 8 (pantallas pendientes) | API implementada · pantallas pendientes |
| RF-18 Procesos asistidos | HU-13 | UC-07 | OE-2 | tramites | Pendiente | Pendiente | Pendiente | — | Especificado |

## Matriz · Portal de contratación

| Requerimiento | Historia | Caso de uso | Objetivo | Módulo | Issue | Commit / PR | Prueba | Evidencia | Estado |
|---|---|---|---|---|---|---|---|---|---|
| RF-19 Catálogo de servicios | HU-23 | UC-01 | OE-4 | portal, ventas | Backend #39 | Backend PR #44 (catálogo del formulario) y PR #48 (servicios visibles y contratables) · Frontend PR #16 (landing desde la API) | Semillas e idempotencia; servicio contratable con precio; visibles no contratables; la landing lee la API | Prototipo pantalla 1 | Implementado · precios sin confirmar (IVA) · bodega vendible en el portal con tres planes (PR #60, fusionado) |
| RF-20 Formularios dinámicos | HU-24 | UC-01 | OE-3 | portal, tramites | Backend #50 · Frontend #17 | Backend PR #51 · Frontend PR #18 | 498 pruebas backend; flujo verificado en el navegador con datos ficticios; 123 pruebas frontend | Prototipo pantalla 2 | Implementado · formulario del domicilio tributario; el motor genérico sigue pendiente |
| RF-21 Revisión y confirmación | HU-26, HU-27 | UC-01 | OE-4 | portal, tramites | Backend #50 · Frontend #17 | Backend PR #51 · Frontend PR #18 | Vista previa del PDF real, edición con versión nueva y confirmación; 123 pruebas frontend | Prototipo pantalla 3 | Implementado |
| RF-22 Pago en línea | HU-39, HU-62 | UC-17 | OE-4 | integrations, ventas, portal | Backend #23, #27 · Frontend #7, #11 | Backend #25, #26, #28 · Frontend #9, #10, #12 | Pruebas unitarias y de integración (214 backend, 97 frontend); pagos de prueba con Klap y Flow en sandbox | Prototipo pantalla 4 | Implementado en pruebas · proveedor y precios por confirmar |
| RF-23 Verificación del pago | HU-57 | UC-17 | OE-4 | integrations, ventas | Backend #23, #27 | Backend #25, #26, #28 | Pruebas de webhooks (duplicados, firma falsa, monto distinto, rechazo tardío) y de sincronización | — | Implementado en pruebas |
| RF-24 Generación del documento | HU-19, HU-65, HU-66 | UC-15 | OE-4 | documentos, configuracion, inmuebles | #49 (prueba técnica) · #50 · #36 · #37 | Backend PR #51 · HU-65 y HU-66 fusionadas | Cada campo de `PLT-01` desde una sola fuente; sin campos sin llenar; sucursal incompleta rechazada | — | Implementado · `PLT-01` · `PLT-02` (backend #59, PR #60 fusionado) |
| RF-25 Envío a firma | HU-35 | UC-16 | OE-4 | integrations | Backend #66 · Frontend #26 | Backend PR #68 · Frontend PR #27 (fusionados el 27-10-2026) | Pruebas del prestador de pruebas (`firma_sandbox/tests`); conteo por registrar | Prototipo pantalla 5 | Implementado en pruebas · prestador de pruebas sin validez legal; tufirma.digital pendiente (TA-07) |
| RF-26 Entrega del documento final | HU-29 | UC-03 | OE-4 | portal, documentos | Backend #50 · Frontend #17 | Backend PR #51 · Frontend PR #18 | Descarga del PDF firmado, sin caché; subida del firmado por un ejecutivo (firma asistida); 123 pruebas frontend | Prototipo pantalla 6 | Implementado |
| RF-27 Seguimiento del estado | HU-28 | UC-02 | OE-4 | portal, tramites | Backend #50 · Frontend #17 | Backend PR #51 · Frontend PR #18 | Estados y motivo de pendiente en el portal; 123 pruebas frontend | Prototipo pantallas 5 y 6 | Implementado |
| RF-45 Cuenta del cliente | HU-03 | UC-01 | OE-4 | clientes, portal | Backend #22 · Frontend #6 | Backend #24 · Frontend #8 · correcciones de sesión: backend PR #52 y frontend PR #19, fusionados | Pruebas de registro, vinculación por RUT y guardias de ruta | — | Implementado |
| RF-46 Acceso desde el sitio web | HU-58 | UC-01 | OE-4 | portal | Pendiente | Backend, rama `feature/integracion-sitio-web` (sitio simulado; último cambio 27-09-2026), sin PR ni fusión | Pendiente | — | Especificado |
| RF-48 Elección del medio de pago | HU-62 | UC-17 | OE-4 | portal | Frontend #11 | Frontend #12 | Pruebas del selector y del checkout de Klap y Flow | — | Implementado en pruebas |
| RF-49 Compras del cliente | HU-63 | UC-01 | OE-4 | portal | Frontend #7 | Frontend #9, #10 | Pruebas de pantallas de pago | — | Implementado en pruebas |
| RNF-12 Identidad visual | HU-59 | — | OE-4 | frontend | Frontend #24 | Frontend PR #25 (fusionado el 22-10-2026) | Pruebas de los tokens de marca y del contraste de cada par de colores | — | Implementado · paleta oficial por confirmar (TA-06) |

## Matriz · Integración portal y CRM

| Requerimiento | Historia | Caso de uso | Objetivo | Módulo | Issue | Commit / PR | Prueba | Evidencia | Estado |
|---|---|---|---|---|---|---|---|---|---|
| RF-28 Registro automático de la operación | HU-54 | UC-01 | OE-4 | tramites | Pendiente | Pendiente | HU-44 | — | Especificado |
| RF-29 Creación o actualización del cliente | HU-54 | UC-01 | OE-4 | clientes | Pendiente | Pendiente | HU-44 | — | Especificado |
| RF-30 Registro del servicio contratado | HU-55 | UC-01 | OE-4 | servicios | Pendiente | Pendiente | HU-44 | — | Especificado |
| RF-31 Asociación del documento firmado | HU-56 | UC-16 | OE-4 | documentos | Backend #50 · Frontend #17 | Backend PR #51 · Frontend PR #18 | Subida del PDF firmado en el backoffice, con su permiso y SHA-256 | Prototipo pantalla 8 | Implementado como firma asistida; la firma con proveedor sigue pendiente (IN-01) |
| RF-32 Inicio del seguimiento | HU-55 | UC-01 | OE-4 | servicios | Pendiente | Pendiente | Pendiente | — | Especificado |

## Matriz · Motor configurable y documentos

| Requerimiento | Historia | Caso de uso | Objetivo | Módulo | Issue | Commit / PR | Prueba | Evidencia | Estado |
|---|---|---|---|---|---|---|---|---|---|
| RF-33 Tipos de trámite configurables | HU-14, HU-15, HU-16 | UC-11 | OE-3 | tramites | Backend #59 · Frontend #22 (HU-15) | Backend PR #60 · Frontend PR #23, fusionados el 21-10-2026 | Segundo tipo de trámite (`bodega`) sobre las mismas funciones genéricas que el domicilio; claves de `TipoTramite.configuracion` documentadas en el modelo; botones "Nuevo trámite" según los tipos iniciables; 637 pruebas del backend y 150 del frontend | Falta pantalla en prototipo · `docs/variation-points.md` del backend | En desarrollo · dos tipos de trámite como datos; sin pantalla de configuración ni motor genérico |
| RF-34 Incorporación modular de documentos | HU-15 | UC-11 | OE-3 | documentos | Backend #59 · Frontend #22 | Backend PR #60 · Frontend PR #23, fusionados el 21-10-2026 | Cada campo de `PLT-02` desde una sola fuente; duración del plan impresa en el documento; bodega o autorizante incompletos rechazados; formulario del portal y pantalla "Nuevo trámite" del backoffice (150 pruebas frontend) | `PLT-02` anonimizado en `13_plantillas` | En desarrollo · segundo documento (`PLT-02`); cada plantilla aún requiere su convertidor |
| RF-35 Incorporación de nuevos servicios | HU-14 | UC-11 | OE-3 | servicios | Pendiente | Pendiente | Pendiente | — | Especificado |
| RF-36 Sistema de plantillas | HU-15, HU-18 | UC-12 | OE-3 | documentos | Backend #59 | Backend PR #60, fusionado | Pasos de conversión compartidos entre `PLT-01` y `PLT-02`; comando `convertir_plantilla` | Falta pantalla en prototipo | En desarrollo · conversión común para dos plantillas; sin administración de plantillas |
| RF-37 Versionado de plantillas | HU-18, HU-20 | UC-12 | OE-3 | documentos | Backend #50 | Backend PR #51 | Versión inmutable una vez usada | Prototipo pantalla 3 | Implementado |
| RF-38 Copia congelada de los datos | HU-21 | UC-15 | OE-4 | documentos | Backend #50 | Backend PR #51 | HU-43 · copia congelada y SHA-256 por archivo | — | Implementado |
| RF-39 Verificación de integridad | HU-22 | UC-16 | OE-4 | documentos | Pendiente | Pendiente | Pendiente | — | Propuesto |
| RF-40 Múltiples firmantes | HU-36 | UC-16 | OE-4 | documentos | Backend #50 · Frontend #17 · Frontend #26 | Backend PR #51 · Frontend PR #18 · estado de cada firmante: Frontend PR #27 | Por registrar | Prototipo pantallas 5 y 8 | Implementado · el orden de las firmas es un supuesto (RN-39) |
| RF-41 Máquina de estados | HU-12, HU-17 | UC-11 | OE-3 | tramites | Backend #50 | Backend PR #51 | Por registrar (HU-12) | — | En desarrollo · tabla de transiciones en el código (`TRANSICIONES`), supuesto sin validar (`MOD-001` MD-17); falta definir estados por tipo de trámite (HU-17) |

## Matriz · Datos, calidad y entrega

| Requerimiento | Historia | Caso de uso | Objetivo | Módulo | Issue | Commit / PR | Prueba | Evidencia | Estado |
|---|---|---|---|---|---|---|---|---|---|
| RF-42 Base de datos centralizada | — | — | OE-2 | datos | Pendiente | Pendiente | Pendiente | `MOD-001` | Diseñado |
| RF-43 Migración desde Excel | HU-32 | — | OE-2 | migracion | Pendiente | Pendiente | Pendiente | — | Bloqueado por entrega de la planilla |
| RF-44 Reporte de inconsistencias | HU-33 | — | OE-2 | migracion | Pendiente | Pendiente | Pendiente | — | Propuesto |
| RNF-01 Persistencia relacional | — | — | OE-2 | datos | Pendiente | Pendiente | Pendiente | `REC-001` | Decidido |
| RNF-03 Escalabilidad de datos | — | — | OE-2 | datos | Pendiente | Pendiente | Pendiente | `MOD-001 §5` | Diseñado |
| RNF-04 Escalabilidad funcional | HU-14 | UC-11 | OE-3 | tramites | Pendiente | Pendiente | Pendiente | — | Especificado |
| RNF-05 Respaldo periódico | — | — | OE-5 | infra | Pendiente | Pendiente | Pendiente | `REC-001 §7` | Diseñado |
| RNF-07 Portabilidad | — | — | OE-5 | infra | Pendiente | Pendiente | Pendiente | — | Pendiente |
| RNF-09 Documentación técnica | — | — | OE-5 | docs | Pendiente | Pendiente | — | — | Pendiente |
| RNF-10 Rendimiento | HU-46 | — | OE-5 | — | Pendiente | Pendiente | Pendiente | — | Propuesto |
| RS-01 Acceso individual | HU-01 | — | OE-2 | core | Backend #14 · Frontend #2 | Backend PR #15 | Las de RF-01 | — | Implementado · ver RF-01 |
| RS-02 Control de permisos | HU-04 | UC-10 | OE-2 | core | Pendiente | Pendiente | HU-45 | — | Especificado |
| RS-04 Registro de accesos | HU-30 | UC-13 | OE-2 | core | Sin issue todavía | Backend, rama `feature/HU-30-auditoria` publicada en GitHub (último cambio c2bbc1c, 22-10-2026), sin PR ni fusión | Ingreso, ingreso fallido y cierre de sesión (personal, portal y admin); descarga de documentos; acceso a la cédula del autorizante | — | Implementado en el backend · sin fusionar |
| RS-08 Datos reales fuera del repositorio | — | — | OE-2 | — | — | `DOD-001` | Revisión en PR | `DOD-001` | Vigente |
| RS-09 Anonimización en desarrollo | — | — | OE-2 | migracion | Pendiente | Pendiente | Pendiente | — | Propuesto |
| IN-01 Firma electrónica | HU-34, HU-35, HU-37 | UC-16 | OE-4 | integrations | Backend #66 · Frontend #26 | Backend PR #68 · Frontend PR #27 | Interfaz con proveedor simulado y adaptador HTTP; conteo de pruebas por registrar | — | Implementado en pruebas con el prestador de pruebas · tufirma.digital: dependencia externa (TA-07) |
| IN-02 Pasarela de pago | HU-38, HU-39, HU-62 | UC-17 | OE-4 | integrations | Backend #23, #27, #29 | Backend #25, #26, #28, #30 | Adaptadores probados con transporte simulado y pagos de prueba | — | Implementado con Klap y Flow · decisión de Easy Office pendiente |
| RNF-02 Plataforma web | — | — | — | frontend | — | — | — | — | Cubierto · aplicación web (frontend React y API Django) |
| RNF-06 Usabilidad para no técnicos | — | — | — | frontend | Pendiente | Pendiente | Pendiente | — | Pendiente · se verifica en la aceptación con Easy Office |
| RNF-08 Mantenibilidad | HU-14, HU-15 | UC-11 | OE-3 | tramites | Backend #59 · Frontend #22 | Backend PR #60 · Frontend PR #23 | Ver RF-33 | — | En desarrollo · dos tipos de trámite y dos plantillas como datos; sin motor genérico |

---

## Cobertura por objetivo

Estado al 28-10-2026. Los conteos salen de la columna *Objetivo* de las matrices de este documento; RNF-02 y RNF-06 son transversales y no se asignan a un objetivo.

| Objetivo | Requerimientos | Implementado | En rama sin fusionar | Parcial o en desarrollo | Diseñado, decidido o vigente | Sin implementar | Situación |
|---|---|---|---|---|---|---|---|
| OE-1 | — | — | — | — | — | — | En ejecución. Las decisiones de `ACTA-002` siguen sujetas a la confirmación escrita de Easy Office |
| OE-2 | 28 | 8 | 4 | 7 | 4 | 5 | Autenticación, clientes, dashboard y motivo de pendiente implementados. Auditoría en rama sin fusionar. Migración desde Excel bloqueada por la planilla (TA-10) |
| OE-3 | 9 | 2 | 0 | 5 | 0 | 2 | Segundo tipo de trámite y segundo documento (`PLT-02`) implementados; motor genérico y pantalla de configuración pendientes |
| OE-4 | 25 | 17 | 0 | 0 | 0 | 8 | Domicilio tributario de punta a punta, cuenta del cliente, pagos con Klap y Flow y firma de pruebas implementados. Firma legal (tufirma.digital), alertas de vencimiento y acceso desde el sitio web pendientes |
| OE-5 | 4 | 0 | 0 | 0 | 1 | 3 | Pruebas de rendimiento y de seguridad, manual técnico y despliegue por hacer. El backend tiene CI; el frontend no |

*Implementado* incluye lo que funciona solo en ambiente de pruebas (pagos con Klap y Flow, firma de pruebas) y no equivale a validado por Easy Office.

---

## Requerimientos sin historia asociada

Se registran para que la brecha sea visible, no para disimularla.

| Requerimiento | Motivo |
|---|---|
| RF-42 Base de datos centralizada | Es una condición del sistema, se satisface con el modelo de datos completo |
| RNF-01, RNF-02, RNF-03, RNF-05, RNF-06, RNF-07, RNF-09, RNF-11 | Atributos de calidad transversales, no funcionalidades. Se verifican en el plan de pruebas y en el despliegue |
| RS-03, RS-05, RS-06, RS-07 | Transversales. Se incorporan en la Definition of Done y en las pruebas de seguridad |
| RE-01 a RE-07 | Restricciones, no funcionalidades |
| IN-03 a IN-06 | Fuera del MVP |

---

## Historias sin fila propia y fuentes divergentes

Se registran para que el desajuste entre `BKL-001` y esta matriz sea visible.
Todas las filas de esta sección son `[PROPUESTA – VALIDAR CON EL EQUIPO]`: no se
cambió ningún vínculo hasta que el equipo decida cuál de los dos documentos tiene
la razón.

**Historias del backlog que ninguna fila de la matriz lista en su columna *Historia*.**

| Historia | Situación | Propuesta |
|---|---|---|
| HU-07 Buscar un cliente y ver su ficha | El backlog cita RF-05. La función la cubren RF-08 (HU-47) y RF-10 (HU-49) | Listar HU-07 en RF-08 y RF-10, o fusionarla con HU-47 y HU-49 |
| HU-09 Vigencia de la representación legal | El backlog cita RN-03. `SPR-001` la marca *Parcial*: el modelo registra `vigente_desde` y `vigente_hasta` | Fila propia en la matriz CRM interno, como implementado parcial |
| HU-10 Crear un trámite y HU-11 Ver su estado e historial | El backlog cita RF-11 (servicios contratados), que no corresponde. El código las cubre en `tramites` y en la pantalla «Nuevo trámite» | Decidir el requerimiento (RF-18, RF-27 o RF-47) y agregarlas |
| HU-25 Validar los datos antes de continuar | El backlog cita RF-06, cuya fila lista solo HU-52 | Agregar HU-25 a la fila de RF-06 |
| HU-40 Registrar el pago de forma idempotente | El backlog cita RF-22. Las pruebas de webhooks duplicados están en RF-23 | Agregar HU-40 a RF-22 y RF-23 |
| HU-43, HU-44 y HU-45 (pruebas y calidad) | Aparecen en la columna *Prueba* como pruebas previstas, sin fila propia | Registrarlas como filas de calidad del objetivo OE-5, o declarar que son historias de verificación |

**Historias cuya fuente en `BKL-001` no coincide con la fila donde esta matriz las ubica.**

| Historia | Fuente en `BKL-001` | Fila de esta matriz | Propuesta |
|---|---|---|---|
| HU-02 | RF-01 | RF-03 | Corregir la fuente en el backlog |
| HU-05 | RS-02 | RF-04 | Agregar RF-04 a la fuente en el backlog |
| HU-17 | RF-33 | RF-41 | Agregar RF-41 a la fuente en el backlog |
| HU-24 | RF-19 | RF-20 | Agregar RF-20 a la fuente en el backlog |
| HU-37 | RF-25 | IN-01 | Decidir entre RF-25, RF-31 o IN-01 |
| HU-48 | RF-11 y RF-30 | RF-11 | Agregar HU-48 a la fila de RF-30 |
| HU-43 | RNF-06 | — | RNF-06 es *Usabilidad para no técnicos*, no pruebas. Corregir la fuente (por ejemplo `PLA-001` y `DOD-001`) |

---

## Evidencia pendiente

Los estados *Implementado* de esta matriz se apoyan en el código fusionado, no en
una evidencia reproducible. Falta registrar la evidencia de 37 filas: RF-01, RF-02, RF-03, RF-04, RF-05, RF-06, RF-07, RF-08, RF-09, RF-10, RF-14, RF-47, RF-50, RF-15, RF-16, RF-17, RF-19, RF-20, RF-21, RF-22, RF-23, RF-24, RF-25, RF-26, RF-27, RF-45, RF-48, RF-49, RNF-12, RF-31, RF-37, RF-38, RF-40, RS-01, RS-04, IN-01, IN-02.
En ellas la columna *Evidencia* está vacía o solo cita un prototipo.

Cada evidencia debe identificar:

| Campo | Qué registrar |
|---|---|
| Fecha | Cuándo se ejecutó |
| Commit | El commit probado |
| Entorno | Sistema operativo, versiones, base de datos, y si fue local, en CI o en un ambiente de pruebas |
| Resultado | Pruebas que pasan y que fallan, con las cifras |
| Enlace | La ejecución de CI, el reporte o la captura que lo respalda |

El formato completo está en `12_evidencias/README.md`.

**Los conteos de pruebas de la columna *Prueba* no son comparables entre filas.**
Cada cifra (por ejemplo 97, 123, 128, 131, 150, 214, 498, 545, 637 y 696) corresponde a la fecha de su PR y
no indica el commit ni el entorno. Las 696 pruebas incluyen las de la rama de
auditoría, que no está fusionada.

---

## Registro de fusiones

PR fusionados en la rama principal de cada repositorio. Fuente: el historial de
git, consultado el 28-10-2026 (backend `7869d0a`, frontend `f2e4758`). La fecha es la de
la fusión y el commit es el de fusión.

| Repositorio | PR | Rama | Fecha de fusión | Commit |
|---|---|---|---|---|
| Backend | #68 | `feature/IN-01-firma-sandbox` | 27-10-2026 | `7869d0a` |
| Backend | #60 | `feature/HU-15-plt02-bodega` | 21-10-2026 | `e73431e` |
| Backend | #54 | `feature/HU-41-dashboard-real` | 20-10-2026 | `5139f10` |
| Backend | #52 | `fix/sesion-admin-separada` | 17-10-2026 | `e50a5dc` |
| Backend | #51 | `feature/HU-24-domicilio-plt01` | 17-10-2026 | `8dba2a5` |
| Backend | #48 | `feature/HU-23-catalogo-visible-portal` | 15-10-2026 | `794a983` |
| Backend | #45 | `feature/HU-60-contrato-folio` | 14-10-2026 | `12c11c1` |
| Backend | #44 | `feature/HU-23-catalogo-servicios` | 14-10-2026 | `a9c9a96` |
| Backend | #43 | `feature/HU-65-config-empresa` | 14-10-2026 | `8ccfeeb` |
| Backend | #42 | `feature/HU-66-inmueble-autorizante` | 14-10-2026 | `adf0669` |
| Backend | #40 | `feature/HU-08-persona-datos-civiles` | 14-10-2026 | `66b20b9` |
| Backend | #32 | `feature/skill-exportar-cambios` | 14-10-2026 | `4f65088` |
| Backend | #30 | `feature/HU-39-tunel-diagnostico` | 12-10-2026 | `16cb97c` |
| Backend | #28 | `feature/HU-39-pago-flow` | 10-10-2026 | `964d70c` |
| Backend | #26 | `feature/HU-39-pago-klap` | 10-10-2026 | `9e41e82` |
| Backend | #24 | `feature/HU-03-cuentas-cliente` | 10-10-2026 | `2520fe3` |
| Backend | #18 | `feature/HU-06-gestion-clientes` | 10-10-2026 | `09d13d6` |
| Backend | #15 | `feature/HU-01-login` | 05-10-2026 | `d536f10` |
| Frontend | #27 | `feature/IN-01-firma-avanzada` | 27-10-2026 | `f2e4758` |
| Frontend | #25 | `feature/HU-59-identidad-visual` | 22-10-2026 | `77f13eb` |
| Frontend | #23 | `feature/HU-15-plt02-bodega` | 21-10-2026 | `3ab1398` |
| Frontend | #21 | `feature/HU-41-dashboard-real` | 20-10-2026 | `1f96fff` |
| Frontend | #19 | `fix/sesion-cambio-de-perfil` | 17-10-2026 | `103b7d9` |
| Frontend | #18 | `feature/HU-24-domicilio-plt01` | 17-10-2026 | `099b088` |
| Frontend | #16 | `feature/HU-23-catalogo-portal-api` | 15-10-2026 | `ed73773` |
| Frontend | #14 | `feature/skill-exportar-cambios` | 14-10-2026 | `97cb5ba` |
| Frontend | #12 | `feature/HU-39-pago-flow` | 10-10-2026 | `b442066` |
| Frontend | #10 | `feature/HU-39-pago-klap` | 10-10-2026 | `6d24116` |
| Frontend | #8 | `feature/HU-03-cuentas-cliente` | 10-10-2026 | `7e0d50a` |
| Frontend | #5 | `feature/HU-06-gestion-clientes` | 10-10-2026 | `1fa1cf4` |
| Frontend | #3 | `feature/HU-41-dashboard` | 05-10-2026 | `d18c72d` |

---

## Cómo se mantiene

1. Al crear una historia en GitHub Projects, se registra el número de issue.
2. Los commits y Pull Requests referencian el issue, de modo que la columna
   *Commit / PR* se completa desde el historial del repositorio.
3. Al escribir la prueba que cubre la historia, se anota su identificador.
4. Al cerrar la historia, la columna *Evidencia* apunta al artefacto que la
   demuestra, con fecha, commit, entorno y resultado (ver «Evidencia pendiente»).

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
| 2.12 | 22-10-2026 | Auditoría (HU-30 y API de HU-31) implementada en el backend, en la rama `feature/HU-30-auditoria`, sin issue ni PR todavía: RF-15, RF-16, RF-17, RS-04 y el historial de RF-10 |
| 2.13 | 28-10-2026 | Estados contrastados con el código de la rama principal: RNF-12 (frontend PR #25), RF-02, RF-03, RF-04, RS-01, RF-11, RF-25, IN-01, RF-40 y RF-41 actualizados; firma de pruebas (backend PR #68, frontend PR #27); rama de auditoría publicada. Filas nuevas de RNF-02, RNF-06 y RNF-08. «Cobertura por objetivo» recalculada. Nuevas secciones: historias sin fila y fuentes divergentes (propuestas), evidencia pendiente y registro de fusiones |
