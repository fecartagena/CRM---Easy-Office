# Registro inicial de riesgos

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | RSK-001 · Registro inicial de riesgos |
| **Versión** | 1.3 |
| **Fecha** | 12 de octubre de 2026 |
| **Revisión** | Al cierre de cada sprint |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## Escala

**Probabilidad e impacto:** Alta / Media / Baja

**Nivel:** resultado de cruzar ambas. La matriz cubre las nueve combinaciones:

| | Impacto Bajo | Impacto Medio | Impacto Alto |
|---|---|---|---|
| **Probabilidad Alta** | Medio | Alto | Crítico |
| **Probabilidad Media** | Bajo | Medio | Alto |
| **Probabilidad Baja** | Bajo | Bajo | Medio |

**Estado:** Abierto · Mitigado · Materializado · Cerrado

**Disparador:** el hecho observable que indica que el riesgo se está
materializando y que corresponde activar la contingencia.

**Última revisión:** 12-10-2026, tras la integración de los pagos. **Próxima revisión:** cierre del sprint en curso.

---

## Matriz

| ID | Riesgo | Categoría | Prob. | Impacto | Nivel | Disparador | Mitigación | Plan de contingencia | Responsable | Estado |
|---|---|---|---|---|---|---|---|---|---|---|
| R-01 | La planilla Excel no se recibe a tiempo | Dependencia externa | Media | Alto | Alto | No llega al cierre del hito H2 | Solicitud formal el 11-09-2026; fecha límite fijada en el hito H2 · 08-10-2026: recibidas la estructura (34 campos) y una muestra; falta la planilla completa anonimizada | Diseñar el modelo de datos sobre la estructura descrita en el levantamiento y construir el proceso de migración contra datos sintéticos. La carga real se ejecuta cuando llegue el archivo | Fernando | Mitigado |
| R-02 | Las plantillas documentales no se reciben a tiempo | Dependencia externa | Media | Alto | Alto | No llegan al cierre del hito H2 | Solicitud formal el 11-09-2026 junto con R-01; se pide al menos la del trámite prioritario · 08-10-2026: recibidas las plantillas del trámite piloto y de otros cuatro documentos; falta el poder simple | Construir el motor con una plantilla de prueba redactada por el equipo a partir de los campos descritos, y sustituirla al recibir la original | Fernando | Mitigado |
| R-03 | Las credenciales del proveedor de firma no se habilitan | Dependencia externa | Alta | Medio | Alto | Sin respuesta del proveedor dos semanas después de la solicitud | Interfaz `SignatureService` con implementación simulada que cumple el mismo contrato; el flujo se construye y prueba sin depender del proveedor · 08-10-2026: Easy Office gestiona las credenciales de prueba. Plan B: firma asistida, en que el ejecutivo firma en la plataforma del proveedor y carga el documento firmado | Entregar el flujo demostrado con la implementación simulada, documentando que la sustitución por la real es un cambio de configuración | Nicolás | Abierto |
| R-04 | El proveedor de pago no se define durante el proyecto | Dependencia externa | Media | Medio | Medio | Sin decisión al inicio del sprint que incluye la integración | Interfaz `PaymentService` desacoplada; Transbank en ambiente de integración como implementación de referencia · 08-10-2026: candidatos Klap (Easy Office) y Transbank (equipo); se diseñan ambas implementaciones detrás de `PaymentService` · 12-10-2026: el equipo integró Klap y Flow; falta la decisión de Easy Office · 12-10-2026: ambos adaptadores probados con pagos de prueba, por lo que el riesgo remanente es solo de decisión y de credenciales productivas | Entregar con el medio que Easy Office elija; el otro queda desactivado por configuración | Nicolás | Abierto |
| R-05 | Exposición de datos personales reales en el repositorio público | Cumplimiento y seguridad | Baja | Alto | Medio | Aparece un archivo con datos reales en un Pull Request | Anonimización de la planilla antes de ingresar a cualquier entorno; regla de Definition of Done que prohíbe versionar datos reales; secretos en variables de entorno | Purgar el historial del repositorio y rotar credenciales si ocurre; notificar a la contraparte | Nicolás | Abierto |
| R-06 | Alcance demasiado amplio respecto del tiempo disponible | Gestión | Alta | Alto | Crítico | Dos sprints consecutivos cierran sin completar lo comprometido | Alcance declarado en `ALC-001` y congelado en el hito H2; priorización MoSCoW; regla de que el MVP no crece por adición | Reducir el número de documentos adicionales configurados, manteniendo el trámite prioritario completo | Fernando | Abierto |
| R-07 | Baja disponibilidad de la contraparte para validar | Dependencia externa | Media | Alto | Alto | Una validación de cierre de sprint se posterga más de una semana | Validaciones agendadas al cierre de cada sprint, con acta; agendamiento anticipado de la reunión de validación (hito H2) | Avanzar con supuestos explícitamente documentados como tales y registrarlos para validación posterior | Fernando | Abierto |
| R-08 | Dedicación parcial del equipo al proyecto | Recursos | Alta | Medio | Alto | Un integrante no registra actividad en el repositorio durante un sprint | Sprints de dos semanas; seguimiento asíncrono tres veces por semana; responsables por historia; revisión cruzada de Pull Requests | Redistribuir las historias del sprint en curso y bajar de prioridad los elementos Could | Marcos | Abierto |
| R-09 | Cambios tardíos de requerimientos | Gestión | Media | Alto | Alto | Una solicitud nueva llega después del congelamiento del alcance | Procedimiento de control de cambios de `ALC-001`: toda solicitud entra por el backlog y desplaza explícitamente otro elemento | Registrar el cambio como diferido al backlog posterior a la entrega del MVP si compromete un hito | Fernando | Abierto |
| R-10 | Las plantillas reales exigen fidelidad de formato que el motor no reproduce | Técnico | Media | Alto | Alto | Al recibir las plantillas, la contraparte exige reproducción exacta del formato | Definir la tecnología de generación documental recién al recibir las plantillas; consultarlo expresamente en la reunión de validación (hito H2) · 08-10-2026: las plantillas son Word con campos de combinación; se genera desde el formato original | Adoptar generación desde el formato original de la plantilla en lugar de un formato propio, aunque implique mayor esfuerzo | Nicolás | Mitigado |
| R-11 | La firma desasistida no resulta viable en los términos descritos | Legal y técnico | Media | Medio | Medio | El proveedor confirma que la modalidad no aplica al caso de uso | Consultar al proveedor y a la contraparte antes de diseñar el flujo de firma; no comprometerla en la presentación | Mantener el flujo con firma iniciada manualmente por el titular del certificado, como opera hoy la empresa | Fernando | Abierto |
| R-12 | El hosting del ambiente productivo no se define | Dependencia externa | Media | Medio | Medio | Sin definición al cierre del hito H3 | Entorno contenerizado y portable; definición conjunta comprometida para el hito H3 · 08-10-2026: sigue sin definición | Desplegar en un proveedor gratuito o de bajo costo a nombre del equipo, documentando la migración posterior | Marcos | Abierto |
| R-13 | La calidad de los datos del Excel impide una migración limpia | Técnico | Media | Medio | Medio | Más del 10% de los registros queda en cuarentena en la primera corrida | Perfilado de los datos antes de migrar; registros inconsistentes a cuarentena en vez de carga parcial | Migrar el subconjunto válido y entregar el reporte de inconsistencias para que la contraparte lo corrija | Nicolás | Abierto |
| R-14 | Sobreingeniería del motor configurable en desmedro de funcionalidad demostrable | Técnico | Media | Alto | Alto | El motor lleva un sprint completo sin generar ningún documento real | Implementar primero el trámite prioritario de forma directa y extraer el motor a partir de casos concretos; no construir editor visual de flujos | Entregar los documentos implementados aunque el motor quede parcialmente generalizado, priorizando funcionalidad sobre abstracción | Nicolás | Abierto |
| R-15 | Dos implementaciones de pago y la cuenta obligatoria del cliente aumentan el esfuerzo del MVP | Gestión | Media | Medio | Medio | El sprint del portal cierra sin el flujo de pago completo | Klap y Flow ya están integrados detrás de la misma interfaz; la cuenta del cliente y el flujo de pago con ambos proveedores ya están implementados y probados en sandbox; el esfuerzo restante es conectar el pago con el trámite del domicilio tributario | Entregar con una pasarela real y la segunda desactivada por configuración | Nicolás | En seguimiento |
| R-16 | Los precios cargados en el catálogo de ventas no coinciden con los de Easy Office | Dependencia externa | Alta | Medio | Alto | Se publica el portal con valores sin confirmar | Los servicios llevan `precio_confirmado = falso` y el portal los rotula como «Valores de ejemplo»; los valores cargados (14.900 y 3.500) difieren de los de los contratos vigentes (59.990 y 5.900) | Easy Office entrega la lista de precios vigente antes de abrir el portal | Fernando | Abierto |
| R-17 | No existe política de reembolsos y el adaptador de Flow no soporta la operación | Producto y legal | Media | Medio | Medio | Un cliente pide devolución de un pago | Se registra como pendiente en `ACTA-002`; el adaptador de Klap ya soporta el reembolso | Gestionar los reembolsos de forma manual en el panel del proveedor mientras se define la política | Nicolás | Abierto |
| R-18 | El túnel de desarrollo expone la API local a internet | Seguridad | Baja | Medio | Bajo | Uso de datos reales en un entorno local | Solo se usan datos ficticios en local; las llaves viven en archivos ignorados por Git; el túnel se renueva solo y no usa el socket de Docker | Apagar el servicio `tunnel` y usar un túnel manual | Nicolás | Mitigado |

