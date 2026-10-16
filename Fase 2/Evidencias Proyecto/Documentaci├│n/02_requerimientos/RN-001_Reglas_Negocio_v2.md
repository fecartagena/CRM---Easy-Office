# Reglas de negocio

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | RN-001 · Reglas de negocio |
| **Versión** | 2.3 |
| **Fecha** | 14 de octubre de 2026 |
| **Reemplaza a** | Versión 1.0 (10-09-2026) |
| **Motivo** | Las especificaciones formales de Easy Office confirman reglas que la versión 1.0 marcaba como propuesta, y agregan otras nuevas |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## Criterio de elaboración

Una regla de negocio describe cómo funciona el negocio, no cómo se implementa el
sistema. Las decisiones técnicas del equipo, como el uso de interfaces
desacopladas o la elección del stack, no figuran acá: están en `MRQ-001` y en
`ARQ-001`.

Cada regla indica su estado:

- **Confirmada** — declarada por la contraparte en el levantamiento o escrita en
  las especificaciones formales
- `[PROPUESTA – VALIDAR CON EASY OFFICE]` — inferida por el equipo, no enunciada
  por la contraparte

Se distingue además entre reglas del **proceso actual**, que describen cómo opera
Easy Office hoy, y del **proceso propuesto**, que describen cómo operará con la
plataforma. Confundirlas es lo que llevó a que la versión 1.0 tratara la
modalidad de pago 50/50 como si fuera una regla del sistema a construir.

---

## Cliente

**RN-01.** Un cliente puede ser una persona natural o una persona jurídica.
**Confirmada** · Proceso propuesto · ESP §4.4
*El formulario de ingreso contempla datos personales o empresariales según
corresponda.*

**RN-02.** Un cliente puede tener uno o varios servicios contratados
simultáneamente.
**Confirmada** · Proceso actual y propuesto · ESP §4.7

**RN-03.** Cada cliente tiene un identificador único dentro del sistema.
**Confirmada** · Proceso propuesto · ESP §4.5

**RN-04.** No pueden existir dos clientes con el mismo RUT.
`[PROPUESTA – VALIDAR CON EASY OFFICE]` · Proceso propuesto
*El §4.4 exige validar posibles duplicidades antes de guardar, pero no define el
criterio de unicidad.*

**RN-41.** El cliente debe registrarse en el portal para contratar. Con su cuenta
accede a sus trámites y documentos mientras dure el servicio.
**Confirmada** · Proceso propuesto · Reunión del 08-10-2026 (`ACTA-002`)

---

## Empresa

**RN-05.** Un documento emitido a nombre de una empresa debe identificar a su
representante legal.
**Confirmada** · Proceso actual y propuesto · Levantamiento

**RN-06.** No puede emitirse un documento a nombre de una empresa que no tenga un
representante legal con representación vigente.
`[PROPUESTA – VALIDAR CON EASY OFFICE]` · Proceso propuesto

---

## Servicio contratado

**RN-07.** Cada servicio contratado registra tipo, fecha de inicio, fecha de
término o vencimiento, estado, precio, características contratadas, documentos
asociados, observaciones y ejecutivo responsable.
**Confirmada** · Proceso propuesto · ESP §4.7

**RN-08.** El precio de un servicio contratado se registra en la contratación y
puede diferir del precio base del tipo de servicio.
`[PROPUESTA – VALIDAR CON EASY OFFICE]` · Proceso propuesto
*Se infiere de que el §4.9 pide ventas por servicio y por ejecutivo, lo que
requiere un precio por operación.*

**RN-09.** Un servicio con fecha de vencimiento genera alertas internas con
anticipación configurable.
**Confirmada** · Proceso propuesto · ESP §4.8
*Valores iniciales declarados: 60, 30, 15 y 7 días.*

**RN-10.** La renovación de un servicio genera un registro nuevo y conserva el
anterior.
`[PROPUESTA – VALIDAR CON EASY OFFICE]` · Proceso propuesto
*Necesaria para el historial que exige el §4.6.*

