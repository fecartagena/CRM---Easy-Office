# Documento de inicio de proyecto

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | DIP-001 · Documento de inicio de proyecto |
| **Versión** | 1.0 |
| **Fecha** | 24 de septiembre de 2026 |
| **Estado** | Emitido para revisión |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |
| **Contraparte** | Easy Office |
| **Clasificación** | Uso interno del proyecto |

---

## 1. Problema u oportunidad

Easy Office presta servicios de formalización y gestión documental a emprendedores
y empresas, con una red de oficinas en las regiones de Valparaíso, Metropolitana y
del Libertador General Bernardo O'Higgins. Su oferta incluye asesorías
tributarias y comerciales, y la emisión de documentos con firma electrónica
avanzada en el marco de la Ley 19.799.

**La operación es manual.** El cliente envía sus antecedentes por canales como
mensajería instantánea, un ejecutivo los transcribe a una plantilla, genera el
documento, lo devuelve como borrador, espera la confirmación y luego acumula
documentos para firmarlos en lote.

La empresa cuantificó el efecto de esa operación:

| Indicador | Situación actual |
|---|---|
| Capacidad operativa | 3 a 5 clientes por día |
| Cartera mensual | 200 a 250 clientes |
| Tiempo por servicio | 3 a 4 horas, mayoritariamente de espera entre pasos |
| Catálogo documental | 25 a 40 tipos de documentos, todos de elaboración manual |
| Sistema de registro | Planillas de cálculo con macros |

La empresa identificó además dos problemas sobre la información: cualquier
ejecutivo accede a la totalidad de la base de clientes, y no queda registro de
qué acción realizó cada usuario sobre esos datos.

**La oportunidad.** El tiempo perdido no corresponde a trabajo efectivo sino a
espera entre pasos manuales. Automatizar los documentos estandarizables y
centralizar la información permite atender más clientes con el mismo equipo,
eliminar la transcripción de datos y disponer de trazabilidad sobre la
información, en un contexto donde la normativa de protección de datos personales
lo hace exigible.

---

## 2. Objetivos del proyecto

### Objetivo general

Desarrollar e implementar una plataforma web que centralice la gestión de
clientes, servicios y documentos de Easy Office, y automatice la contratación,
generación y firma de los servicios estandarizables.

### Objetivos específicos

| # | Objetivo | Resultado verificable |
|---|---|---|
| OE-1 | Especificar los requerimientos, las reglas de negocio y el diseño funcional de la solución, validados con la contraparte | Especificación validada y alcance acordado |
| OE-2 | Implementar el CRM interno sobre una base de datos relacional, con control de acceso por roles, trazabilidad de las acciones y migración de la información mantenida en planillas | Sistema operativo con los datos migrados y auditoría activa |
| OE-3 | Implementar un motor configurable que permita incorporar servicios, formularios, plantillas y flujos documentales sin desarrollo específico para cada uno | Un servicio nuevo incorporado únicamente por configuración |
| OE-4 | Automatizar el servicio piloto de domicilio tributario de extremo a extremo, desde la contratación en línea hasta la firma del documento y el seguimiento de su vigencia | Trámite completado de punta a punta en la plataforma |
| OE-5 | Verificar y desplegar la solución, con pruebas y documentación técnica de operación | Solución desplegada, probada y documentada |

---

## 3. Usuarios y partes interesadas

### Usuarios del sistema

| Usuario | Perfil | Necesidad principal |
|---|---|---|
| **Cliente** | Emprendedor o empresa que contrata servicios | Resolver un trámite estandarizado sin depender de la disponibilidad de un ejecutivo |
| **Ejecutivo** | Personal de atención de Easy Office | Gestionar clientes y servicios sin transcribir información ni revisar planillas |
| **Administrador** | Dueño o administrador de Easy Office | Configurar el sistema, administrar usuarios y permisos, y consultar auditoría e indicadores |

Los perfiles y sus facultades están definidos por la contraparte: el perfil
Administrador tiene acceso completo; el perfil Ejecutivo puede crear y modificar
información autorizada, pero **no puede eliminar registros**.

### Partes interesadas

| Parte interesada | Interés en el proyecto | Involucramiento |
|---|---|---|
| Dirección de Easy Office | Aumentar la capacidad operativa y ordenar la información | Define el alcance, valida los entregables y decide sobre proveedores |
| Ejecutivos de Easy Office | Herramienta de trabajo diaria | Validan usabilidad y flujos |
| Clientes de Easy Office | Autoservicio y tiempos de respuesta | Beneficiarios indirectos |
| Proveedor de firma electrónica | Integración técnica | Provee la API y las credenciales |
| Proveedor de medios de pago | Integración técnica | Por definir |
| Equipo de desarrollo | Ejecución del proyecto | Diseña, construye, prueba y entrega |

---

## 4. Alcance del MVP

El detalle completo está en `ALC-001 Alcance del MVP v2.0`. Resumen:

### Dentro del alcance

**CRM interno.** Autenticación individual, roles y permisos, formulario de ingreso
de clientes con validación, identificador único, búsqueda y filtros, ficha del
cliente, gestión de servicios contratados, detección de vencimientos con alertas
configurables, dashboard de indicadores y registro de auditoría protegido.

**Portal de contratación.** Catálogo de servicios, formularios dinámicos por
servicio, revisión y confirmación, pago en línea con verificación del resultado,
generación automática del documento, envío a firma y entrega del documento final.

