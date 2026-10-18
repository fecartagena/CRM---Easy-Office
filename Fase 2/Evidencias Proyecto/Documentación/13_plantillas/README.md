# Plantillas documentales

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | PLT-000 · Catálogo de plantillas y diccionario de campos |
| **Versión** | 1.2 |
| **Fecha** | 17 de octubre de 2026 |
| **Fuente** | Documentos entregados por Easy Office el 8 de octubre de 2026 (ver `ACTA-002`) |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## Tratamiento de los originales

Los documentos originales contienen datos personales reales de clientes y de
representantes, una cuenta bancaria e imágenes de una cédula de identidad.
**Los originales no se versionan.** Esta carpeta contiene versiones anonimizadas
en las que:

- Cada campo de combinación de Word (`MERGEFIELD`) muestra su nombre entre
  comillas angulares, por ejemplo `«RAZON_SOCIAL»`, en lugar del valor del último
  cliente combinado. Los campos siguen funcionando.
- El texto con datos personales que no era un campo se reemplazó por un marcador
  con el mismo formato, por ejemplo `«EO_REP_RUT»`.
- Se eliminaron las imágenes de la cédula de identidad, el vínculo a la planilla
  de origen de la combinación (ruta local del equipo de Easy Office), los datos de
  destinatarios guardados y los autores del documento.
- Se conservan los datos corporativos de Easy Office: razón social, RUT de la
  empresa, correos y teléfono institucionales, y el logo del encabezado.

Los originales quedan en la carpeta de acceso restringido del equipo, no en el
repositorio.

**Publicación.** Easy Office autorizó el 12-10-2026 el uso de los documentos
entregados (`ACTA-002`, TA-11). Las plantillas anonimizadas se publican en esta
carpeta; los originales no.

---

## Catálogo

| Código | Plantilla | Formato de origen | Estado de la plantilla | Prioridad |
|---|---|---|---|---|
| PLT-01 | Contrato de servicios con autorización de domicilio (oficina) | Word con campos de combinación | Lista para generación | **Trámite piloto: domicilio tributario** |
| PLT-02 | Contrato de servicios con autorización de uso de bodega | Word con campos de combinación | Requiere ajustes (ver observaciones) | `[PENDIENTE DE VALIDAR CON EASY OFFICE]` |
| PLT-03 | Contrato de arriendo de vehículo | Word con marcadores `[ ]` | Requiere convertir marcadores en campos | `[PENDIENTE DE VALIDAR CON EASY OFFICE]` |
| PLT-04 | Contrato de arriendo de estacionamiento | Word con espacios en blanco | Requiere definir cada campo | `[PENDIENTE DE VALIDAR CON EASY OFFICE]` |
| PLT-05 | Acuerdo de accionistas para SpA | Word con texto variable en negrita | Requiere cláusulas variables | `[PENDIENTE DE VALIDAR CON EASY OFFICE]` |
| — | Poder simple | No entregado | — | `[PENDIENTE DE RECIBIR]` |

La especificación de Easy Office sobre cómo se completa cada documento está en
`01_levantamiento/ESP-DOC_Especificaciones_Documentos.docx`.

---

## Diccionario de campos de PLT-01 y PLT-02

Los nombres de campo son los de la plantilla. La columna de origen corresponde a
la planilla de Easy Office (`MUESTRA DE CAMPOS BB DD`, 34 columnas).

