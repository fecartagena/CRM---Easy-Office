# Matriz de trazabilidad

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | TRZ-001 · Matriz de trazabilidad |
| **Versión** | 2.0 |
| **Fecha** | 24 de septiembre de 2026 |
| **Reemplaza a** | Versión 1.0 (10-09-2026) |
| **Motivo** | La versión 1.0 referenciaba los identificadores de `MRQ-001 v1.0`, que la versión 2.0 reutilizó para requerimientos distintos. Esta versión está reconstruida contra el catálogo vigente `CAT-001` |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## Propósito

Demostrar la cadena completa del proyecto:

**Requerimiento → Historia de usuario → Desarrollo → Prueba → Evidencia**

A la fecha de esta versión están poblados los tres primeros niveles. Las columnas
de issue, commit, prueba y evidencia se completan a medida que avanza el
desarrollo, de modo que la matriz registra el avance real del proyecto sprint a
sprint.

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
| RF-01 Autenticación individual | HU-01 | — | OE-2 | core | Pendiente | Pendiente | Pendiente | — | Especificado |
| RF-02 Permisos por roles | HU-04 | UC-10 | OE-2 | core | Pendiente | Pendiente | HU-45 | — | Especificado |
| RF-03 Administración de usuarios | HU-02 | UC-10 | OE-2 | core | Pendiente | Pendiente | Pendiente | — | Especificado |
| RF-04 Restricción de eliminación | HU-05 | UC-10 | OE-2 | core | Pendiente | Pendiente | HU-45 | — | Especificado |
| RF-05 Formulario de ingreso | HU-06 | UC-04 | OE-2 | clientes | Pendiente | Pendiente | Pendiente | — | Especificado |
| RF-06 Validación previa al guardado | HU-52 | UC-04 | OE-2 | clientes | Pendiente | Pendiente | Pendiente | — | Especificado |
| RF-07 Identificador único | HU-51 | UC-04 | OE-2 | clientes | Pendiente | Pendiente | Pendiente | — | Especificado |
| RF-08 Búsqueda de clientes | HU-47 | UC-05 | OE-2 | clientes | Pendiente | Pendiente | Pendiente | — | Especificado |
| RF-09 Filtros de listado | HU-50 | UC-05 | OE-2 | clientes | Pendiente | Pendiente | Pendiente | — | Especificado |
| RF-10 Ficha del cliente | HU-49 | UC-05 | OE-2 | clientes | Pendiente | Pendiente | Pendiente | Falta pantalla en prototipo | Especificado |
| RF-11 Gestión de servicios contratados | HU-48 | UC-06 | OE-2 | servicios | Pendiente | Pendiente | Pendiente | Falta pantalla en prototipo | Especificado |
| RF-12 Detección de vencimientos | HU-53 | UC-14 | OE-4 | servicios | Pendiente | Pendiente | Pendiente | — | Especificado |
| RF-13 Alertas configurables | HU-42 | UC-09 | OE-4 | servicios | Pendiente | Pendiente | Pendiente | Falta pantalla en prototipo | Especificado |
| RF-14 Dashboard | HU-41 | UC-08 | OE-2 | backoffice | Pendiente | Pendiente | Pendiente | Prototipo pantalla 7 | Especificado |
| RF-15 Registro de auditoría | HU-30 | UC-13 | OE-2 | core | Pendiente | Pendiente | Pendiente | Prototipo pantalla 8 | Especificado |
| RF-16 Protección del historial | HU-30 | UC-13 | OE-2 | core | Pendiente | Pendiente | Pendiente | — | Especificado |
| RF-17 Consulta de auditoría | HU-31 | UC-13 | OE-2 | core | Pendiente | Pendiente | Pendiente | Prototipo pantalla 8 | Especificado |
| RF-18 Procesos asistidos | HU-13 | UC-07 | OE-2 | tramites | Pendiente | Pendiente | Pendiente | — | Especificado |

## Matriz · Portal de contratación

| Requerimiento | Historia | Caso de uso | Objetivo | Módulo | Issue | Commit / PR | Prueba | Evidencia | Estado |
|---|---|---|---|---|---|---|---|---|---|
| RF-19 Catálogo de servicios | HU-23 | UC-01 | OE-4 | portal | Pendiente | Pendiente | Pendiente | Prototipo pantalla 1 | Especificado |
| RF-20 Formularios dinámicos | HU-24 | UC-01 | OE-3 | portal | Pendiente | Pendiente | Pendiente | Prototipo pantalla 2 | Especificado |
| RF-21 Revisión y confirmación | HU-26, HU-27 | UC-01 | OE-4 | portal | Pendiente | Pendiente | Pendiente | Prototipo pantalla 3 | Especificado |
| RF-22 Pago en línea | HU-39 | UC-17 | OE-4 | integrations | Pendiente | Pendiente | Pendiente | Prototipo pantalla 4 | Especificado · proveedor pendiente |
| RF-23 Verificación del pago | HU-57 | UC-17 | OE-4 | integrations | Pendiente | Pendiente | Pendiente | — | Especificado |
| RF-24 Generación del documento | HU-19 | UC-15 | OE-4 | documentos | Pendiente | Pendiente | HU-43 | — | Especificado |
| RF-25 Envío a firma | HU-35 | UC-16 | OE-4 | integrations | Pendiente | Pendiente | Pendiente | Prototipo pantalla 5 | Especificado · credenciales pendientes |
| RF-26 Entrega del documento final | HU-29 | UC-03 | OE-4 | portal | Pendiente | Pendiente | Pendiente | Prototipo pantalla 6 | Especificado |
| RF-27 Seguimiento del estado | HU-28 | UC-02 | OE-4 | portal | Pendiente | Pendiente | Pendiente | Prototipo pantallas 5 y 6 | Propuesto |

