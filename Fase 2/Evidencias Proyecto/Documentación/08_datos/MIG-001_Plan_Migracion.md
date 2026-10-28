# Plan de migración desde la planilla de Easy Office

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | MIG-001 · Plan de migración |
| **Versión** | 0.1 |
| **Fecha** | 27 de octubre de 2026 |
| **Estado** | **Diseño propuesto.** El importador no existe y ningún ensayo se ha ejecutado. Contrastado con los modelos del backend en la rama principal (`7869d0a`) |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## 1. Alcance y punto de partida

- `13_plantillas/datos_sinteticos_muestra.csv` replica las **34 columnas** de la
  planilla de Easy Office con tres registros ficticios. Con eso se diseña y se
  ensaya el proceso ahora, sin esperar la planilla completa (TA-10, `DEC-001`
  DE-01).
- La app `migracion` del backend está vacía: **no hay comando de importación**.
  Este documento no le asigna un nombre ni da la carga por hecha.
- **Una fila es un contrato** con hasta tres servicios, no tres clientes. El RUT
  de la empresa, el RUT del representante y el folio del contrato cumplen papeles
  distintos y se conservan separados, junto con la procedencia de cada registro.

---

## 2. Mapeo de las 34 columnas

El destino usa los nombres reales de los modelos (`app.Modelo.campo`). La columna
*Control* indica la transformación y lo que se manda a cuarentena.