| Campo en plantilla | Columna en planilla | Dato | Entidad en `MOD-001` |
|---|---|---|---|
| `FOLIO` | FOLIO | Número del contrato | `CONTRATO` |
| `FECHA_DE_INICIO` | FECHA DE INICIO | Inicio de vigencia, en texto: "15 DE ENERO DE 2026" | `CONTRATO` |
| `DURACIÓN` | DURACIÓN | "(12 MESES)", "(6 MESES)", etc. | `CONTRATO` |
| `VENCIMIENTO` | VENCIMIENTO | Fin de vigencia | `CONTRATO` (calculado) |
| `SERVICIO_1..3` | SERVICIO 1..3 | Nombre del servicio | `SERVICIO_CONTRATADO` |
| `PRECIO_Ss_1..3` | PRECIO Ss 1..3 | Precio pactado, sin IVA | `SERVICIO_CONTRATADO` |
| `CICLO_PAGO_Ss_1..3` | CICLO PAGO Ss 1..3 | Único, mensual, trimestral, semestral, anual | `SERVICIO_CONTRATADO` |
| `DESCRIPCIÓN_Ss_1..3` | DESCRIPCIÓN Ss 1..3 | Contenido del servicio | `TIPO_SERVICIO` |
| `BANCOLINK` | BANCO/LINK | Texto de la cuenta de pago de Easy Office | `CUENTA_PAGO` |
| `SUCURSAL` | SUCURSAL | Dirección de la oficina | `OFICINA` |
| `COMUNA_SUCURSAL` | COMUNA SUCURSAL | Comuna de la oficina | `OFICINA` |
| `REGIÓN_SUCURSAL` | REGIÓN SUCURSAL | Región de la oficina | `OFICINA` |
| `ROL_PROPIEDAD` | ROL PROPIEDAD | Rol de avalúo de la oficina | `OFICINA` |
| `RAZON_SOCIAL` | RAZON SOCIAL | Razón social del cliente | `CLIENTE` |
| `RUT` | R.U.T. | RUT de la empresa cliente | `CLIENTE` |
| `GIRO_PRINCIPAL` | GIRO PRINCIPAL | Giro | `CLIENTE` |
| `FECHA_ESCRITURAESTATUTO_ACTUALIZADO` | FECHA ESCRITURA/ESTATUTO ACTUALIZADO | Fecha de la personería | `REPRESENTACION` |
| `SITIONOTARÍA` | SITIO/NOTARÍA | Registro o notaría de la personería | `REPRESENTACION` |
| `REPRESENTANTE_LEGAL` | REPRESENTANTE LEGAL | Nombre del representante | `PERSONA` |
| `RUT_REP_LEGAL` | RUT REP. LEGAL | RUT del representante | `PERSONA` |
| `DOMIC_REP_LEGAL` | DOMIC. REP. LEGAL | Domicilio particular | `PERSONA` |
| `COMUNA_REP_LEGAL` | COMUNA REP. LEGAL | Comuna | `PERSONA` |
| `REGIÓN_REP_LEGAL` | REGIÓN REP. LEGAL | Región | `PERSONA` |
| `MAIL` | MAIL | Correo de contacto | `CLIENTE` |
| `CELULAR_8_DIGITOS` | CELULAR 8 DIGITOS | Celular; la plantilla antepone "+56" | `CLIENTE` |
| — | EMPRESA | Siempre "EASY OFFICE" en la muestra; Easy Office e Inoffice son la misma empresa (`ACTA-002`) | `CONFIG_EMPRESA` |

### Marcadores de configuración (no provienen del cliente)

| Marcador | Dato | Dónde se administra |
|---|---|---|
| `EO_REPRESENTANTE`, `EO_REP_RUT`, `EO_REP_NACIONALIDAD`, `EO_REP_ESTADO_CIVIL`, `EO_REP_PROFESION` | Representante legal de Easy Office | `CONFIG_EMPRESA` |
| `AUTORIZANTE_RAZON_SOCIAL`, `AUTORIZANTE_RUT`, `AUTORIZANTE_REPRESENTANTE`, `AUTORIZANTE_REP_*` | Propietario del inmueble que autoriza el domicilio | `OFICINA` |
| `AUTORIZANTE_NOMBRE`, `AUTORIZANTE_NACIONALIDAD`, `AUTORIZANTE_ESTADO_CIVIL`, `AUTORIZANTE_PROFESION` | Autorizante persona natural (caso de la bodega) | `OFICINA` |

En el contrato original estos datos estaban escritos directamente en el texto. La
autorización de domicilio la firma el **propietario del inmueble**, que puede ser
distinto en cada oficina; por eso se modela como atributo de la oficina y no como
dato fijo.

---

## Regla del folio (obtenida de la planilla entregada)

La macro del formulario `FORMULARIO EASY OFFICE 2026` genera el folio así:

```
{código de sucursal}-{correlativo de 4 dígitos}-{año}
LF-0152-2026
```

| Elemento | Regla observada en la macro |
|---|---|
| Código de sucursal | `LF` La Florida · `VL` Valparaíso · `SA` San Antonio / Algarrobo |
| Correlativo | Mayor número del año en curso **entre todas las sucursales**, más uno |
| Reinicio | Cada año: la base tiene una hoja por año |
| Unidad | Cada ingreso del formulario crea una fila y un folio nuevos: el folio identifica el **contrato**, no al cliente |

