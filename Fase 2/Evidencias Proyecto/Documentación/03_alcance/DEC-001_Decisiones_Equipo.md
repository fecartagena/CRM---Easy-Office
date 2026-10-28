# Decisiones del equipo ante la participación limitada de Easy Office

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | DEC-001 · Decisiones del equipo |
| **Versión** | 1.0 |
| **Fecha** | 27 de octubre de 2026 |
| **Estado** | **Borrador para adopción.** Cada decisión está en estado *Propuesta* hasta que el equipo la adopte en la sección 5, con fecha y revisor reales |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## 1. Antecedente

Fernando Cartagena informó al equipo que la docente indicó que, si Easy Office no
participa completamente en las conversaciones, el equipo puede definir algunos
aspectos con su propio criterio. **No hay registro escrito ni fecha de esa
indicación.** Se conserva como antecedente reportado por un integrante; no es un
acta ni una aprobación del cliente.

Con ese antecedente, las definiciones necesarias para avanzar se resuelven con
estos criterios: coherencia con los objetivos del Capstone, simplicidad,
reversibilidad, protección de datos y posibilidad de demostrar el resultado. La
falta de respuesta no detiene el diseño, las pruebas con datos sintéticos, la
documentación ni la aceptación técnica interna.

Una decisión del equipo **no es una condición comercial confirmada**. Tarifas,
acceso a cuentas, cobros reales y operación con datos de clientes se describen
según su estado efectivo.

---

## 2. Procedimiento

1. Identificar la pregunta y su impacto en requisito, historia, interfaz, datos o
   prueba.
2. Revisar las fuentes: actas, especificaciones, código y decisiones anteriores.
   No reabrir algo ya resuelto en un registro más reciente.
3. Elegir una alternativa razonada. Si cambia un objetivo o un entregable,
   registrar el cambio de alcance y su impacto académico.
4. Registrar fecha real, autor y revisor. Indicar si es *decisión del equipo* o
   *supuesto de demostración*.
5. Aplicar la decisión en `RN-001`, `MRQ-001`, `ALC-001`, `FUN-001`, pruebas y
   manuales. Si Easy Office responde después con otra definición, el cambio se
   evalúa con trazabilidad.

**Estados.** Propuesta → Adoptada → Implementada → Verificada. *Confirmada por
Easy Office* es un atributo aparte, que exige su propia fuente. Una decisión
puede estar verificada técnicamente sin estar confirmada comercialmente.

**Identificador.** `DE-nn` (decisión del equipo). No se usa `D-nn`, que en
`CAT-001` corresponde a las dependencias de `SPR-001`, ni `D2-nn`, que son
decisiones de `ACTA-002`.

---

## 3. Decisiones propuestas

La columna *Hoy en el código* indica lo verificado en la rama principal el
27-10-2026 (backend `7869d0a`, frontend `f2e4758`). Que el código ya lo haga no
reemplaza el registro de adopción.

