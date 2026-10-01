# Catálogo de identificadores

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | CAT-001 · Catálogo de identificadores |
| **Versión** | 1.0 |
| **Fecha** | 24 de septiembre de 2026 |
| **Estado** | Vigente. Este documento es la fuente única de verdad de los identificadores del proyecto |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## 1. Por qué existe este documento

La versión 2.0 de `MRQ-001` reorganizó la matriz de requerimientos por áreas del
sistema e incorporó los requerimientos provenientes de las especificaciones
formales de Easy Office. En esa reorganización **se reutilizaron códigos para
requerimientos distintos**, sin actualizar los documentos que los referenciaban.

El resultado fue que el mismo código significaba cosas diferentes según el
documento:

| Código | En `MRQ-001 v2` | En `TRZ-001` y `BKL-001` (heredado de v1) |
|---|---|---|
| RF-03 | Administración de usuarios | Registro y consulta de clientes |
| RF-12 | Detección de vencimientos | Motor configurable de tipos de trámite |
| RF-14 | Dashboard de indicadores | Procesos asistidos |
| RF-25 | Envío al proceso de firma | Alertas de vencimiento y renovación |

Una historia de usuario podía aparecer vinculada a un requerimiento que no
implementa. Este documento corrige esa situación y establece la regla para que no
vuelva a ocurrir.

---

## 2. Regla de estabilidad

**Los identificadores de la versión 2.0 quedan congelados.**

- Un identificador asignado no se reutiliza jamás para otro contenido.
- Si un requerimiento se elimina, su código queda **retirado** y no se reasigna.
- Si un requerimiento cambia de redacción sin cambiar de objeto, conserva su
  código y el cambio se registra en el historial del documento.
- Si un requerimiento se divide, el original se retira y se crean códigos nuevos,
  dejando constancia de la descomposición.
- Las versiones siguientes de `MRQ-001` solo **agregan** identificadores, con
  numeración correlativa a partir del último asignado.

Los códigos actualmente asignados llegan hasta: `RF-44`, `RNF-11`, `RS-09`,
`RE-07`, `IN-06`, `RN-24`, `HU-46`, `UC-17`, `R-14`.

---

## 3. Prefijos en uso

| Prefijo | Significado | Documento fuente |
|---|---|---|
| `RF` | Requerimiento funcional | `MRQ-001` |
| `RNF` | Requerimiento no funcional | `MRQ-001` |
| `RS` | Requerimiento de seguridad | `MRQ-001` |
| `RE` | Restricción | `MRQ-001` |
| `IN` | Integración | `MRQ-001` |
| `RN` | Regla de negocio | `RN-001` |
| `HU` | Historia de usuario | `BKL-001` |
| `UC` | Caso de uso | `UML-001` |
| `R` | Riesgo | `RSK-001` |
| `OE` | Objetivo específico | `PV-001` |
| `PV` | Pendiente de validación | `ALC-001` |
| `AD` | Decisión de arquitectura | `ARQ-001` |

---

## 4. Tabla de equivalencias · requerimientos funcionales

Lectura: un código de la versión 1.0 y dónde quedó su contenido en la 2.0.

| v1.0 | Nombre en v1.0 | v2.0 | Nombre en v2.0 | Observación |
|---|---|---|---|---|
| RF-01 | Autenticación | RF-01 | Autenticación individual | Sin cambio de objeto |
| RF-02 | Gestión de roles y facultades | RF-02 | Modelo de permisos por roles | Sin cambio de objeto |
| RF-03 | Registro y consulta de clientes | RF-05, RF-08, RF-10 | Formulario de ingreso · Búsqueda · Ficha del cliente | Dividido en tres |
| RF-04 | Gestión de empresas | RF-05, RF-10 | Formulario de ingreso · Ficha del cliente | Absorbido: el encargo trata cliente natural y jurídico en la misma ficha |
| RF-05 | Registro de trámites | RF-11 | Gestión de servicios contratados | Renombrado según el vocabulario del encargo |
| RF-06 | Generación automática de documentos | RF-24 | Generación automática del documento | Sin cambio de objeto |
| RF-07 | Autoservicio de domicilio tributario | RF-19 a RF-26 | Flujo completo del portal | Descompuesto en el flujo de ocho pasos del §5.1 |
| RF-08 | Vista previa del borrador | RF-21 | Revisión y confirmación de datos | Sin cambio de objeto |
| RF-09 | Confirmación del cliente | RF-21 | Revisión y confirmación de datos | Fusionado con el anterior |
| RF-10 | Seguimiento del estado del trámite | RF-27 | Seguimiento del estado | Sin cambio de objeto |
| RF-11 | Descarga del documento final | RF-26 | Entrega del documento final | Sin cambio de objeto |
| RF-12 | Motor configurable de tipos de trámite | RF-33 | Definición configurable de tipos de trámite | **Código reasignado en v2 a otro requerimiento** |
| RF-13 | Administración de plantillas documentales | RF-36 | Sistema de plantillas | Sin cambio de objeto |
| RF-14 | Procesos asistidos | RF-18 | Procesos asistidos | **Código reasignado en v2 a otro requerimiento** |
| RF-15 | Registro de auditoría | RF-15 | Registro de auditoría | Sin cambio |
| RF-16 | Consulta de auditoría | RF-17 | Consulta de auditoría | Sin cambio de objeto |
| RF-17 | Migración de datos desde Excel | RF-43 | Migración desde Excel | Sin cambio de objeto |
| RF-18 | Reporte de inconsistencias de migración | RF-44 | Reporte de inconsistencias de migración | Sin cambio de objeto |
| RF-19 | Envío a firma electrónica | RF-25 | Envío al proceso de firma | Sin cambio de objeto |
| RF-20 | Múltiples firmantes por documento | RF-40 | Múltiples firmantes | Sin cambio de objeto |
| RF-21 | Registro del pago | RF-22 | Pago en línea | Ampliado: el encargo pide pago en línea, no solo su registro |
| RF-22 | Validación de datos del formulario | RF-06 | Validación previa al guardado | Ampliado con la detección de duplicidades del §4.4 |
| RF-23 | Catálogo de servicios | RF-19 | Catálogo de servicios contratables | Sin cambio de objeto |
| RF-24 | Panel de supervisión | RF-14 | Dashboard de indicadores | **Código reasignado en v2.** Además pasó de *Could* a *Must* |
| RF-25 | Alertas de vencimiento y renovación | RF-12, RF-13 | Detección de vencimientos · Alertas configurables | **Código reasignado en v2.** Dividido y pasó de *Could* a *Must* |

