# Reunión de validación con Easy Office

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | ACTA-002 · Reunión de validación con la contraparte |
| **Versión** | 1.7 |
| **Fecha de la reunión** | Jueves 8 de octubre de 2026, 20:00 |
| **Modalidad** | `[CONFIRMAR: remota o presencial]` |
| **Participantes** | Por Easy Office: Natanael Escobar. Por el equipo: Fernando Cartagena `[CONFIRMAR asistencia de Nicolás Zapata y Marcos Álvarez]` |
| **Hito** | H2 · Validación de requerimientos con la contraparte |
| **Estado del documento** | Emitido. Pendiente de confirmación escrita de Easy Office |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## 1. Objetivo

Resolver las preguntas abiertas del levantamiento (`CHK-001`), revisar los
documentos entregados por Easy Office y confirmar el alcance del trámite piloto.

---

## 2. Documentos recibidos

| Documento | Contenido | Uso en el proyecto | Tratamiento |
|---|---|---|---|
| Especificaciones de documentos | Paso a paso del contrato de domicilio tributario y descripción de los demás documentos | Define el flujo del trámite piloto | Versionado sin datos personales (`01_levantamiento/ESP-DOC`) |
| Formulario Excel con macros | Formulario de ingreso de clientes y macro que asigna el folio | Referencia funcional del formulario y de la regla del folio | **No se versiona**: contiene datos reales |
| Muestra de campos de la base de datos | 34 columnas con dos registros reales | Fuente oficial de campos del modelo de datos | **No se versiona**. Se reemplaza por `13_plantillas/datos_sinteticos_muestra.csv` |
| Contrato de servicios, oficina | Plantilla Word con campos de combinación y autorización de domicilio | Plantilla del trámite piloto (`PLT-01`) | Anonimizada |
| Contrato de servicios, bodega | Igual estructura, con autorización de uso de bodega | `PLT-02` | Anonimizada |
| Contrato de arriendo de vehículo | Plantilla con marcadores | `PLT-03` | Sin datos personales |
| Contrato de arriendo de estacionamiento | Plantilla con espacios en blanco | `PLT-04` | Sin datos personales |
| Acuerdo de accionistas para SpA | Ejemplo con datos reales | `PLT-05` | Anonimizada |

El detalle de las plantillas, el diccionario de campos y la regla del folio están
en `13_plantillas/README.md`.

**Pendiente de recibir:** el poder simple, mencionado en las especificaciones.

---

## 3. Decisiones de la contraparte

Declaradas por Easy Office en la reunión. Se registran como *Confirmado* sujeto a
la confirmación escrita de esta acta.

| ID | Tema | Decisión | Documentos actualizados |
|---|---|---|---|
| D2-01 | Trámite piloto | El domicilio tributario es el trámite que se automatiza de extremo a extremo, siguiendo el proceso descrito en las especificaciones. El motor configurable permite incorporar después los documentos o servicios que Easy Office estime convenientes | Confirma `ALC-001`. `FUN-001` se ajusta al flujo y los estados confirmados en su próxima versión |
| D2-02 | Empresa | Easy Office e Inoffice son la misma empresa. Las dos cuentas bancarias de la planilla corresponden a Easy Office SpA | `MOD-001` |
| D2-03 | Firmante | Los documentos los firma generalmente el cliente y, en pocos casos, Easy Office. Precisado el 12-10-2026 (sección 8) | `RN-001` (RN-39) |
| D2-04 | Vencimiento | El vencimiento es la fecha de inicio más la duración contratada | `RN-001` (RN-37) |
| D2-05 | Cuenta del cliente | El cliente debe registrarse obligatoriamente en el portal, para acceder a sus documentos mientras dure el servicio | `MRQ-001` (RF-45), `BKL-001` (HU-03), `MOD-001`, `UML-001` |
| D2-06 | Dashboard | Lo ven el administrador y los ejecutivos | `MRQ-001` (RF-14), `BKL-001` (HU-41), `UML-001` |
| D2-07 | Estados del trámite | Registrado inicialmente como Solicitado → Llenado de información → Revisión → Pagado → Listo. **Reemplazado el 12-10-2026** por Iniciado → Pendiente → Completado (sección 8) | `RN-001` (RN-42), `MOD-001` |
| D2-08 | Roles | Solo existen dos roles: Administrador y Ejecutivo | `MRQ-001` (RF-02), `BKL-001`, `SPR-001` (D-02 cerrada) |
| D2-09 | Momento del pago | El cliente paga después de revisar el borrador | `RN-001` (RN-40) |
| D2-10 | Proveedor de pago | Por definir. Easy Office evalúa Klap; el equipo propuso Transbank. Actualizado el 12-10-2026: el equipo integra Klap y Flow detrás de la interfaz de pago | `MRQ-001` (IN-02), `RSK-001` (R-04) |
| D2-11 | Firma electrónica | Easy Office está gestionando las credenciales de prueba de tufirma.digital | `RSK-001` (R-03) |
| D2-12 | Acceso desde el sitio | Easy Office quiere un enlace directo desde su sitio web hacia la plataforma, donde el cliente sigue su trámite. Es un enlace, no una integración | `MRQ-001` (RF-46) |
| D2-13 | Identidad visual | La plataforma conserva la paleta de colores de easyoffice.cl | `MRQ-001` (RNF-12) |
| D2-14 | Alojamiento | Sin definir | `RSK-001` (R-12) |

