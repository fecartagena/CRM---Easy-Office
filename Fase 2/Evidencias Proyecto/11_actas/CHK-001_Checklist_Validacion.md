# Checklist para la reunión de validación

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | CHK-001 · Checklist para la reunión de validación |
| **Objetivo de la reunión** | Cerrar los requerimientos, validar los prototipos y congelar el alcance del MVP |
| **Duración sugerida** | 60 minutos |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## Antes de la reunión

- [ ] Enviar con antelación `MRQ-001`, `ALC-001` y las capturas de los prototipos
- [ ] Confirmar asistencia y que la contraparte pueda ver pantalla
- [ ] Designar quién toma el acta (propuesto: Fernando)
- [ ] Preparar grabación o registro escrito, ya que la evidencia de la reunión es
      respalda las decisiones de alcance

---

## 1 · Alcance

- [ ] ¿Confirman que el domicilio tributario es el trámite a automatizar primero?
- [ ] ¿Cuántos documentos adicionales esperan ver funcionando al cierre del
      proyecto? ¿Tres, cuatro, o cinco?
- [ ] De la lista priorizada (contrato de servicio, autorización de domicilio
      tributario, contrato de arriendo, orden de compra, declaración jurada),
      ¿cuáles quieren primero y en qué orden?
- [ ] ¿Están de acuerdo con que la creación de empresas quede fuera del MVP y se
      mantenga como proceso asistido?
- [ ] ¿Confirman que la relación con notarías queda fuera?
- [ ] ¿Hay algo en `ALC-001` que consideren mal clasificado?
- [ ] Explicar y acordar el procedimiento de control de cambios: desde esta
      reunión, el MVP no crece por adición

---

## 2 · Datos

- [ ] ¿Nos pueden entregar la planilla Excel? ¿Cuándo?
- [ ] ¿Es una sola planilla o varias? ¿Cuántos registros aproximadamente?
- [ ] ¿Qué columnas tiene? ¿Cuáles son obligatorias en la práctica?
- [ ] ¿Hay campos de texto libre donde se anota información importante?
- [ ] ¿Hay clientes duplicados o registros que sepan que están mal?
- [ ] ¿Contiene datos personales reales? Confirmar que trabajaremos con una
      versión anonimizada en desarrollo
- [ ] ¿Guardan histórico de trámites en la planilla o solo el estado actual?
- [ ] ¿Qué información quieren conservar y qué se puede descartar en la
      migración?

---

## 3 · Documentos y plantillas

- [ ] ¿Nos pueden entregar las plantillas de los documentos priorizados?
- [ ] **¿En qué formato están? ¿Word, PDF, Google Docs?** *(Determina la
      tecnología de generación documental)*
- [ ] **¿El formato visual debe respetarse exactamente, o se puede rediseñar?**
      *(Es la pregunta técnica más importante de la reunión)*
- [ ] ¿Con qué frecuencia cambian las plantillas? ¿Quién las modifica?
- [ ] Para el contrato de domicilio tributario: ¿qué campos varían de un cliente a
      otro? Confirmar la lista completa
- [ ] ¿Qué campos son obligatorios y cuáles opcionales?
- [ ] ¿Hay cláusulas que cambien según el caso o el texto es siempre el mismo?
- [ ] ¿Quiénes firman cada documento priorizado? ¿En qué orden?
- [ ] ¿Los documentos emitidos tienen numeración o folio propio?

---

## 4 · Flujo del trámite

- [ ] Recorrer con ellos el flujo del domicilio tributario paso a paso y
      confirmar que refleja su proceso
- [ ] ¿Qué estados debería tener un trámite? Definir la lista
- [ ] ¿Puede un trámite volver a un estado anterior? ¿En qué casos?
- [ ] ¿Qué pasa si el cliente no confirma el borrador? ¿Hay un plazo?
- [ ] ¿Qué pasa si el cliente pide un cambio después de generado el documento?
- [ ] ¿Qué pasa si el pago no se completa?
- [ ] ¿Qué acciones puede hacer el cliente por su cuenta y cuáles requieren a un
      ejecutivo?
- [ ] ¿Un ejecutivo puede intervenir un trámite automatizado si algo sale mal?
- [ ] ¿Hay casos en que el trámite se cancela o se anula? ¿Quién puede hacerlo?
- [ ] ¿Un contrato de domicilio tributario tiene vigencia? ¿Se renueva?

---

## 5 · Roles y permisos

- [ ] ¿Qué roles existen hoy en la empresa? ¿Ejecutivo, supervisor,
      administrador, otros?
