# Levantamiento de requerimientos

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | ACTA-001 · Levantamiento de requerimientos |
| **Versión** | 1.0 |
| **Fecha** | 10 de septiembre de 2026 |
| **Estado del documento** | Preliminar, sujeto a validación en la reunión de validación con Easy Office (hito H2) |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## 1. Identificación de las sesiones

El levantamiento se realizó en dos sesiones remotas con la contraparte.

| | Sesión 1 | Sesión 2 |
|---|---|---|
| Carácter | Presentación de la problemática por parte de la empresa | Ronda de preguntas y cierre preliminar de requerimientos |
| Fecha | 28 de septiembre de 2026 | Jueves 3 de septiembre de 2026  |
| Registro | Transcripción disponible | Transcripción del 5 de septiembre de 2026 |
| Modalidad | Remota | Remota |

**Participantes**

- Por Easy Office: Natanael Escobar, representante de la empresa.
- Por el equipo CRM Easy Office: Fernando Cartagena (formuló preguntas en la
  sesión 2). asistencia de Nicolás Zapata y Marcos Álvarez

**Objetivo de las sesiones**

Comprender el modelo de operación de Easy Office, identificar los procesos
manuales que la empresa desea digitalizar, establecer prioridades y recoger las
restricciones y dependencias que condicionan el alcance del proyecto.

---

## 2. Contexto de Easy Office

*Todo el contenido de esta sección corresponde a lo declarado por la contraparte.*

Easy Office presta servicios de apoyo a emprendedores y empresas orientados a la
formalización del emprendimiento. Opera una red de oficinas en tres regiones:
Valparaíso, Metropolitana y O'Higgins.

Su oferta incluye asesorías tributarias, asesorías comerciales y de negocios, y
una línea de gestión documental mediante la cual entrega a sus clientes
documentos con firma electrónica avanzada, en el marco de la Ley 19.799.

La empresa se posiciona en el proceso de desnotarización: emite documentos que
antes requerían notaría, para todos los tipos que la normativa habilita. Entre
los documentos que declararon poder operar hoy con firma electrónica avanzada se
mencionaron: declaración jurada de domicilio, declaración de soltería, ciertos
contratos de arriendo, poderes de representación y cesiones de domicilio. Los
finiquitos fueron mencionados como no habilitados, porque se tramitan
directamente con la Dirección del Trabajo.

Además de generar documentos, Easy Office comercializa firmas electrónicas a sus
clientes a través de un acceso preferente en la plataforma de su proveedor.

---

## 3. Descripción del proceso actual

*Confirmado por la contraparte.*

El proceso documental típico se ejecuta de forma manual, con apoyo de personal
externo:

1. El cliente solicita un servicio.
2. El cliente envía sus antecedentes por canales como WhatsApp.
3. Un ejecutivo recopila y transcribe los datos.
4. Se completa manualmente la plantilla correspondiente.
5. Se genera el documento y se envía al cliente como borrador.
6. El cliente revisa y confirma. La revisión no es inmediata.
7. Easy Office acumula documentos y los sube en lote a la plataforma de firma.
8. El documento se firma con firma electrónica avanzada. En el caso del
   domicilio tributario, quien firma es el dueño de la oficina.
9. Se entrega el documento final al cliente.

La contraparte explicó explícitamente que el tiempo de espera de tres a cuatro
horas por servicio no corresponde a trabajo efectivo, sino a los tiempos muertos
entre pasos manuales: el cliente no revisa de inmediato y la empresa tampoco
firma de inmediato, sino que espera a reunir un lote de documentos.

La información de clientes se mantiene en planillas Excel.

---

## 4. Problemas detectados

*Todos declarados por la contraparte.*

| # | Problema | Efecto declarado |
|---|---|---|
| P-01 | La base de clientes está en Excel, sin mecanismos de seguridad | Cualquier ejecutivo accede a toda la información |
| P-02 | No existe registro de las acciones sobre los datos | No se sabe qué movimiento hizo cada ejecutivo, si eliminó un cliente o si compartió un dato |
| P-03 | El proceso documental es íntegramente manual | Transcripción de datos, generación y envío manual del documento |
| P-04 | La firma se ejecuta por lotes, no por documento | Se acumulan tiempos de espera de tres a cuatro horas o más por servicio |
| P-05 | Capacidad operacional limitada | Máximo de cuatro a cinco clientes diarios; el flujo de venta se declara estancado |
| P-06 | Comunicación dispersa en canales no trazables | Los antecedentes llegan por WhatsApp |
| P-07 | Preocupación regulatoria | La contraparte vinculó la necesidad de la base de datos con la seguridad de los datos y con las leyes de protección de datos personales |