---

## 4. Hallazgos del equipo al revisar los documentos

| ID | Hallazgo | Consecuencia |
|---|---|---|
| H2-01 | La macro del formulario genera el folio como `{sucursal}-{correlativo de 4 dígitos}-{año}`, con códigos `LF`, `VL` y `SA`, correlativo común a las tres sucursales y reinicio anual | Regla RN-38. La muestra de datos usa un formato anterior (`LF-01-2026`) |
| H2-02 | Cada ingreso del formulario genera un folio nuevo, y el contrato se titula "Contrato de servicios Nro. {folio}": **el folio identifica el contrato, no al cliente** | El modelo de datos incorpora la entidad `CONTRATO`; el cliente tiene un identificador propio |
| H2-03 | El vencimiento se digita a mano en la planilla. La muestra tiene casos que no cumplen la regla D2-04, y la autorización de domicilio emitida vence el día anterior al aniversario (inicio 02-10-2026, vencimiento 01-10-2027) | Confirmar si la regla es "inicio + duración" o "inicio + duración − 1 día". La migración recalcula y reporta diferencias |
| H2-04 | La autorización de domicilio la otorga el **propietario del inmueble**, que no es necesariamente Easy Office ni su representante | El autorizante se configura por oficina |
| H2-05 | Un contrato agrupa hasta tres servicios en la planilla. La cláusula PRIMERO del contrato solo nombra dos | El sistema genera la lista desde los servicios contratados, sin límite fijo |
| H2-06 | Los arriendos de vehículo y estacionamiento tienen como partes al cliente y a un tercero; Easy Office no es parte | Requieren datos de terceros (propietario, vehículo, inmueble) que hoy no existen en el modelo |
| H2-07 | Los documentos originales contienen datos personales reales e imágenes de una cédula de identidad | No se versionan. Solo se usan las versiones anonimizadas |

---

## 5. Temas abiertos

| # | Tema | Responsable | Estado |
|---|---|---|---|
| TA-01 | Confirmar por escrito los nombres exactos de los estados y si la firma es un estado propio entre "Pagado" y "Listo" | Easy Office | **Cerrado 12-10-2026**: Iniciado, Pendiente, Completado. Queda abierto TA-13 |
| TA-02 | Confirmar la regla del vencimiento (H2-03) | Easy Office | **Cerrado 12-10-2026**: inicio + duración − 1 día |
| TA-03 | Confirmar el formato vigente del folio y el tratamiento de los folios históricos | Easy Office | Abierto |
| TA-04 | Orden de los documentos a configurar después del domicilio tributario | Easy Office | Abierto |
| TA-05 | Entregar el poder simple | Easy Office | Abierto |
| TA-06 | Entregar la paleta oficial (códigos de color) y el logo en alta resolución | Easy Office | Abierto |
| TA-07 | Credenciales de prueba de tufirma.digital | Easy Office | En gestión |
| TA-08 | Decisión del proveedor de pago | Easy Office | Abierto |
| TA-09 | Alojamiento productivo | Easy Office y equipo | Abierto |
| TA-10 | Planilla completa anonimizada para la migración | Easy Office | Abierto |
| TA-11 | Autorización para usar las plantillas anonimizadas en el repositorio del proyecto | Easy Office | **Cerrado 12-10-2026**: autorizado |
| TA-12 | IVA en los precios, máximo de servicios por contrato y precios por cliente | Easy Office | **Parcialmente cerrado 12-10-2026**: los precios incluyen IVA y no hay precios especiales; queda abierto el máximo de servicios |
| TA-13 | Lista de motivos por los que un trámite o documento queda Pendiente | Easy Office | Abierto |
| TA-14 | Política de reembolsos de los pagos en línea | Easy Office y equipo | Abierto |
| TA-15 | Lista de precios vigente de los servicios (los cargados en el portal son de ejemplo) | Easy Office | Abierto |
| TA-16 | IVA: el anexo de `PLT-01` dice «MÁS I.V.A.», pero se informó que los precios lo incluyen (RN-44) | Easy Office | Abierto |
| TA-17 | Si la renovación automática conserva el folio o genera uno nuevo | Easy Office | Abierto |
| TA-18 | Si el correo y el celular de aviso son de la empresa o del representante | Easy Office | Abierto |
| TA-19 | Texto de la unidad del SII por sucursal y región de la sucursal de Algarrobo | Easy Office | Abierto |
| TA-20 | Precios de los planes del sitio web (anual 59.990 con firma incluida, semestral 39.990) y precio de la firma electrónica (3.500, 5.900 o 5.000) | Easy Office | Abierto |
| TA-21 | Si la fecha de inicio puede ser futura y hasta cuántos días (hoy, hasta 30) | Easy Office | Abierto |
| TA-22 | Domicilio del autorizante y ciudad de firma, que siguen escritos en la plantilla | Easy Office | Abierto |
| TA-23 | Si en línea solo contratan clientes empresa | Easy Office | Abierto |
| TA-24 | Quién es el ejecutivo de una venta en línea: hoy las ventas del portal no tienen ejecutivo y el dashboard las muestra como "En línea (sin ejecutivo)" | Easy Office | Abierto |

