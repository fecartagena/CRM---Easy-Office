# Alcance del MVP

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | ALC-001 · Alcance del MVP |
| **Versión** | 2.0 |
| **Fecha** | 24 de septiembre de 2026 |
| **Reemplaza a** | Versión 1.0 (10-09-2026) |
| **Motivo de la revisión** | Incorporación de las especificaciones formales entregadas por Easy Office |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## Cambios respecto de la versión 1.0

| # | Cambio | Origen |
|---|---|---|
| C-01 | El portal de contratación en línea entra al MVP como parte central, no como complemento | §5, §9 y §12 del encargo |
| C-02 | Se elimina la línea "e-commerce completo fuera de alcance" y se precisa qué queda fuera | Corrección de una ambigüedad de la v1.0 |
| C-03 | Alertas de vencimiento y renovación pasan de *Could* a requerimiento del MVP | §4.8 |
| C-04 | Dashboard de indicadores pasa de *Could* a requerimiento del MVP | §4.9 |
| C-05 | El motor configurable deja de ser propuesta del equipo y pasa a requerimiento del cliente | §5.2, §8.2, §8.3 y §11 |
| C-06 | Roles y permisos quedan confirmados y detallados | §4.2 |
| C-07 | Se incorpora la recomendación técnica como entregable formal | §10 |
| C-08 | Los documentos iniciales a automatizar se precisan | §8.2 |

---

## Nota sobre el término "e-commerce"

La versión 1.0 de este documento dejaba el e-commerce fuera del alcance. Leída
junto al encargo, esa formulación es engañosa y se corrige.

Lo que Easy Office llama **e-commerce** en los §5, §9 y §12 es el flujo de
contratación en línea: seleccionar servicio, ingresar antecedentes, confirmar,
pagar, generar el documento, firmarlo y registrar la operación en el CRM. **Ese
flujo está dentro del MVP** y es, de hecho, el eje del proyecto.

Lo que queda fuera es el **sitio comercial**: identidad visual, contenidos de
marketing, catálogo promocional y posicionamiento. El §14 menciona que Easy
Office entregará logotipo, identidad visual y contenidos de la página web, lo que
confirma que esa capa es responsabilidad de la empresa.

---

## 1. Incluido en el MVP

### 1.1 CRM · plataforma interna

- Acceso individual con usuario y contraseña
- Roles y permisos diferenciados: Dueño/Administrador y Ejecutivo, con la
  restricción de que los ejecutivos no eliminan registros
- Formulario de ingreso de clientes con validación de campos obligatorios,
  formatos y duplicidades
- Identificador único por cliente y búsqueda por RUT, nombre o razón social,
  teléfono, correo, folio y servicio contratado
- Ficha del cliente con datos, servicios, documentos, historial de gestiones,
  ejecutivo responsable e historial de modificaciones
- Gestión de servicios contratados con fechas, estado, precio y responsable
- Detección automática de vencimientos y alertas configurables a 60, 30, 15 y 7
  días
- Dashboard con los indicadores del §4.9
- Registro de auditoría protegido contra alteración

### 1.2 Portal de contratación en línea

- Catálogo de servicios contratables
- Formulario dinámico por servicio, construido desde la configuración
- Revisión y confirmación de los datos por parte del cliente
- Pago en línea con verificación del resultado antes de continuar
- Generación automática del documento
- Envío al proceso de firma electrónica
- Entrega del documento final y seguimiento del estado

### 1.3 Integración portal ↔ CRM

- Creación o actualización automática del cliente desde el portal
- Registro automático del servicio contratado con sus fechas
- Asociación del documento firmado al cliente y a la operación
- Sin redigitación interna de la información que ingresó el cliente

### 1.4 Motor configurable

- Definición de tipos de servicio y de trámite mediante configuración: campos,
  validaciones, plantilla, estados, y si requieren pago y firma
- Administración de plantillas documentales con versionado
- Incorporación de nuevos servicios y documentos sin desarrollo específico

### 1.5 Servicio piloto y documentos

- **Domicilio tributario** implementado de extremo a extremo, desde la
  contratación hasta el seguimiento del vencimiento. Es el piloto que el §15
  recomienda validar primero
- **Declaración jurada de domicilio** y **declaración jurada de soltería**,
  incorporadas mediante configuración del motor (§8.2)
- Uno o dos documentos adicionales de la lista priorizada, según lo que permita
  el tiempo `[CANTIDAD A CONFIRMAR EN LA VALIDACIÓN]`

### 1.6 Datos