---

## 5. Necesidades planteadas por la contraparte

*Confirmado por la contraparte.* Al ser consultada por los requerimientos más
urgentes, la empresa identificó tres:

1. **Base de datos.** Reemplazar Excel por una base SQL, migrar toda la
   información existente y dejar de usar Excel como sistema principal. Motivada
   explícitamente por seguridad de los datos y por el marco regulatorio de
   protección de datos personales.
2. **Automatización documental en la página web.** Que el cliente pueda completar
   el proceso sin intervención humana y sin pasar por el canal de venta.
3. **Software para los ejecutivos.** Visualización y gestión de clientes al
   momento de ingresarlos, con distintos roles y facultades dentro del sistema.

Consultada sobre si esperaba un CRM o una plataforma integral que incluyera CRM,
gestión de trámites y automatización documental, la contraparte indicó que la
segunda alternativa se asemeja más a lo que buscan.

Se identificó además el **domicilio tributario** como la funcionalidad más urgente
y de mayor impacto. Corresponde al contrato por el arriendo o cesión del espacio
que Easy Office facilita como domicilio, y contiene la ubicación de la oficina, el
rol de la oficina y los datos de la empresa cliente. La empresa ya cuenta con el
contrato estándar y con la autorización estándar regularizada ante el Servicio de
Impuestos Internos; lo que falta es la automatización y la vinculación con la
plataforma de firma.

---

## 6. Datos y métricas mencionadas

*Todas declaradas por la contraparte durante el levantamiento.*

| Métrica | Valor declarado | Sesión |
|---|---|---|
| Capacidad operacional actual | 3 a 5 clientes por día; se mencionó también un límite de 4 a 5 | Ambas |
| Volumen mensual | 200 a 250 clientes mensuales entre los distintos servicios | 2 |
| Tiempo de espera por servicio | 3 a 4 horas, o menos de 4 horas | 2 |
| Tiempo estimado con automatización | Una declaración jurada podría estar en menos de 10 minutos | 2 |
| Meta de escalamiento | Aproximadamente 50 clientes, contratos o documentos diarios | 1 |
| Catálogo documental | Entre 25 y 40 tipos de documentos | 2 |
| Documentos a priorizar | 4 o 5 de mayor demanda | 2 |
| Modalidad de pago | Solo contado por transferencia, modalidad 50% inicial y 50% al cierre | 2 |

> **Observación para el equipo.** Cinco clientes diarios por veintidós días
> hábiles no coincide aritméticamente con 200 a 250 clientes mensuales. La lectura
> más probable es que la primera cifra corresponda a clientes atendidos por día y
> la segunda a la cartera mensual incluyendo renovaciones, pero esto
> `[PENDIENTE DE VALIDAR CON EASY OFFICE]`.
>
> La meta de reducción de errores en torno a un 90% que aparece en el documento
> 1.5 **no está registrada en ninguna de las dos transcripciones**.
> `[PENDIENTE DE VALIDAR CON EASY OFFICE]`

---

## 7. Requerimientos identificados

La especificación detallada está en el documento
`MRQ-001 Matriz preliminar de requerimientos`. Este listado resume lo que se
recogió en las sesiones, con su estado.

### Confirmado por la contraparte

- Centralizar la información de clientes en una base de datos relacional y migrar
  la información actualmente en Excel.
- Registrar las acciones de los usuarios internos sobre la información: qué
  ejecutivo ingresó o modificó un dato, y qué movimiento realizó.
- Gestión de usuarios internos con distintos roles y facultades dentro del
  sistema.
- Módulo de ingreso y visualización de clientes para los ejecutivos.
- Automatizar la generación de documentos estandarizados a partir de plantillas.
- Permitir que el cliente complete el proceso de domicilio tributario sin
  intervención humana.
- Enviar el documento al cliente como borrador para revisión antes de la firma.
- Integrar la plataforma con el servicio de firma electrónica avanzada.
- Incorporar una pasarela de pago.
- Soportar varios firmantes por documento.
- Mantener procesos asistidos por un ejecutivo para los trámites no
  estandarizables.