| # | Columna de origen | Destino en el modelo | Control |
|---|---|---|---|
| 1 | EMPRESA | `contratos.Contrato.marca` → `configuracion.Marca` | Resolver por `Marca.nombre`. No confundir la marca de Easy Office con la razón social del cliente |
| 2 | FOLIO | `Contrato.folio`, `codigo_sucursal`, `correlativo`, `anio_folio` | Conservar el valor y descomponerlo. Hoy el folio solo se asigna en el servicio de creación: el importador necesita una vía que reciba el folio histórico y actualice `CorrelativoFolio` del año para que no choque con folios nuevos (`DEC-001` DE-10). Duplicado o formato ambiguo: cuarentena |
| 3–5 | SERVICIO 1, 2 y 3 | `contratos.ServicioContratado` (`orden` 1 a 3), `servicio` → `ventas.Servicio`, `nombre` | Resolver con una tabla de equivalencias revisable. No crear una línea vacía. Servicio desconocido: cuarentena |
| 6–8 | PRECIO Ss 1, 2 y 3 | `ServicioContratado.precio`, `precio_incluye_iva` | Monto entero en pesos. `PLT-000` lo describe «sin IVA» y RN-44 dice que los precios lo incluyen (TA-16): marcar `precio_incluye_iva` según la regla que se adopte, sin aplicar IVA dos veces. Distinguir cero de dato ausente |
| 9–11 | CICLO PAGO Ss 1, 2 y 3 | `ServicioContratado.ciclo_pago` → `ventas.CicloPago` | Mapear único, mensual, trimestral, semestral y anual al catálogo por `codigo` |
| 12 | BANCO/LINK | `Contrato.cuenta_pago` → `configuracion.CuentaPago` | Resolver contra las cuentas configuradas; si no hay, `Marca.cuenta_pago_default`. No convertir texto libre en una cuenta nueva ni en un pago |
| 13–15 | DESCRIPCIÓN Ss 1, 2 y 3 | `ServicioContratado.descripcion` | Conservar el texto; solo normalizar espacios |
| 16 | FECHA DE INICIO | `Contrato.fecha_inicio` | Interpretar fechas en español («15 DE ENERO DE 2026») y fechas de Excel; conservar el valor original en el reporte |
| 17 | SUCURSAL | `ServicioContratado.inmueble` → `inmuebles.Oficina` | Resolver por `codigo_sucursal` y no solo por dirección libre |
| 18–19 | COMUNA y REGIÓN SUCURSAL | Control contra `Oficina.comuna` y `Oficina.region` | No se escriben: se comparan con el inmueble resuelto y se reporta la diferencia |
| 20 | ROL PROPIEDAD | Control contra `Oficina.rol_avaluo` | Texto, sin quitar guiones. Si difiere del inmueble resuelto: cuarentena |
| 21 | DURACIÓN | `Contrato.duracion_meses` | Leer «(12 MESES)» y «(6 MESES)»; validar contra `contratos.DuracionContrato` |
| 22 | RAZON SOCIAL | `clientes.Empresa.razon_social` | Identificar la empresa por RUT, no por nombre |
| 23 | R.U.T. | `Empresa.rut` y `clientes.Cliente` (tipo empresa) | `normalize_rut` y `validate_rut` de `core/validators.py`: sin puntos, K mayúscula, dígito verificador válido y único |
| 24 | REPRESENTANTE LEGAL | `clientes.Persona` (`nombres`, `apellido_paterno`, `apellido_materno`) y `clientes.RepresentanteLegal` | Separar nombres y apellidos solo con una regla explícita; si es ambiguo, cuarentena. El vínculo con el contrato es `Contrato.representante` |
| 25 | RUT REP. LEGAL | `Persona.rut` | Mismas validaciones que la columna 23, independientes de ella |
| 26–28 | DOMIC., COMUNA y REGIÓN REP. LEGAL | `Persona.domicilio`, `comuna`, `region` | No reemplazar por el domicilio tributario contratado |
| 29 | GIRO PRINCIPAL | `Empresa.giro` | Conservar el texto; no inventar un código del SII |
| 30 | MAIL | `Contrato.email_aviso` | Correo para avisos del contrato. No asignarlo a la persona representante sin evidencia |
| 31 | CELULAR 8 DIGITOS | `Contrato.movil_aviso` | El encabezado dice 8 dígitos y los ejemplos tienen 9: definir un formato canónico y validar, sin recortar |
| 32 | FECHA ESCRITURA/ESTATUTO ACTUALIZADO | `RepresentanteLegal.fecha_instrumento` | Validar la fecha; no confundirla con el inicio del servicio |
| 33 | SITIO/NOTARÍA | `RepresentanteLegal.sitio_o_notaria` | Conservar la referencia; no afirmar verificación notarial |
| 34 | VENCIMIENTO | Control contra `Contrato.fecha_vencimiento` | El sistema calcula inicio + duración − 1 día (RN-37). Si la planilla dice otra fecha, se reporta la diferencia y se decide antes de cargar |

`RepresentanteLegal.vigente_desde` no tiene columna de origen: la regla para
completarlo (por ejemplo, la fecha del instrumento) es una decisión pendiente del
equipo.

### Los vencimientos de la muestra

Las tres filas de la muestra tienen vencimiento igual a inicio + duración, sin
restar el día de RN-37:

| Fila del CSV (contando el encabezado) | Inicio | Meses | Vencimiento en la muestra | Según RN-37 |
|---|---|---|---|---|
| 2 | 15-01-2026 | 12 | 15-01-2027 | 14-01-2027 |
| 3 | 03-03-2026 | 12 | 03-03-2027 | 02-03-2027 |
| 4 | 20-04-2026 | 6 | 20-10-2026 | 19-10-2026 |

**No se corrigen.** `PLT-000` (OB-03) registra que en la planilla el vencimiento
se digita a mano y no sigue una regla única, y la muestra replica esa situación.
Son justamente el caso que el importador debe detectar (`MIG-T07`). Los seis RUT
de la muestra tienen dígito verificador válido.

---

## 3. Proceso propuesto

1. **Recepción.** Asignar un identificador de lote y registrar archivo, hash,
   fecha y procedencia. Los originales con datos personales quedan fuera de Git.
2. **Lectura y normalización.** Codificación, encabezados, fechas, montos, RUT,
   nombres y ubicación. Cada transformación queda registrada.