---

## Riesgos de nivel crítico o alto que requieren seguimiento en cada sprint

- **R-06** Alcance. Es el único riesgo crítico y depende íntegramente del equipo.
- **R-03, R-07** Dependencias de la contraparte que siguen abiertas tras la
  reunión del 08-10-2026. R-01 y R-02 pasan a mitigados con los documentos
  recibidos.
- **R-05** Datos personales. Se controla mediante la Definition of Done.
- **R-14** Sobreingeniería. Se controla con el orden de implementación acordado.

---

## Riesgos descartados

Se dejan registrados para evitar que se agreguen sin fundamento:

| Riesgo | Por qué se descarta |
|---|---|
| Volumen de datos o rendimiento de la base | La contraparte declaró 200 a 250 clientes mensuales. El volumen no representa un desafío técnico |
| Concurrencia de usuarios | La empresa opera con un equipo reducido de ejecutivos |
| Escalabilidad horizontal | Fuera del alcance y del volumen del problema |

---

## Historial

| Versión | Fecha | Cambios |
|---|---|---|
| 1.0 | 10-09-2026 | Registro inicial a partir del levantamiento y de la planificación |
| 1.1 | 24-09-2026 | Matriz de nivel completada con las nueve combinaciones; niveles recalculados; incorporación de la columna de disparador y de la fecha de revisión |
| 1.2 | 09-10-2026 | Revisión tras `ACTA-002`: R-01, R-02 y R-10 mitigados; R-03, R-04 y R-12 actualizados; nuevo R-15 |
| 1.3 | 12-10-2026 | Estado de la integración de pagos: R-04 y R-15 actualizados; nuevos R-16 (precios), R-17 (reembolsos) y R-18 (túnel de desarrollo) |