**Integración entre ambos.** La información ingresada por el cliente crea o
actualiza su registro, el servicio contratado queda registrado con su vigencia y
el documento firmado queda asociado a la operación, sin transcripción interna.

**Motor configurable.** Definición de tipos de servicio y de trámite mediante
configuración, con administración y versionado de plantillas documentales.

**Servicio piloto.** Domicilio tributario completo, más las declaraciones juradas
de domicilio y de soltería incorporadas por configuración.

**Datos.** Base de datos relacional y migración de la información mantenida en
planillas, con limpieza, validación y reporte de inconsistencias.

**Entrega.** Solución contenerizada y desplegada, con respaldos programados y
documentación técnica de operación.

### Fuera del alcance

| Elemento | Motivo |
|---|---|
| Sitio comercial: identidad visual y contenidos | La contraparte entrega esos insumos |
| Catálogo documental completo | Se incorpora progresivamente mediante configuración |
| Automatización de la constitución de empresas | Requiere intervención humana en cada caso |
| Integración con notarías | El contacto es manual y se mantiene así |
| Integración con el Servicio de Impuestos Internos | La contraparte indicó que no es necesaria |
| Notificaciones a clientes por canales externos | Extensión posterior |
| Aplicación móvil | El alcance acordado es una plataforma web |
| Operación y soporte posteriores a la entrega | Corresponden a la contraparte |

---

## 5. Restricciones

### De negocio

- La constitución de empresas no es automatizable: varía en capital, número de
  acciones y razón social, y requiere gestión en plataformas externas.
- Los finiquitos quedan fuera del flujo: se tramitan ante la Dirección del
  Trabajo.
- El catálogo de documentos automatizables está determinado por lo que habilita
  la legislación sobre firma electrónica.
- La relación con notarías se mantiene manual.

### Técnicas

- La integración de firma electrónica está sujeta a las capacidades y la API que
  disponga el proveedor.
- El proveedor de medios de pago no está definido por la contraparte.
- El proveedor de alojamiento no está definido.
- La planilla a migrar contiene datos personales reales y debe anonimizarse antes
  de usarse en ambientes de desarrollo.

### De ejecución

- El proyecto se ejecuta en una ventana de 18 semanas con un equipo de tres
  personas.
- El repositorio del proyecto es público, por lo que ningún dato personal real ni
  credencial puede versionarse.

### Regulatorias

- La Ley 21.719 sobre protección de datos personales entra en plena vigencia el 1
  de diciembre de 2026. El diseño incorpora control de acceso, minimización y
  trazabilidad en consecuencia. Este documento no constituye asesoría legal.

---

## 6. Justificación de la solución

### Por qué una plataforma y no un CRM

Consultada expresamente sobre si esperaba un CRM o una plataforma integral que
incluyera gestión de trámites y automatización documental, la contraparte indicó
que la segunda alternativa representa mejor su necesidad. Un CRM resolvería el
problema de registro, pero no el de capacidad operativa, que es el que limita el
crecimiento.

### Por qué un motor configurable

El catálogo documental de Easy Office depende de qué documentos habilita la
normativa de firma electrónica, y la contraparte declaró que quiere incorporar
cada nuevo tipo que la ley permita. Sus especificaciones piden explícitamente
formularios que se adapten a distintos servicios sin desarrollo nuevo, plantillas
configurables e incorporación modular de documentos.

Programar cada documento por separado produciría una solución que queda obsoleta
con cada cambio normativo. Representar los tipos de trámite como configuración
permite que la empresa incorpore documentos sin depender de desarrollo.

El alcance de esta decisión es acotado: el motor cubre la familia de trámites
estandarizables. Una regla de negocio fuera del modelo requiere extenderlo.

### Por qué la arquitectura propuesta

Un requisito central del encargo es que la información ingresada por el cliente no
se vuelva a digitar internamente. Eso determina que el CRM y el portal operen
sobre un mismo dominio y una misma base de datos, en lugar de ser dos sistemas
que se sincronizan.

La justificación completa del stack tecnológico, incluidos los criterios de
alojamiento, seguridad, respaldo, escalabilidad, costos recurrentes,
mantenibilidad y dependencia de proveedores, está en `REC-001 Recomendación
técnica`.

### Retorno esperado

Las siguientes son metas del proyecto, a medir durante su ejecución. No son
resultados obtenidos.

| Métrica | Línea base | Meta |
|---|---|---|
| Tiempo de emisión de un trámite estandarizado | 3 a 4 horas | A determinar mediante medición comparativa |
| Capacidad operativa diaria | 3 a 5 clientes | Escalar según proyección de la contraparte |
| Transcripción manual de datos | En cada operación | Eliminada en los trámites automatizados |
| Trazabilidad de acciones sobre los datos | Inexistente | Registro completo e inalterable |

---

## 7. Documentos relacionados

| Código | Documento |
|---|---|
| ACTA-001 | Acta de levantamiento de requerimientos |
| MRQ-001 | Matriz de requerimientos v2.0 |
| RN-001 | Reglas de negocio v2.0 |
| ALC-001 | Alcance del MVP v2.0 |
| FUN-001 | Ficha funcional del servicio piloto |
| REC-001 | Recomendación técnica |
| DIS-001 | Documento de diseño |
| RSK-001 | Registro de riesgos |
| PLA-001 | Plan de pruebas |

---

## Control de cambios

| Versión | Fecha | Descripción | Autor |
|---|---|---|---|
| 1.0 | 24-09-2026 | Emisión inicial | Equipo de proyecto |