3. **Validación sin escritura.** Estructura de 34 columnas, obligatoriedad,
   dígito verificador, catálogos, fechas, duplicados y relaciones. Produce un
   reporte de simulación.
4. **Cuarentena.** Cada fila rechazada queda con número, motivo y campos
   afectados; no se carga una parte de su contrato.
5. **Carga en transacción.** Usar los servicios del dominio, no escrituras
   directas. Propuesta: la unidad es la fila; una fila válida se inserta completa
   y una inválida no deja registros parciales.
6. **Idempotencia.** Claves estables (RUT de empresa, RUT de representante,
   folio) para que repetir el lote no duplique nada. Distinguir un duplicado
   idéntico de un conflicto sobre el mismo folio.
7. **Conciliación.** Contar aceptados, rechazados, existentes sin cambio y
   conflictos; las categorías no se solapan y suman las filas leídas.

**Reglas para avanzar sin esperar a Easy Office:**

- No consumir un correlativo nuevo para reidentificar contratos importados.
- No inventar precios, fechas, instrumentos ni pagos para completar una fila.
- Ensayar primero con datos sintéticos, incluidos errores intencionales
  etiquetados.
- No enviar correos, pagos ni solicitudes de firma durante un ensayo.
- Definir cómo revertir un lote de prueba sin tocar registros previos. Para una
  carga real, respaldo previo y procedimiento de recuperación (`MOP-001`).

---

## 4. Casos de aceptación del importador

| ID | Caso | Resultado esperado | Estado |
|---|---|---|---|
| MIG-T01 | Fila válida con un servicio | Cliente, representante, contrato y línea relacionados | No ejecutado |
| MIG-T02 | Fila válida con tres servicios | Un contrato con tres líneas, sin clientes duplicados | No ejecutado |
| MIG-T03 | Repetir el mismo archivo | Sin duplicados; reporte de existentes | No ejecutado |
| MIG-T04 | RUT inválido | Rechazo con número de fila y motivo | No ejecutado |
| MIG-T05 | Folio repetido con otros datos | Conflicto en cuarentena, sin sobrescribir | No ejecutado |
| MIG-T06 | Servicio o sucursal desconocidos | Cuarentena; equivalencia pendiente | No ejecutado |
| MIG-T07 | Vencimiento distinto de RN-37, fin de mes y año bisiesto | Diferencia reportada; regla explícita | No ejecutado |
| MIG-T08 | Falla durante la carga de una fila | Sin contrato parcial | No ejecutado |
| MIG-T09 | Revertir un lote sintético | Solo se eliminan los datos del lote | No ejecutado |

---

## 5. Reporte del ensayo

| Dato | Valor |
|---|---|
| Lote, fecha, operador y revisor | Pendiente |
| Commit del importador y versión del mapeo | Pendiente |
| Filas leídas, aceptadas, rechazadas, existentes y en conflicto | No medido |
| Resultado de la reimportación | No ejecutado |
| Conteos de clientes, contratos y líneas antes y después | No medido |
| Reporte y registro saneados | Pendiente |
| Decisión | Pendiente |

La migración no se da por terminada porque el modelo de datos esté diseñado.

---

## Trazabilidad

| Elemento | Referencias |
|---|---|
| Requerimientos | RF-43, RF-44 |
| Reglas | RN-37, RN-38, RN-44 |
| Decisiones | `DEC-001` DE-01, DE-07, DE-10 |
| Temas abiertos | TA-03 folios históricos, TA-10 planilla completa, TA-16 IVA |
| Modelo | `MOD-001`, `UML-002` |
| Hito | H5 · migración (`GNT-001`, 31-10-2026) |

---

## Historial de versiones

| Versión | Fecha | Cambios |
|---|---|---|
| 0.1 | 27-10-2026 | Versión inicial: mapeo de las 34 columnas a los modelos reales, proceso, casos de aceptación y reporte de ensayo vacío |