### Propuesta del equipo

- Motor configurable de trámites y documentos, de modo que un nuevo tipo de
  trámite se defina mediante campos, validaciones, plantilla, estados y
  requerimiento de pago y firma, en lugar de desarrollarse individualmente.
- Versionado de plantillas documentales y asociación de cada documento emitido a
  la versión con que fue generado.
- Registro inmutable de auditoría, sin modificación ni eliminación de eventos.
- Verificación de integridad del archivo mediante función hash.
- Interfaces desacopladas para los servicios de firma y de pago.
- Validación de los datos del formulario antes de generar el documento.

### Pendiente

- Definición de los estados exactos de un trámite.
- Definición de los roles y facultades específicos del sistema.
- Campos obligatorios y reglas de validación de cada documento.
- Qué documentos, dentro de los priorizados, se implementarán efectivamente.

---

## 8. Restricciones conocidas

*Confirmado por la contraparte.*

- **R-01.** La creación de empresas no puede automatizarse en esta etapa: varía
  entre clientes en capital, número de acciones y razón social, y exige ingreso
  manual a plataformas externas como Tu Empresa en un Día y al Servicio de
  Impuestos Internos.
- **R-02.** Los finiquitos no están habilitados para este flujo; se tramitan con
  la Dirección del Trabajo y deben publicarse allí.
- **R-03.** El catálogo de documentos automatizables depende de lo que habilite la
  normativa de firma electrónica. La contraparte manifestó que quiere incorporar
  cada documento nuevo que la ley habilite.
- **R-04.** El pago se recibe hoy únicamente por transferencia, en modalidad
  50/50.
- **R-05.** La relación con notarías es manual y se mantiene así: Easy Office
  envía el documento, el notario lo procesa y lo devuelve, en algunos casos por
  WhatsApp. Aplica a protocolizaciones y a documentos que alguna entidad exija
  firmados ante notario.

---

## 9. Integraciones mencionadas

| Integración | Lo que declaró la contraparte | Estado |
|---|---|---|
| Firma electrónica avanzada | El proveedor es `tufirma.digital`. Easy Office tiene un acceso preferente en esa plataforma y a través de él comercializa firmas. Mencionaron un servicio de firma desasistida, en el que el documento se firma automáticamente al subirse. | Confirmado como proveedor. **La existencia de una API de integración y las condiciones de acceso no fueron confirmadas en la reunión**: la consulta sobre la API publicada en el sitio fue respondida en términos de reventa de firmas y acceso al portal, no de integración técnica. `[PENDIENTE DE VALIDAR]` |
| Pasarela de pago | La empresa maneja dos o tres opciones, entre ellas Mercado Pago y Transbank. El criterio declarado es cuál resulta más conveniente y más fácil de integrar con su página web. Indicaron que la decisión les corresponde a ellos y que solo falta tomarla. | Pendiente de decisión de la contraparte |
| Sitio web actual | Consultados sobre integrar la solución con lo que ya tienen, respondieron que idealmente sí. | Deseable. Estrategia técnica `[PENDIENTE DE DEFINIR]` |
| Servicio de Impuestos Internos | Consultados sobre si la pasarela de pago debía vincularse al SII, respondieron que no necesariamente y que esa parte la resuelve la empresa con su contabilidad. | Fuera de alcance |
| Notarías | Contacto manual, sin integración técnica. | Fuera de alcance |

---

## 10. Riesgos y dependencias externas identificadas

Se registran en detalle en `RSK-001 Registro inicial de riesgos`. Los surgidos
directamente del levantamiento son:

- Entrega de las plantillas documentales y de la estructura de campos del Excel
  por parte de la empresa.
- Habilitación de credenciales del proveedor de firma electrónica.
- Decisión pendiente sobre el proveedor de pago.
- Presencia de datos personales reales en la planilla que se entregará.
- Disponibilidad de la contraparte para las validaciones.

---

## 11. Decisiones tomadas