**RN-37.** La fecha de vencimiento de un servicio contratado es la fecha de inicio
más la duración contratada, menos un día. Ejemplo: inicio 02-10-2026 por 12
meses, vencimiento 01-10-2027.
**Confirmada** · Proceso actual y propuesto · `ACTA-002` (D2-04 y actualización
del 12-10-2026)
*La planilla actual registra el vencimiento a mano; la migración lo recalcula con
esta regla e informa las diferencias.*
*Implementación (issue #38): cuando el día de inicio no existe en el mes de
vencimiento, se usa el último día de ese mes* `[SUPUESTO – VALIDAR CON EASY OFFICE]`.

**RN-38.** Cada contrato recibe un folio con el formato
`{código de sucursal}-{correlativo de 4 dígitos}-{año}`, donde el código es `LF`
(La Florida), `VL` (Valparaíso) o `SA` (San Antonio / Algarrobo). El correlativo
es común a todas las sucursales y se reinicia cada año. El folio identifica el
contrato, no al cliente.
**Confirmada** · Proceso actual y propuesto · Formulario Excel entregado por Easy
Office, Reunión del 08-10-2026 (`ACTA-002`)
*Implementación (issue #38): el correlativo es común también a las marcas
EASY OFFICE e INOFFICE; la sucursal sale del inmueble del primer servicio con
ubicación y, si ninguno la tiene, se indica explícitamente; el ancho del
correlativo es configurable (cuatro por defecto) para conservar los folios
históricos de dos dígitos (TA-03). El año del folio es el año de emisión del
contrato* `[SUPUESTO – VALIDAR CON EASY OFFICE]`*. Si una renovación conserva el
folio está pendiente; hoy se registra como contrato nuevo que apunta al anterior.*
*Regla obtenida de la macro del formulario. La muestra de datos usa un formato
anterior de dos dígitos.* `[PENDIENTE DE VALIDAR CON EASY OFFICE: formato vigente
y tratamiento de los folios históricos]`

---

## Trámite

**RN-11.** Los trámites se clasifican en automatizados y asistidos. Los
automatizados corresponden a servicios estandarizados y no requieren intervención
humana. Los asistidos requieren la intervención de un ejecutivo.
**Confirmada** · Proceso propuesto · Levantamiento y ESP §4

**RN-12.** La creación de empresas es siempre un trámite asistido.
**Confirmada** · Proceso actual y propuesto · Levantamiento
*Varía entre clientes en capital, número de acciones y razón social, y requiere
ingreso manual a plataformas externas.*

**RN-13.** Un trámite no avanza a la etapa de firma mientras el cliente no
confirme que la información es correcta.
**Confirmada** · Proceso actual y propuesto · Levantamiento y ESP §5.1

**RN-14.** Un trámite solo puede transitar entre estados según las transiciones
definidas para su tipo.
`[PROPUESTA – VALIDAR CON EASY OFFICE]` · Proceso propuesto
*Los estados del trámite quedan definidos en RN-42 (Iniciado, Pendiente,
Completado); las transiciones permitidas siguen siendo propuesta.*

---

## Documento

**RN-15.** Un documento emitido queda asociado de forma permanente al trámite que
lo originó y a los datos con que fue generado. Las modificaciones posteriores a
la ficha del cliente no alteran documentos ya emitidos.
`[PROPUESTA – VALIDAR CON EASY OFFICE]` · Proceso propuesto

**RN-16.** Un documento no puede generarse si faltan campos obligatorios del tipo
de trámite correspondiente.
**Confirmada** · Proceso propuesto · ESP §4.4

**RN-17.** El catálogo de documentos que pueden emitirse con firma electrónica
está determinado por lo que habilita la legislación chilena.
**Confirmada** · Proceso actual y propuesto · ESP §8.2 y levantamiento
*Los finiquitos fueron mencionados como no habilitados: se tramitan con la
Dirección del Trabajo.*

**RN-18.** Un trámite puede producir más de un documento.
`[PROPUESTA – VALIDAR CON EASY OFFICE]` · Proceso propuesto
*El levantamiento menciona contrato y autorización para el domicilio tributario,
pero no se confirmó si ambos se emiten en la misma operación.*

---

## Plantilla

**RN-19.** Cada tipo de documento tiene una plantilla definida por Easy Office, en
la que los datos ingresados se insertan automáticamente.
**Confirmada** · Proceso propuesto · ESP §6 y §8.3

**RN-20.** Una plantilla puede tener varias versiones. Cada documento emitido
queda asociado a la versión con que se generó.
`[PROPUESTA – VALIDAR CON EASY OFFICE]` · Proceso propuesto

---

## Firma

**RN-21.** Los documentos estandarizados se firman con firma electrónica
avanzada.
**Confirmada** · Proceso actual y propuesto · Levantamiento y ESP §7

**RN-22.** Un documento puede requerir la firma de más de una parte.
**Confirmada** · Proceso actual y propuesto · Levantamiento

**RN-23.** En el trámite de domicilio tributario, la firma la ejecuta el titular
de la oficina.
**Confirmada** · Proceso actual · Levantamiento
*Discrepancia resuelta en `ACTA-002`: en el proceso propuesto el firmante
habitual es el cliente (RN-39), y la autorización de uso del domicilio la otorga
el propietario del inmueble (RN-43).*

**RN-24.** Un trámite no se considera completado mientras existan firmas
pendientes.
`[PROPUESTA – VALIDAR CON EASY OFFICE]` · Proceso propuesto

**RN-39.** En el domicilio tributario firman el cliente, a través de su
representante legal, y el dueño de la oficina, este último mediante firma
desasistida. En los demás documentos firman el cliente y las partes
involucradas.
**Confirmada** · Proceso propuesto · `ACTA-002` (D2-03 y actualización del
12-10-2026)
*El orden de las firmas en el domicilio tributario está pendiente de
confirmación.*

**RN-43.** La autorización de uso del domicilio tributario la otorga el
propietario del inmueble de la oficina, que puede ser distinto en cada oficina.
*El propietario puede ser una persona natural (bodega, `PLT-02`) o una empresa con
su representante legal (oficina, `PLT-01`); el documento incorpora la imagen de su
cédula de identidad, que se guarda como archivo protegido.*
**Confirmada** · Proceso actual y propuesto · Contratos entregados por Easy
Office, Reunión del 08-10-2026 (`ACTA-002`)

---

## Pago

**RN-25.** El pago del servicio se recibe hoy únicamente por transferencia, en
modalidad 50% al inicio y 50% al término.
**Confirmada** · **Proceso actual** · Levantamiento
*Describe cómo opera Easy Office hoy. No define cómo debe operar el pago en línea
del proceso propuesto.*

**RN-26.** El resultado del pago debe verificarse antes de continuar con las
etapas posteriores del trámite.
**Confirmada** · Proceso propuesto · ESP §5.3

**RN-27.** No todos los tipos de trámite requieren pago dentro del flujo.
`[PROPUESTA – VALIDAR CON EASY OFFICE]` · Proceso propuesto

**RN-28.** Un trámite puede registrar más de un pago: intentos fallidos y, si la
modalidad 50/50 se traslada al proceso en línea, dos cobros sucesivos.
`[PROPUESTA – VALIDAR CON EASY OFFICE]` · Proceso propuesto

**RN-40.** En el proceso en línea, el cliente paga después de revisar y aprobar el
borrador del documento.
**Confirmada** · Proceso propuesto · Reunión del 08-10-2026 (`ACTA-002`)

---

## Estados

**RN-29.** El estado de un servicio contratado y el de un trámite son visibles
tanto para el ejecutivo como para el cliente.
**Confirmada** · Proceso propuesto · Reunión del 08-10-2026 (`ACTA-002`)

**RN-42.** Un trámite pasa por tres estados: **Iniciado → Pendiente →
Completado**. Mientras está Pendiente, el trámite registra el motivo (por ejemplo,
falta de información, revisión del borrador, pago o firma), visible para el
ejecutivo en el CRM.
**Confirmada** · Proceso propuesto · `ACTA-002` (actualización del 12-10-2026)
`[PENDIENTE DE VALIDAR CON EASY OFFICE: lista cerrada de motivos (TA-13)]`

**RN-44.** Los precios de los servicios incluyen IVA y no existen precios
especiales por cliente.
*Al contratar se copia el precio de lista y no se puede cambiar; solo un servicio
sin precio fijo recibe el precio acordado. El anexo de `PLT-01` dice «TODOS LOS
VALORES MÁS I.V.A.», lo que contradice esta regla* `[DISCREPANCIA A RESOLVER CON
EASY OFFICE]`*; mientras tanto, cada servicio lleva el indicador
`precio_incluye_iva` y ningún precio está confirmado.*
**Confirmada** · Proceso actual y propuesto · `ACTA-002` (actualización del
12-10-2026)

---

## Permisos

**RN-30.** Los usuarios internos acceden al sistema con perfiles de acceso
diferenciados.
**Confirmada** · Proceso propuesto · ESP §4.2

**RN-31.** El perfil Dueño o Administrador tiene acceso completo a la información
y la configuración, puede crear, modificar y eliminar registros, administrar
usuarios, asignar permisos, consultar auditoría e historial, y acceder a reportes
y dashboards.
**Confirmada** · Proceso propuesto · ESP §4.2

**RN-32.** El perfil Ejecutivo puede crear clientes, ingresar y modificar
información autorizada, registrar servicios y gestiones, consultar clientes,
estados y vencimientos, y consultar el dashboard de indicadores. **No puede
eliminar registros de la base de datos.**
**Confirmada** · Proceso propuesto · ESP §4.2 · Dashboard: Reunión del 08-10-2026 (`ACTA-002`)

**RN-33.** El modelo de permisos debe permitir crear nuevos perfiles sin
reconstruir el sistema.
**Confirmada** · Proceso propuesto · ESP §4.2

---

## Auditoría

**RN-34.** Toda modificación importante registra el usuario que la realizó, la
fecha y hora, el registro afectado, la acción realizada y, cuando corresponda, el
valor anterior y el nuevo.
**Confirmada** · Proceso propuesto · ESP §4.3

**RN-35.** El historial de auditoría está protegido para impedir que usuarios
normales lo alteren o eliminen.
**Confirmada** · Proceso propuesto · ESP §4.3

**RN-36.** Se registran también los accesos al sistema y las acciones relevantes,
además de las modificaciones.
**Confirmada** · Proceso propuesto · ESP §4.3

---

## Resumen

| Categoría | Confirmadas | Propuestas por validar |
|---|---|---|
| Cliente | 4 | 1 |
| Empresa | 1 | 1 |
| Servicio contratado | 4 | 2 |
| Trámite | 3 | 1 |
| Documento | 2 | 2 |
| Plantilla | 1 | 1 |
| Firma | 5 | 1 |
| Pago | 4 | 2 |
| Estados | 2 | 0 |
| Permisos | 4 | 0 |
| Auditoría | 3 | 0 |
| **Total** | **33** | **11** |

En la versión 1.0 eran 14 confirmadas y 10 propuestas. El aumento proviene casi
enteramente de las especificaciones formales, en particular de los §4.2, §4.3,
§4.7 y §4.8.

La versión 2.1 incorpora siete reglas confirmadas en la reunión del 8 de octubre
(`ACTA-002`), confirma RN-29 y resuelve la discrepancia de RN-23. Las once reglas
que siguen como propuesta, más los puntos marcados en RN-37, RN-38 y RN-42, son la
agenda de la próxima validación. RN-37 quedó confirmada el 12-10-2026.

---

## Historial de versiones

| Versión | Fecha | Cambios |
|---|---|---|
| 1.0 | 10-09-2026 | Versión inicial a partir del levantamiento |
| 2.0 | 24-09-2026 | Incorporación de las reglas de las especificaciones formales; distinción entre proceso actual y propuesto; renumeración a RN-01 a RN-36; registro de la discrepancia de firmante en el piloto |
| 2.1 | 09-10-2026 | Reglas RN-37 a RN-43 a partir de `ACTA-002` y de los documentos entregados: vencimiento, folio, firmante, momento del pago, cuenta del cliente, estados y autorizante del domicilio. RN-29 pasa a confirmada; discrepancia de RN-23 resuelta; dashboard para el perfil Ejecutivo en RN-32 |
| 2.2 | 12-10-2026 | RN-37 con la regla de inicio + duración − 1 día; RN-39 precisa los firmantes del domicilio tributario; RN-42 cambia a tres estados con motivo de pendiente; nueva RN-44 sobre precios con IVA |
| 2.3 | 14-10-2026 | Notas de implementación en RN-37 (fin de mes), RN-38 (marcas, sucursal, ancho configurable, año de emisión, renovación), RN-43 (autorizante persona o empresa) y RN-44 (copia del precio y discrepancia del IVA con el anexo de `PLT-01`) |