### Requerimientos funcionales nuevos en la versión 2.0

Sin equivalente en v1.0. Todos provienen de las especificaciones formales.

| v2.0 | Nombre | Origen |
|---|---|---|
| RF-03 | Administración de usuarios | ESP §4.2 |
| RF-04 | Restricción de eliminación para el perfil ejecutivo | ESP §4.2 |
| RF-07 | Identificador único de cliente | ESP §4.5 |
| RF-09 | Filtros de listado | ESP §4.5 |
| RF-16 | Protección del historial de auditoría | ESP §4.3 |
| RF-20 | Formularios dinámicos por servicio | ESP §5.2 |
| RF-23 | Verificación del resultado del pago | ESP §5.3 |
| RF-28 | Registro automático de la operación en el CRM | ESP §5.1, §9 |
| RF-29 | Creación o actualización del cliente desde el portal | ESP §9 |
| RF-30 | Registro del servicio contratado con sus fechas | ESP §9 |
| RF-31 | Asociación del documento firmado | ESP §7, §9 |
| RF-32 | Inicio del seguimiento del servicio | ESP §9 |
| RF-34 | Incorporación modular de documentos | ESP §8.2 |
| RF-35 | Incorporación de nuevos servicios | ESP §11 |
| RF-37 | Versionado de plantillas | Equipo |
| RF-38 | Copia congelada de los datos | Equipo |
| RF-39 | Verificación de integridad | Equipo (antes RS-06 en v1.0) |
| RF-41 | Máquina de estados por tipo de trámite | Equipo |
| RF-42 | Base de datos centralizada | ESP §4.1 |

---

## 5. Tabla de equivalencias · otros prefijos

| v1.0 | v2.0 | Observación |
|---|---|---|
| RNF-01 a RNF-08 | RNF-01 a RNF-09 | Conservan su objeto; se agregaron RNF-10 rendimiento y RNF-11 disponibilidad |
| RS-01 a RS-05 | RS-01 a RS-09 | Se ampliaron con los requisitos del §4.3 |
| RS-06 Integridad del documento | **RF-39** | Reclasificado: es una función del sistema, no un control de seguridad |
| RE-01 a RE-06 | RE-01 a RE-06 | Sin cambio. Se agregó RE-07 hosting sin definir |
| IN-01 a IN-06 | IN-01 a IN-06 | Sin cambio de objeto. IN-03 pasó de "sitio web actual" a "notificaciones a clientes"; el sitio web se trata en `ALC-001` |

---

## 6. Códigos retirados

Ninguno a la fecha. Todos los requerimientos de la versión 1.0 tienen destino en
la versión 2.0, según las tablas anteriores.

---

## 7. Documentos que dependen de este catálogo

| Documento | Relación | Estado |
|---|---|---|
| `MRQ-001 v2` | Define los identificadores | Vigente |
| `RN-001 v2` | Referencia requerimientos | Sincronizado |
| `BKL-001 v2` | Cada historia cita su requerimiento | Sincronizado |
| `TRZ-001 v2` | Relaciona requerimiento con historia, desarrollo y evidencia | Sincronizado |
| `UML-001` | Cada caso de uso cita sus requerimientos | Sincronizado |
| `MOD-001` | Cita reglas de negocio | Sincronizado |

Cualquier documento nuevo que referencie requerimientos debe usar los códigos de
este catálogo y agregarse a esta tabla.

---

## Historial de versiones

| Versión | Fecha | Cambios |
|---|---|---|
| 1.0 | 24-09-2026 | Creación del catálogo tras detectar la reutilización de códigos entre las versiones 1.0 y 2.0 de `MRQ-001` |
