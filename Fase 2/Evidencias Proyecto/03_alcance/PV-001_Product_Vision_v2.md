# Product Vision

| | |
|---|---|
| **Cliente** | Easy Office |
| **Producto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | PV-001 · Product Vision |
| **Versión** | 2.0 |
| **Fecha** | 24 de septiembre de 2026 |
| **Estado** | Emitido para revisión |
| **Preparado por** | Equipo de proyecto |

---

## Para quién

| | |
|---|---|
| **Cliente final** | Emprendedores y empresas que contratan servicios de formalización y documentación. No conocen el trámite y hoy dependen de la disponibilidad de un ejecutivo. |
| **Ejecutivo** | Personal de atención de Easy Office. Hoy transcribe datos a mano y consulta planillas. |
| **Administrador** | Dirección de Easy Office. Necesita configurar el sistema, controlar accesos y ver el estado de la operación. |

## Qué problema resuelve

Easy Office opera manualmente. El cliente envía sus antecedentes por mensajería,
un ejecutivo los transcribe, genera el documento, lo devuelve como borrador,
espera confirmación y acumula documentos para firmarlos en lote. El proceso toma
entre tres y cuatro horas por servicio, y ese tiempo es espera entre pasos, no
trabajo efectivo.

El resultado es una capacidad tope de tres a cinco clientes diarios sobre una
cartera de 200 a 250 mensuales, y una base de clientes en planillas donde todos
acceden a todo y nadie deja rastro de lo que modifica.

## Qué es el producto

Una plataforma web con dos caras sobre una misma base de información:

**Un CRM interno** donde los ejecutivos registran y consultan clientes, gestionan
los servicios contratados con sus vigencias, reciben alertas de vencimiento y
donde cada acción queda registrada de forma inalterable.

**Un portal de contratación** donde el cliente selecciona un servicio, ingresa sus
datos, revisa el documento, paga, firma y lo descarga, sin que un ejecutivo
intervenga.

Lo que une ambas caras es que **lo que el cliente escribe no se vuelve a
escribir**: la contratación crea el registro, el servicio y el documento en el
CRM automáticamente.

## Qué lo diferencia

Un tipo de trámite no se programa, se describe. Cada servicio define por
configuración qué campos pide, qué validaciones aplica, qué plantilla usa, qué
estados recorre y si requiere pago y firma. El sistema interpreta esa
configuración y construye el flujo.

Esto importa porque el catálogo documental de Easy Office depende de qué
documentos habilita la normativa de firma electrónica, y la empresa quiere
incorporar cada nuevo tipo que la ley permita. Un catálogo que cambia con la
regulación no conviene programarlo documento por documento.

El alcance de esta capacidad es acotado y conviene declararlo: cubre la familia
de trámites estandarizables. Una regla de negocio fuera del modelo requiere
extender el motor.

## Qué valor entrega

| Para Easy Office | Para el cliente final |
|---|---|
| Atender más operaciones con el mismo equipo | Resolver un trámite sin esperar a un ejecutivo |
| Eliminar la transcripción manual y sus errores | Ver el documento antes de comprometerse |
| Saber quién accedió y modificó cada dato | Seguir el estado de su trámite |
| Anticipar vencimientos y gestionar renovaciones | Disponer del documento firmado en línea |
| Incorporar servicios nuevos sin desarrollo | — |

## Alcance del primer entregable

**Entra:** CRM interno completo · base de datos relacional con la migración desde
planillas · motor configurable · servicio de domicilio tributario de extremo a
extremo · declaraciones juradas de domicilio y de soltería por configuración ·
portal de contratación del servicio piloto · firma y pago tras interfaces
desacopladas · despliegue con respaldos y documentación.

**No entra:** el catálogo documental completo · la constitución de empresas, que
requiere criterio humano · la integración con notarías · el sitio comercial · la
operación posterior a la entrega.

## Restricciones conocidas

La integración de firma depende de la API que disponga el proveedor. El proveedor
de medios de pago no está definido por la contraparte. La planilla a migrar
contiene datos personales reales y debe anonimizarse antes de usarse en
desarrollo. El proyecto se ejecuta en una ventana de 18 semanas con un equipo de
tres personas.

## Cómo sabremos que funcionó

1. La información de clientes está en la base relacional y las planillas dejaron
   de ser el sistema de registro.
2. Un cliente completa el domicilio tributario de punta a punta sin intervención
   de un ejecutivo.
3. Al menos tres documentos adicionales operan habiendo sido incorporados solo
   por configuración.
4. Toda acción sobre información de clientes queda registrada y el registro no
   admite alteración.
5. Existe una medición del tiempo del trámite en la plataforma, comparable con la
   línea base de tres a cuatro horas.
6. La solución está desplegada, documentada y validada por la contraparte.

El punto 5 es una medición, no un valor comprometido. El proyecto no promete un
porcentaje de reducción: promete medirlo.

---

| Versión | Fecha | Descripción |
|---|---|---|
| 1.0 | 08-09-2026 | Emisión inicial |
| 2.0 | 24-09-2026 | Reformulado a dos páginas; incorporación de las especificaciones de la contraparte; alcance actualizado |