La muestra de la base de datos usa `LF-01-2026` (dos dígitos), formato anterior a
la macro actual. El sistema usa cuatro dígitos con un ancho configurable
(`CONTRATO_FOLIO_ANCHO`), de modo que los folios históricos puedan conservarse.

**Uso en el sistema.** El backend guarda su propia copia de `PLT-01`, convertida
una sola vez desde esta versión anonimizada: los campos de combinación pasan a
marcas propias, el anverso y el reverso de la cédula del autorizante quedan como
imágenes que el sistema inserta al generar el documento, y el control de cambios
se acepta. Esta carpeta sigue siendo la referencia; el sistema no la lee.
`[PENDIENTE DE VALIDAR CON EASY OFFICE]` cómo se tratan esos folios en la
migración (TA-03).

---

## Observaciones sobre las plantillas

| # | Plantilla | Observación | Propuesta |
|---|---|---|---|
| OB-01 | PLT-01 | La cláusula PRIMERO solo nombra `SERVICIO_1` y `SERVICIO_2`; un tercer servicio aparece en la tabla de precios pero no en la cláusula | Generar la lista de servicios desde los servicios contratados, sin límite fijo · Resuelto en la plantilla convertida: las filas de servicio se repiten según los servicios del contrato |
| OB-02 | PLT-01 | La autorización dice "UNIDAD DE SAN ANTONIO «COMUNA_SUCURSAL»" para la unidad del SII | Confirmar el texto correcto por sucursal y llevarlo a la configuración de la oficina · Resuelto en la plantilla convertida: la unidad del SII y la municipalidad salen de la sucursal `[supuesto: texto por sucursal]` |
| OB-03 | PLT-01, PLT-02 | El vencimiento se digita a mano en la planilla; la muestra tiene vencimientos que no siguen una regla única | El sistema lo calcula: inicio + duración − 1 día (RN-37) |
| OB-04 | PLT-01, PLT-02 | La tabla de servicios complementarios tiene precios "desde" escritos en el documento, distintos entre plantillas (contabilidad $15.900 y $19.900) | Llevar los precios a configuración `[PENDIENTE DE VALIDAR CON EASY OFFICE]` |
| OB-05 | PLT-02 | La duración ("DOCE (12) MESES") y la dirección y rol de la bodega están escritos en el texto, no como campos | Convertirlos en campos de la oficina o bodega |
| OB-06 | PLT-03 | Usa marcadores `[CAMPO]`; las partes son el propietario del vehículo y la empresa cliente. Easy Office no es parte | Convertir marcadores en campos y modelar al propietario como persona |
| OB-07 | PLT-04 | Solo tiene líneas en blanco; incluye datos del Conservador de Bienes Raíces y cuenta bancaria del arrendador | Definir el formulario completo con Easy Office antes de automatizarlo |
| OB-08 | PLT-05 | Los artículos varían según el trámite (cambio de representante, razón social, nombre de fantasía, capital, etc.) y el original dice "constituida por 10.000 accionistas" en lugar del número de accionistas | Modelar las modificaciones como cláusulas opcionales; confirmar el texto con Easy Office |
| OB-09 | PLT-01, PLT-02 | El anexo de servicios complementarios dice «TODOS LOS VALORES MÁS I.V.A.», pero Easy Office indicó que los precios incluyen IVA (RN-44) | Confirmar con Easy Office; cada servicio lleva el indicador `precio_incluye_iva` |
| OB-10 | PLT-01, PLT-02 | El domicilio del autorizante (Valparaíso) y la ciudad de firma ("En Santiago") están escritos en el texto, no como campos | Siguen fijos en la plantilla convertida; confirmar con Easy Office si deben salir de la configuración (TA-22) |

---

## Datos sintéticos

`datos_sinteticos_muestra.csv` replica las 34 columnas de la planilla de Easy
Office con tres registros ficticios, uno por sucursal. Los nombres, correos y
teléfonos son inventados; los RUT son generados con dígito verificador válido
para probar las validaciones. Sirve para desarrollo, pruebas y la migración
mientras no se reciba la planilla real anonimizada.