- Base de datos relacional PostgreSQL
- Migración de la información mantenida en Excel con macros, con limpieza,
  validación y reporte de inconsistencias

### 1.7 Integraciones

- Firma electrónica y pasarela de pago detrás de interfaces desacopladas, con
  implementación real cuando la contraparte habilite los accesos y una
  implementación simulada que cumple el mismo contrato mientras tanto

### 1.8 Entrega

- Aplicación contenerizada, desplegada en ambiente productivo, con configuración
  por variables de entorno, respaldos programados y documentación técnica de
  operación

---

## 2. Fuera del MVP

| Elemento | Razón |
|---|---|
| Sitio comercial: identidad visual, contenidos, catálogo promocional | El §14 indica que Easy Office entrega logotipo, identidad visual y contenidos |
| Catálogo documental completo | El §8.2 define una lista inicial y señala que el resto se incorpora de forma modular |
| Automatización completa de la creación de empresas | La contraparte confirmó en el levantamiento que requiere intervención humana en cada caso |
| Integración directa con notarías | La contraparte confirmó que el contacto es manual y se mantiene así |
| Integración con el Servicio de Impuestos Internos | La contraparte indicó que no es necesaria |
| Notificaciones por correo o mensajería a clientes | El §4.8 las menciona como extensión futura. La arquitectura queda preparada; las alertas del MVP son internas |
| Nuevas sucursales, unidades o formas de pago adicionales | El §11 las menciona como escalabilidad futura, no como alcance actual |
| Aplicación móvil | El alcance acordado es una plataforma web |
| Operación y soporte posteriores a la entrega | Corresponden a Easy Office |

---

## 3. Pendiente de validación

| # | Elemento | Efecto si cambia | Origen |
|---|---|---|---|
| PV-01 | Listado definitivo de los ~30 campos del cliente | Ajusta el modelo de datos y el formulario | §14 |
| PV-02 | Formulario Excel con macros como referencia | Determina la complejidad de la migración | §14 |
| PV-03 | Listado de servicios y características de cada uno | Puebla los tipos de servicio | §14 |
| PV-04 | Formatos y plantillas de contratos | Determina la tecnología de generación documental | §14 |
| PV-05 | Listado definitivo de documentos a automatizar | Define cuántas configuraciones se entregan | §14 |
| PV-06 | Documentación técnica y API de la plataforma de firma | Define si la integración es real o simulada | §14 |
| PV-07 | Requisitos del sistema de pagos | Define la implementación de pago | §14 |
| PV-08 | Usuarios y roles iniciales definitivos | Confirma el modelo de permisos | §14 |
| PV-09 | Estados del trámite y transiciones válidas | Puebla la máquina de estados | Levantamiento |
| PV-10 | Si existe límite de empresas domiciliadas por oficina | Regla de negocio del servicio piloto | Levantamiento |
| PV-11 | Presupuesto de infraestructura y proveedor de alojamiento | Condiciona el despliegue | `REC-001` |
| PV-12 | Quién administrará el sistema tras la entrega | Determina el alcance del manual de operación | `REC-001` |

---

## 4. Criterio para controlar cambios de alcance

El alcance se congela con la validación de esta versión. Desde ese momento, toda
solicitud de funcionalidad nueva se evalúa así:

1. **Registro.** Se anota como issue en el repositorio, con su origen y quién la
   plantea. Nada entra al desarrollo sin pasar por el backlog.
2. **Impacto.** Se estima el esfuerzo contra el sprint en curso y los hitos, y se
   identifica qué elemento tendría que desplazarse.
3. **Contraste con los objetivos.** Si no contribuye a un objetivo específico del
   proyecto, se registra pero no se incorpora.
4. **Decisión.** Se toma con la contraparte en la revisión de cierre de sprint,
   con tres resultados posibles: se incorpora desplazando explícitamente otro
   elemento de esfuerzo igual o mayor, se difiere al backlog posterior a
   la entrega del MVP, o se descarta con la razón registrada.
5. **Registro del cambio.** Toda modificación se anota en el historial de
   versiones de este documento y se refleja en `MRQ-001` y en el backlog.

**Regla de fondo.** El MVP no crece por adición. Si entra algo, sale algo.

---

## Historial de versiones

| Versión | Fecha | Cambios | Responsable |
|---|---|---|---|
| 1.0 | 10-09-2026 | Versión inicial a partir del levantamiento | Equipo |
| 2.0 | 24-09-2026 | Incorporación de las especificaciones formales de Easy Office; corrección del alcance del portal de contratación; incorporación de vencimientos y dashboard | Equipo |