| # | Decisión | Origen |
|---|---|---|
| D-01 | La solución será una plataforma integral que combine CRM, gestión de trámites y automatización documental, no solo un CRM | Confirmado por la contraparte |
| D-02 | El trámite prioritario a automatizar es el domicilio tributario | Confirmado por la contraparte |
| D-03 | Se priorizarán 4 o 5 documentos de mayor demanda: contrato de servicio, autorización por domicilio tributario, contratos de arriendo, órdenes de compra y declaraciones juradas | Confirmado por la contraparte |
| D-04 | El resto del catálogo se abordará posteriormente | Confirmado por la contraparte |
| D-05 | La creación de empresas se mantiene como proceso asistido | Confirmado por la contraparte |
| D-06 | La relación con notarías queda fuera del alcance | Confirmado por la contraparte |
| D-07 | Excel se reemplaza completamente por una base de datos relacional | Confirmado por la contraparte |
| D-08 | Las integraciones de firma y pago se implementarán detrás de interfaces desacopladas | Decisión técnica del equipo |
| D-09 | El stack será React, Django con Django REST Framework, PostgreSQL y Docker | Decisión técnica del equipo |
| D-10 | Se construirá un motor configurable de trámites y documentos, validado con los documentos priorizados | Propuesta del equipo, pendiente de presentar a la contraparte |

---

## 12. Elementos pendientes de confirmar

| # | Pendiente | Responsable de resolverlo |
|---|---|---|
| PC-01 | Existencia, documentación y condiciones de acceso a la API del proveedor de firma | Easy Office |
| PC-02 | Credenciales de ambiente de prueba de firma electrónica | Easy Office |
| PC-03 | Proveedor de pago definitivo | Easy Office |
| PC-04 | Estructura, campos y calidad de la planilla Excel actual | Easy Office |
| PC-05 | Plantillas reales de los documentos priorizados y sus campos variables | Easy Office |
| PC-06 | Estados del trámite y transiciones válidas | Conjunto |
| PC-07 | Roles y facultades específicos del sistema | Conjunto |
| PC-08 | Cuántos documentos adicionales se implementarán, además del prioritario | Conjunto |
| PC-09 | Meta de reducción de errores | Easy Office |
| PC-10 | Relación entre clientes diarios y cartera mensual | Easy Office |
| PC-11 | Tecnología del sitio web actual, quién lo administra y estrategia de integración | Easy Office |
| PC-12 | Alcance y viabilidad legal de la firma desasistida | Easy Office / proveedor |
| PC-13 | Hosting del ambiente productivo | Conjunto |
| PC-14 | Validación de los prototipos | Easy Office |

---

## 13. Información que Easy Office debe entregar

Comprometido por la contraparte durante la sesión 2:

- Las plantillas de todos los documentos que trabajan, y los formatos utilizados.
- Los campos o casillas que mantienen actualmente en su base de datos Excel.

Solicitado adicionalmente por el equipo, `[PENDIENTE DE CONFIRMAR]`:

- Una copia de la planilla Excel, anonimizada si contiene datos personales.
- Un ejemplo de contrato de domicilio tributario ya emitido, con datos ficticios.

---

## 14. Próximos pasos

| # | Acción | Responsable | Plazo |
|---|---|---|---|
| PP-01 | Solicitar formalmente plantillas, formatos y estructura de campos | Fernando | 11-09-2026 |
| PP-02 | Solicitar credenciales de ambiente de prueba del proveedor de firma | Fernando | 11-09-2026 |
| PP-03 | Consultar la meta de reducción de errores y la relación entre las cifras de volumen | Fernando | 11-09-2026 |
| PP-04 | Agendar la reunión de validación de requerimientos y prototipos | Fernando | 11-09-2026 |
| PP-05 | Completar el prototipo del backoffice, incluida la configuración de un tipo de trámite | Marcos | 18-09-2026 |
| PP-06 | Preparar la matriz de requerimientos y el backlog para validación | Fernando / Equipo | 18-09-2026 |
| PP-07 | Preparar el modelo de datos preliminar sobre la base de la estructura recibida | Nicolás | 18-09-2026 |
| PP-08 | Ejecutar la reunión de validación con la checklist `CHK-001` | Equipo | 18-09-2026 (hito H2) |
| PP-09 | Levantar acta de la reunión de validación | Fernando | 18-09-2026 |

---

*Documento elaborado por el equipo del proyecto sobre la base de las
transcripciones de ambas sesiones. Toda afirmación atribuida a la contraparte
proviene de esas transcripciones, que se conservan como evidencia en
`docs/12_evidencias/`.*
