# Reglas de negocio

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | RN-001 · Reglas de negocio |
| **Versión** | 2.0 |
| **Fecha** | 24 de septiembre de 2026 |
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
*Los estados concretos están pendientes de definición.*

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
*El prototipo actual muestra al representante de la empresa como firmante, lo que
contradice esta regla.* `[DISCREPANCIA A RESOLVER EN LA FICHA FUNCIONAL DEL
PILOTO]`

**RN-24.** Un trámite no se considera completado mientras existan firmas
pendientes.
`[PROPUESTA – VALIDAR CON EASY OFFICE]` · Proceso propuesto

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

---

## Estados

**RN-29.** El estado de un servicio contratado y el de un trámite son visibles
tanto para el ejecutivo como para el cliente.
`[PROPUESTA – VALIDAR CON EASY OFFICE]` · Proceso propuesto

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
estados y vencimientos. **No puede eliminar registros de la base de datos.**
**Confirmada** · Proceso propuesto · ESP §4.2

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
| Cliente | 3 | 1 |
| Empresa | 1 | 1 |
| Servicio contratado | 2 | 2 |
| Trámite | 3 | 1 |
| Documento | 2 | 2 |
| Plantilla | 1 | 1 |
| Firma | 3 | 1 |
| Pago | 2 | 2 |
| Estados | 0 | 1 |
| Permisos | 4 | 0 |
| Auditoría | 3 | 0 |
| **Total** | **24** | **12** |

En la versión 1.0 eran 14 confirmadas y 10 propuestas. El aumento proviene casi
enteramente de las especificaciones formales, en particular de los §4.2, §4.3,
§4.7 y §4.8.

Las doce reglas marcadas como propuesta, más la discrepancia de RN-23, son la
agenda de la próxima validación con la contraparte.

---

## Historial de versiones

| Versión | Fecha | Cambios |
|---|---|---|
| 1.0 | 10-09-2026 | Versión inicial a partir del levantamiento |
| 2.0 | 24-09-2026 | Incorporación de las reglas de las especificaciones formales; distinción entre proceso actual y propuesto; renumeración a RN-01 a RN-36; registro de la discrepancia de firmante en el piloto |