## Matriz · Integración portal y CRM

| Requerimiento | Historia | Caso de uso | Objetivo | Módulo | Issue | Commit / PR | Prueba | Evidencia | Estado |
|---|---|---|---|---|---|---|---|---|---|
| RF-28 Registro automático de la operación | HU-54 | UC-01 | OE-4 | tramites | Pendiente | Pendiente | HU-44 | — | Especificado |
| RF-29 Creación o actualización del cliente | HU-54 | UC-01 | OE-4 | clientes | Pendiente | Pendiente | HU-44 | — | Especificado |
| RF-30 Registro del servicio contratado | HU-55 | UC-01 | OE-4 | servicios | Pendiente | Pendiente | HU-44 | — | Especificado |
| RF-31 Asociación del documento firmado | HU-56 | UC-16 | OE-4 | documentos | Pendiente | Pendiente | Pendiente | — | Especificado |
| RF-32 Inicio del seguimiento | HU-55 | UC-01 | OE-4 | servicios | Pendiente | Pendiente | Pendiente | — | Especificado |

## Matriz · Motor configurable y documentos

| Requerimiento | Historia | Caso de uso | Objetivo | Módulo | Issue | Commit / PR | Prueba | Evidencia | Estado |
|---|---|---|---|---|---|---|---|---|---|
| RF-33 Tipos de trámite configurables | HU-14, HU-16 | UC-11 | OE-3 | tramites | Pendiente | Pendiente | Pendiente | Falta pantalla en prototipo | Especificado |
| RF-34 Incorporación modular de documentos | HU-15 | UC-11 | OE-3 | documentos | Pendiente | Pendiente | Pendiente | — | Especificado |
| RF-35 Incorporación de nuevos servicios | HU-14 | UC-11 | OE-3 | servicios | Pendiente | Pendiente | Pendiente | — | Especificado |
| RF-36 Sistema de plantillas | HU-18 | UC-12 | OE-3 | documentos | Pendiente | Pendiente | Pendiente | Falta pantalla en prototipo | Especificado |
| RF-37 Versionado de plantillas | HU-18, HU-20 | UC-12 | OE-3 | documentos | Pendiente | Pendiente | Pendiente | Prototipo pantalla 3 | Propuesto |
| RF-38 Copia congelada de los datos | HU-21 | UC-15 | OE-4 | documentos | Pendiente | Pendiente | HU-43 | — | Propuesto |
| RF-39 Verificación de integridad | HU-22 | UC-16 | OE-4 | documentos | Pendiente | Pendiente | Pendiente | — | Propuesto |
| RF-40 Múltiples firmantes | HU-36 | UC-16 | OE-4 | documentos | Pendiente | Pendiente | Pendiente | Prototipo pantallas 5 y 8 | Especificado |
| RF-41 Máquina de estados | HU-12, HU-17 | UC-11 | OE-3 | tramites | Pendiente | Pendiente | Pendiente | — | Propuesto |

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
| RS-01 Acceso individual | HU-01 | — | OE-2 | core | Pendiente | Pendiente | Pendiente | — | Especificado |
| RS-02 Control de permisos | HU-04 | UC-10 | OE-2 | core | Pendiente | Pendiente | HU-45 | — | Especificado |
| RS-04 Registro de accesos | HU-30 | UC-13 | OE-2 | core | Pendiente | Pendiente | Pendiente | — | Especificado |
| RS-08 Datos reales fuera del repositorio | — | — | OE-2 | — | — | `DOD-001` | Revisión en PR | `DOD-001` | Vigente |
| RS-09 Anonimización en desarrollo | — | — | OE-2 | migracion | Pendiente | Pendiente | Pendiente | — | Propuesto |
| IN-01 Firma electrónica | HU-34, HU-35, HU-37 | UC-16 | OE-4 | integrations | Pendiente | Pendiente | Pendiente | — | Dependencia externa |
| IN-02 Pasarela de pago | HU-38, HU-39 | UC-17 | OE-4 | integrations | Pendiente | Pendiente | Pendiente | — | Dependencia externa |

---

## Cobertura por objetivo

| Objetivo | Requerimientos | Historias | Estado al 24-09-2026 |
|---|---|---|---|
| OE-1 | Todos, mediante `MRQ-001 v2` y `RN-001 v2` | — | En ejecución. Cierra con la validación pendiente |
| OE-2 | RF-01 a RF-18, RF-42 a RF-44, RNF-01, RNF-03, RNF-05, RS-01 a RS-09 | HU-01 a HU-13, HU-30 a HU-33, HU-47 a HU-53 | Especificado, sin implementación |
| OE-3 | RF-20, RF-33 a RF-37, RF-41, RNF-04 | HU-14 a HU-18, HU-20, HU-25 | Especificado, sin implementación |
| OE-4 | RF-19 a RF-32, RF-38 a RF-40, IN-01, IN-02 | HU-19, HU-21 a HU-29, HU-34 a HU-40, HU-54 a HU-57 | Especificado; integraciones con dependencias externas |
| OE-5 | RNF-07, RNF-09, RNF-10, RNF-11 | HU-43 a HU-46 | Planificado |

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