| ID | Decisión propuesta | Justificación y límite | Hoy en el código | Evidencia que la cierra |
|---|---|---|---|---|
| DE-01 | Desarrollar y demostrar con datos sintéticos; ensayar la migración sobre la estructura de 34 columnas disponible | Permite verificar el proceso sin la planilla completa (TA-10). No acredita que la cartera real esté migrada | Hay muestra sintética en `13_plantillas`; no hay importador (`migracion` está vacía) | Ensayo de `MIG-001` con reporte de cuarentena e idempotencia |
| DE-02 | Hacer aceptación técnica interna: revisa un integrante que no implementó el flujo; queda abierta una revisión posterior con Easy Office | Evalúa los requisitos aunque el cliente no asista. No es aceptación comercial ni prueba de usabilidad con ejecutivos reales | — | Acta de aceptación técnica (`EVI-001` §8) |
| DE-03 | Domicilio tributario es el flujo prioritario y bodega el segundo caso | Ambos tienen plantilla y código. Los demás documentos previstos no desaparecen: su selección o ajuste se registra aparte (`DE-04`) | `PLT-01` (PR #51) y `PLT-02` (PR #60) generan de punta a punta | Prueba de punta a punta de ambos (`EVI-T01`, `EVI-T08`, `EVI-T09`) |
| DE-04 | Declarar el motor como **parcialmente configurable** hasta demostrar un alta adicional | Cada plantilla tiene hoy su convertidor y contexto en código. Evita confundir reutilización con incorporar cualquier documento sin programar | `docs/variation-points.md` del backend registra qué difiere entre `PLT-01` y `PLT-02` | `EVI-T11`: alta de un caso acotado, listando qué fue dato, plantilla o código |
| DE-05 | Firma: **asistida** por defecto; **avanzada** demostrable solo contra el PSC de pruebas, sin validez legal; la firma con validez legal queda con un prestador acreditado cuando lleguen las credenciales | El nivel se configura por tipo de trámite. El PSC de pruebas es solo para desarrollo (`ARQ-001` AD-16, Ley 19.799). Ninguna captura ni conclusión presenta la firma de pruebas como firma con validez legal | `configuracion["firma"]` con `asistida` en todos los tipos; firma avanzada contra `firma_sandbox/` (PR #68 y frontend #27) | `just e2e-firma` y pruebas de carga asistida; para la firma legal, credenciales de tufirma.digital (TA-07) |
| DE-06 | Mantener Klap y Flow detrás de la interfaz común e indicar en cada evidencia qué modo se usó | La elección comercial no está hecha (TA-08). El modo simulado reproduce fallos sin depender del sandbox | `PAYMENT_PROVIDER`: simulado, Klap o Flow | Cada evidencia indica `simulated`, Klap sandbox o Flow sandbox; nunca cobros reales |
| DE-07 | Para la demostración, el precio configurado del plan es el total final y se muestra igual en portal y documento | Coherente con RN-44. Contradice el «MÁS I.V.A.» del anexo de `PLT-01` y el «sin IVA» del diccionario de `PLT-000` (TA-16). No redefine la política tributaria de Easy Office | `ServicioContratado.precio_incluye_iva` | Prueba de montos sin doble IVA; la discrepancia queda explícita hasta resolverla |
| DE-08 | Mantener los planes y duraciones actuales; los precios de bodega se identifican como provisionales | Permite probar 3, 6 y 12 meses. No son tarifas confirmadas (TA-15, TA-20) | Catálogo cargado por `seed_demo_bodega` | Catálogo de prueba anotado; revisar ante una lista de precios respaldada |
| DE-09 | Mantener como parámetros técnicos provisionales: inicio hasta 30 días a futuro, adjuntos de hasta 5 MB y PDF firmado de hasta 10 MB | Son reglas presentes en el código; hacen el comportamiento verificable. No se atribuyen a Easy Office (TA-21) | `core/validators.py` (5 MB) y `documentos/services.py` (10 MB para el PDF firmado) | Casos de borde: hoy, +30 y +31 días; archivos en el límite y sobre él |
| DE-10 | Conservar los folios históricos al importar, con su procedencia; no regenerarlos con el contador nuevo. Los contratos nuevos mantienen la regla vigente: el borrador recibe su folio al crearse | Protege la trazabilidad de los documentos ya emitidos. Los conflictos van a cuarentena (`MIG-001`) | El folio solo se asigna en el servicio de creación (`contratos/services.py`); no hay vía para un folio histórico | Casos de colisión, reimportación y folio de borrador (`MIG-T05`) |
| DE-11 | Usar la identidad visual implementada en HU-59, con el logo entregado por Easy Office, como criterio del equipo hasta tener un manual de marca | Permite terminar la interfaz con coherencia. No se presenta como identidad gráfica aprobada por Easy Office | Frontend PR #25 | Capturas y revisión de consistencia entre pantallas |
| DE-12 | Diseñar el despliegue académico en un host Linux con Docker Compose, HTTPS, frontend compilado y proxy hacia la API; elegir el proveedor cuando se conozca disponibilidad y costo | Aprovecha la arquitectura existente y permite cerrar el diseño y el procedimiento ahora (`MOP-001`). No acredita despliegue (TA-09) | `docker-compose.production.yml` con Traefik y nginx | Instalación reproducible; URL y responsable cuando existan. Si se limita a demostración, registrar el ajuste de alcance |
| DE-13 | Adoptar como metas de prueba los umbrales de `PLA-001`, medidos con el percentil 95 donde aplique, y 10 contrataciones concurrentes sin errores inesperados ni folios duplicados | La forma de medir la define el equipo. Son metas académicas, no un acuerdo de nivel de servicio con Easy Office | — | Informe de rendimiento con entorno, carga, percentiles y errores |
| DE-14 | Proponer respaldo diario fuera del host, con objetivos iniciales de pérdida máxima de 24 horas y recuperación en 4 horas; medirlos antes de declararlos logrados | Base operativa verificable. Incluye base de datos, documentos y archivos privados. Retención propuesta: 7 diarios y 4 semanales | Scripts `backup`, `backups` y `restore` de PostgreSQL; no cubren archivos ni copia externa | Ensayo de restauración aislado con tiempos medidos (`MOP-001` §4) |

---

## 4. Temas que necesitan una definición adicional del equipo

- **Bodega compartida o exclusiva.** El código tiene la marca y la configuración
  actual asume uso compartido. Se puede mantener para la demostración probando
  ambas reglas, sin afirmar que Easy Office permite varios ocupantes.
- **Renovación y folio (TA-17).** Propuesta: la renovación es un contrato nuevo,
  con folio nuevo, que apunta al anterior (`Contrato.contrato_anterior`).
  Verificar que el código lo soporte antes de presentarlo como disponible.
- **Cambio o anulación después de pagar (TA-14).** Propuesta académica: revisión
  manual documentada, sin devolución automática y sin borrar el pago original. La
  política de reembolsos sigue siendo comercial.
- **Bodega en el backoffice.** Hoy no crea venta ni pasa por el pago en línea. Se
  describe como canal de gestión con pago externo supuesto y no se suma al panel
  como venta cobrada por la plataforma.

---

## 5. Registro de adopción

Se completa en una sesión del equipo. Una fila por decisión, solo cuando se
adopte o se descarte.

| Decisión | Alternativa elegida | Motivo | Fecha real | Autor | Revisor | Documentos afectados | Evidencia de implementación | Confirmación de Easy Office |
|---|---|---|---|---|---|---|---|---|
| DE-__ | Pendiente de adopción | — | — | — | — | — | — | No disponible |

---

## Historial de versiones

| Versión | Fecha | Cambios |
|---|---|---|
| 1.0 | 27-10-2026 | Versión inicial. Catorce decisiones propuestas, contrastadas con el código de la rama principal. La firma reconoce la firma avanzada contra el PSC de pruebas (PR #68) y el límite de archivos distingue adjuntos (5 MB) de PDF firmado (10 MB) |