---

## 6. Compromisos del equipo

| # | Acción | Responsable | Plazo |
|---|---|---|---|
| CE-01 | Enviar esta acta a Easy Office y pedir confirmación escrita | Fernando | 10-10-2026 |
| CE-02 | Actualizar la documentación afectada (sección 7) | Equipo | 10-10-2026 |
| CE-03 | Dejar las plantillas anonimizadas y el diccionario de campos | Equipo | 10-10-2026 |
| CE-04 | Implementar el flujo de domicilio tributario con la plantilla `PLT-01` | Equipo | Sprint 2 y 3 |
| CE-05 | Corregir el acceso del dashboard para el rol Ejecutivo | Marcos | Sprint 2 · Cumplido: backend PR #54 y frontend PR #21 |

---

## 7. Documentos actualizados a partir de esta acta

| Documento | Versión |
|---|---|
| `MRQ-001` Matriz de requerimientos | 2.1 |
| `RN-001` Reglas de negocio | 2.1 |
| `BKL-001` Product Backlog | 2.1 |
| `MOD-001` Modelo de datos | 1.1 |
| `UML-001` Casos de uso | 1.1 |
| `RSK-001` Registro de riesgos | 1.2 |
| `GNT-001` Carta Gantt | 1.1 |
| `13_plantillas` Catálogo de plantillas | 1.0 (nuevo) |

Actualizaciones del 17-10-2026: `MOD-001` 1.5, `RN-001` 2.4, `BKL-001` 2.5, `TRZ-001` 2.4, `ARQ-001` 1.2, `DIS-001` 1.2, `PLA-001` 1.2, `RSK-001` 1.5, `13_plantillas` 1.2. Con los PR del backend (#51) y del frontend (#18) fusionados: `MOD-001` 1.6, `TRZ-001` 2.5, `BKL-001` 2.6, `PLA-001` 1.3, `DIS-001` 1.3.

Actualizaciones del 14-10-2026: `MOD-001` 1.4, `RN-001` 2.3, `BKL-001` 2.4, `TRZ-001` 2.3, `MRQ-001` 2.4, `RSK-001` 1.4, `13_plantillas` 1.1.

Actualizaciones del 12-10-2026: `MRQ-001` 2.3, `BKL-001` 2.3, `TRZ-001` 2.2, `MOD-001` 1.3, `ARQ-001` 1.1, `DIS-001` 1.1, `UML-001` 1.2, `RSK-001` 1.3, `PLA-001` 1.1.

---

## 8. Actualizaciones posteriores

| Fecha | Tema | Respuesta de Easy Office |
|---|---|---|
| 12-10-2026 | TA-02 · Vencimiento | La fecha de vencimiento es la fecha de inicio más la duración, menos un día. Ejemplo: inicio 02-10-2026, 12 meses, vence el 01-10-2027. Se actualiza RN-37 |
| 12-10-2026 | TA-11 · Uso de los documentos | Easy Office autoriza el uso de los documentos entregados en el proyecto. Las plantillas anonimizadas se publican en el repositorio; los originales, que contienen datos personales de sus clientes y una cédula de identidad, siguen fuera del repositorio |
| 12-10-2026 | TA-01 · Estados del trámite | Los estados son **Iniciado, Pendiente y Completado**. Reemplazan la lista registrada en D2-07. Se actualiza RN-42 |
| 12-10-2026 | Nuevo requerimiento | El ejecutivo debe poder ver en el CRM **por qué** un trámite o un documento está pendiente. Se registra como RF-47 y HU-61 |
| 12-10-2026 | D2-03 · Firmantes | En el domicilio tributario firman el cliente y el dueño de la oficina, este último con firma desasistida. En los demás documentos firman el cliente y las partes involucradas. Se actualiza RN-39 |
| 12-10-2026 | TA-12 · Precios | Los precios incluyen IVA y no existen precios especiales por cliente. Se registra RN-44 |
| 12-10-2026 | D2-10 · Pago | El equipo integra Klap y Flow como proveedores de pago; Easy Office decide cuál usará |
| 12-10-2026 | Avance técnico (equipo) | Quedan implementados en ambiente de pruebas la cuenta del cliente, la venta, la elección del medio de pago y el pago con Klap y con Flow, más la consulta de ventas en el CRM. No requiere respuesta de Easy Office, pero deja dos temas abiertos: política de reembolsos (TA-14) y precios (TA-15) |

Las actualizaciones del 12-10-2026 se registran a partir de lo conversado con
Easy Office y quedan sujetas a su confirmación por correo.