- [ ] ¿Cuántas personas hay en cada rol?
- [ ] ¿Un ejecutivo debe ver todos los clientes o solo los suyos?
- [ ] ¿Quién puede modificar la ficha de un cliente?
- [ ] ¿Quién puede eliminar información? ¿Debería poder alguien?
- [ ] ¿Quién puede configurar un nuevo tipo de trámite?
- [ ] ¿Quién debería poder consultar el registro de auditoría?
- [ ] ¿Hay información que algunos roles no deberían ver?

---

## 6 · Firma electrónica

- [ ] Confirmar que el proveedor es `tufirma.digital`
- [ ] **¿El proveedor ofrece una API para integrar la firma desde otro sistema?**
      *(Esta pregunta quedó sin respuesta en el levantamiento: la consulta sobre la
      API del sitio se respondió en términos de reventa de firmas)*
- [ ] ¿Nos pueden gestionar credenciales de un ambiente de prueba? ¿Con quién hay
      que hablar y cuánto demora?
- [ ] ¿Existe documentación técnica del proveedor a la que podamos acceder?
- [ ] Sobre la firma desasistida: ¿quién es el titular del certificado que firma?
      ¿Cómo se autoriza hoy esa firma automática?
- [ ] ¿Hay documentos que requieran firma del cliente además de la de Easy
      Office?
- [ ] Si un firmante no firma, ¿qué se hace hoy?

---

## 7 · Pago

- [ ] ¿El pago debe formar parte del MVP o puede quedar como flujo simulado?
- [ ] ¿Ya decidieron el proveedor? ¿Mercado Pago, Transbank u otro?
- [ ] Si no está decidido, ¿cuándo lo estará?
- [ ] En el flujo automatizado, ¿el cliente paga antes o después de ver el
      borrador?
- [ ] ¿Se mantiene la modalidad 50/50 en los trámites automatizados o se cobra el
      total?
- [ ] ¿Los precios son fijos por tipo de documento o varían por cliente?
- [ ] ¿Qué pasa si el cliente paga y luego el trámite se cae?
- [ ] ¿Necesitan que el sistema emita algún comprobante?

---

## 8 · Prototipos

Mostrar cada pantalla y pedir confirmación o corrección. Registrar cada
observación en el acta.

- [ ] **Catálogo de servicios**: ¿los servicios mostrados son los correctos? ¿El
      cliente entiende qué está eligiendo?
- [ ] **Formulario de domicilio tributario**: ¿los campos son los correctos?
      ¿Falta alguno? ¿Sobra alguno?
- [ ] **Vista previa del documento**: ¿así esperan que el cliente revise el
      borrador?
- [ ] **Confirmación y pago**: ¿el momento del pago está bien ubicado en el flujo?
- [ ] **Estado del trámite y firmantes**: ¿la información mostrada es suficiente?
- [ ] **Mis trámites**: ¿el cliente necesita ver su historial?
- [ ] **Panel operacional**: ¿qué indicadores les serían realmente útiles? Los
      actuales son una propuesta con datos simulados
- [ ] **Detalle de trámite y auditoría**: ¿el nivel de detalle del registro es el
      adecuado?
- [ ] **Configuración de un tipo de trámite**: presentar el concepto del motor
      configurable y confirmar si les hace sentido que ustedes mismos puedan dar
      de alta documentos nuevos

---

## 9 · Otros temas

- [ ] ¿En qué está construido su sitio web actual y quién lo administra?
- [ ] ¿Prefieren que la plataforma se enlace desde el sitio o que se integre
      dentro de él?
- [ ] ¿Dónde se alojaría el sistema en producción? ¿Tienen hosting propio?
- [ ] ¿Quién administraría la plataforma después de la entrega?
- [ ] Confirmar la meta de reducción de errores mencionada en nuestra
      documentación
- [ ] Aclarar la relación entre cuatro o cinco clientes diarios y 200 a 250
      mensuales

---

## Al cierre de la reunión

- [ ] Recapitular las decisiones tomadas en voz alta antes de terminar
- [ ] Confirmar los compromisos de entrega de la contraparte y sus fechas
- [ ] Acordar la fecha de la próxima validación
- [ ] Enviar el acta dentro de las 48 horas siguientes y pedir confirmación por
      escrito

---

## Después de la reunión

- [ ] Levantar el acta y guardarla en `docs/11_actas/`
- [ ] Actualizar `MRQ-001`: mover a *Confirmado* lo validado y a *Won't* lo
      descartado
- [ ] Actualizar `RN-001` con las reglas confirmadas
- [ ] Congelar `ALC-001` y registrar la versión 2.0
- [ ] Priorizar el backlog `BKL-001` y dejarlo listo para el Sprint 1
- [ ] Actualizar `RSK-001`: cerrar los riesgos resueltos
- [ ] Corregir los prototipos según la retroalimentación
