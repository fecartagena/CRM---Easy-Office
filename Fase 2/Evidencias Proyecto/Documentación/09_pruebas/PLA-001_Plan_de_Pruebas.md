# Plan de pruebas

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | PLA-001 · Plan de pruebas |
| **Versión** | 1.11 |
| **Fecha** | 27 de octubre de 2026 |
| **Estado** | Emitido para revisión |
| **Preparado por** | Equipo de proyecto |

---

## 1. Objetivo y alcance

Definir cómo se verifica que la solución cumple lo especificado, en qué niveles,
con qué herramientas y qué evidencia se produce.

El plan cubre los cuatro niveles exigidos: **unitarias, integración, rendimiento y
seguridad**, más las pruebas de aceptación con la contraparte.

Las pruebas se ejecutan **dentro de cada sprint**, no al cierre del proyecto. Una
historia sin sus pruebas no cumple la Definition of Done y no se considera
terminada.

---

## 2. Estrategia

| Principio | Implicancia |
|---|---|
| La prueba acompaña a la historia | Se escribe en el mismo Pull Request, no después |
| La prueba se ejecuta automáticamente | Integración continua en cada Pull Request |
| Una prueba que no falla nunca no prueba nada | Se verifica que la prueba falle antes de implementar la funcionalidad |
| Sin datos reales | Todos los juegos de datos son sintéticos |
| Cobertura dirigida, no numérica | Se prioriza la lógica de negocio sobre el porcentaje total |

### Qué se prueba con prioridad

1. Reglas de validación y de negocio
2. Control de acceso por rol
3. Generación de documentos y congelamiento de datos
4. Máquina de estados de los trámites
5. Idempotencia de los eventos externos de pago y firma
6. Proceso de migración

### Qué no se prueba

Código de terceros, el panel de administración generado por el framework, y las
implementaciones simuladas de proveedores externos, que existen para probar el
resto.

---

## 3. Niveles de prueba

### 3.1 Pruebas unitarias

| | |
|---|---|
| **Objeto** | Funciones y métodos aislados: validaciones, cálculos de vigencia, transiciones de estado, construcción del documento |
| **Responsable** | Quien implementa la historia |
| **Frecuencia** | En cada Pull Request |
| **Herramienta** | `pytest` con `pytest-django` |
| **Criterio de término** | Toda lógica de negocio nueva tiene prueba y pasa |

Casos representativos:

| Caso | Verifica |
|---|---|
| Validación de RUT con dígito verificador correcto e incorrecto | RF-06 |
| Rechazo de guardado con campos obligatorios ausentes | RN-16 |
| Cálculo de fecha de término a partir de inicio y duración | RN-07 |
| Generación de alertas a 60, 30, 15 y 7 días | RN-09 |
| Transición de estado no permitida es rechazada | RN-14 |
| Documento emitido conserva sus datos tras modificar la ficha del cliente | RN-15 |
| Detección de duplicidad por RUT existente | RN-04 |

### 3.2 Pruebas de integración

| | |
|---|---|
| **Objeto** | Flujos completos que atraviesan varios módulos y la base de datos |
| **Responsable** | Equipo |
| **Frecuencia** | Al cierre de cada sprint |
| **Herramienta** | `pytest` con base de datos de prueba en contenedor |
| **Criterio de término** | El flujo completo del servicio piloto se ejecuta de extremo a extremo |

Casos representativos:

| Caso | Verifica |
|---|---|
| Contratación completa: selección, datos, validación, documento, pago, firma, registro | UC-01 |
| La contratación crea el cliente sin duplicarlo si ya existe | RF-29 |
| El servicio contratado queda registrado con su vigencia | RF-30 |
| El documento firmado queda asociado al cliente y a la operación | RF-31 |
| Una notificación de pago duplicada no genera un segundo registro | RF-22, RF-23 |
| Una notificación de pago con firma falsa, con monto distinto o posterior a un rechazo no altera el estado de la venta | RF-23 |
| Pagar con otro medio cancela la orden abierta del anterior y no duplica la venta | RF-48 |
| Un cliente solo ve y paga sus propias ventas | RF-49 |
| Una notificación de firma fuera de orden se procesa correctamente | IN-01 |
| La migración es repetible sin duplicar registros | RF-43 |
| Los registros inconsistentes quedan en cuarentena y no se cargan parcialmente | RF-44 |

### 3.3 Pruebas de rendimiento

| | |
|---|---|
| **Objeto** | Comportamiento bajo el volumen declarado por la contraparte |
| **Responsable** | Nicolás Zapata |
| **Frecuencia** | Sprint 5 y antes de la entrega |
| **Herramienta** | `locust` |
| **Criterio de término** | Los tiempos de respuesta se mantienen dentro de los umbrales definidos |

Escenarios y umbrales:

| Escenario | Volumen | Umbral |
|---|---|---|
| Búsqueda de clientes | 5.000 clientes en base | Respuesta bajo 1 segundo |
| Listado de servicios con filtros | 10.000 servicios contratados | Respuesta bajo 2 segundos |
| Dashboard de indicadores | Mismo volumen | Respuesta bajo 3 segundos |
| Contratación concurrente | 10 operaciones simultáneas | Sin errores ni bloqueos |
| Detección de vencimientos | 10.000 servicios | Proceso completo bajo 5 minutos |

Los volúmenes proyectan varios años de operación sobre la cartera declarada de 200
a 250 clientes mensuales. Los umbrales son propuestos por el equipo y están
`[PENDIENTES DE VALIDAR CON EASY OFFICE]`.

### 3.4 Pruebas de seguridad

| | |
|---|---|
| **Objeto** | Control de acceso, exposición de datos y gestión de secretos |
| **Responsable** | Marcos Álvarez |
| **Frecuencia** | Control de acceso en cada sprint; revisión completa en sprint 5 |
| **Herramienta** | `pytest` para control de acceso; `bandit` y `pip-audit` en integración continua |
| **Criterio de término** | Ningún caso de acceso no autorizado tiene éxito; sin vulnerabilidades críticas en dependencias |

Casos representativos:

| Caso | Verifica |
|---|---|
| Un ejecutivo intenta eliminar un cliente | RF-04, RN-32 |
| Un ejecutivo intenta consultar el registro de auditoría | RN-31 · automatizado (HU-30) |
| Un usuario no autenticado accede a un endpoint protegido | RS-01 |
| Un cliente del portal intenta consultar el expediente de otro | RS-02 |
| Intento de modificación de un registro de auditoría | RF-16, RN-35 · automatizado (HU-30) |
| Búsqueda de credenciales en el historial del repositorio | RS-08 |
| Revisión de vulnerabilidades conocidas en dependencias | RS-03 |
| Validación de que las cookies de sesión son httpOnly | AD-03 |

### 3.5 Pruebas de aceptación

| | |
|---|---|
| **Objeto** | Que la solución resuelve lo que la contraparte pidió |
| **Responsable** | Fernando Cartagena, con Easy Office |
| **Frecuencia** | Al cierre de sprint, según disponibilidad de la contraparte |
| **Criterio de término** | Los diez criterios de éxito del encargo se verifican |

| # | Criterio declarado por la contraparte | Cómo se verifica |
|---|---|---|
| 1 | Información centralizada y trazable | Demostración de la ficha del cliente y su historial |
| 2 | Los ejecutivos usan el CRM sin conocimientos técnicos | Sesión de uso con un ejecutivo real |
| 3 | Los permisos impiden acciones no autorizadas | Demostración con dos usuarios de distinto rol |
| 4 | Registro de las acciones relevantes | Consulta de auditoría sobre un registro modificado |
| 5 | La información del cliente no requiere nueva digitación | Contratación desde el portal y revisión en el CRM |
| 6 | Los servicios con vencimiento generan alertas | Demostración con fechas simuladas |
| 7 | Los documentos se generan automáticamente | Contratación completa del servicio piloto |
| 8 | La firma se integra al flujo | Demostración del flujo con el proveedor |
| 9 | Se pueden incorporar nuevos servicios y documentos | Alta de un servicio nuevo solo por configuración, en vivo |
| 10 | Mecanismos adecuados de seguridad y respaldo | Revisión de controles y prueba de restauración |

---

## 4. Medición del resultado del proyecto

Además de verificar el funcionamiento, se mide el objetivo declarado.

| Métrica | Línea base | Método |
|---|---|---|
| Tiempo del trámite | 3 a 4 horas, declarado por la contraparte | Registro del tiempo desde el inicio de la solicitud hasta la descarga del documento, sobre al menos diez operaciones de prueba |
| Operaciones sin intervención de ejecutivo | 0% | Proporción de trámites con canal de origen "portal" completados sin acción manual |
| Reingreso manual de datos | En cada operación | Verificación de que el registro del CRM proviene íntegramente del portal |

Estas son mediciones a realizar, no resultados comprometidos.

---

## 5. Entornos

| Entorno | Propósito | Datos |
|---|---|---|
| Local | Desarrollo | Sintéticos |
| Integración continua | Ejecución automática en cada Pull Request | Sintéticos, generados por el propio conjunto de pruebas |
| Pruebas | Integración, rendimiento y aceptación | Sintéticos, con volumen representativo |
| Producción | Operación | Reales, con respaldo activo |

**Ningún entorno distinto de producción opera con datos personales reales.** La
planilla que entregue la contraparte se anonimiza antes de cualquier uso.

---

## 6. Evidencia

Cada sprint produce y versiona:

| Evidencia | Ubicación |
|---|---|
| Resultado de la ejecución de pruebas | Registro de la integración continua |
| Reporte de cobertura | `09_pruebas/sprint-N/` |
| Registro de defectos encontrados y su resolución | Issues del repositorio |
| Acta de la demostración de cierre de sprint | `11_actas/` |
| Reporte de rendimiento | `09_pruebas/rendimiento/` |
| Reporte de seguridad | `09_pruebas/seguridad/` |

---

## 7. Gestión de defectos

| Severidad | Definición | Tratamiento |
|---|---|---|
| Crítica | Impide operar o expone datos | Se corrige antes de cerrar el sprint |
| Alta | Funcionalidad principal no cumple su criterio de aceptación | Se corrige en el sprint o se devuelve la historia al backlog |
| Media | Comportamiento incorrecto con alternativa disponible | Se planifica en el sprint siguiente |
| Baja | Cosmético o menor | Backlog |

Todo defecto se registra como issue, referenciando la historia de origen.

---

## 8. Criterios de entrada y salida

**Entrada a la fase de pruebas de un sprint:** las historias comprometidas están
implementadas, el entorno levanta desde cero y las pruebas unitarias pasan.

**Salida:** no hay defectos críticos ni altos abiertos, los criterios de
aceptación de cada historia se cumplen, la evidencia está versionada y la
matriz de trazabilidad está actualizada con las pruebas ejecutadas.

---

## 9. Estado actual

Sprint 1 cerrado el 3 de octubre con las primeras pruebas de control de acceso
por rol (HU-45). Este documento se actualiza con la evidencia de cada iteración.

**Estado al 12-10-2026**

| Repositorio | Pruebas automatizadas | Herramientas de calidad |
|---|---|---|
| Backend | 214 (213 en la rama principal; la rama del túnel suma una) | `ruff` y `makemigrations --check` sin hallazgos; el esquema OpenAPI valida sin advertencias |
| Frontend | 97 (`vitest`) | `tsc`, `oxlint` y `vite build` sin hallazgos; sin integración continua configurada |

Cobertura funcional ya automatizada para cuentas y pagos:

- Registro del cliente y vinculación por RUT, con error genérico ante rechazos.
- Adaptadores de Klap y Flow con transporte HTTP simulado.
- Estados y transiciones de la orden y de la venta; idempotencia.
- Tareas asíncronas de creación, sincronización y procesamiento de notificaciones.
- Webhooks: duplicados, firmas falsas, rechazo tardío y monto distinto; confirmación asíncrona de Flow; retorno del navegador.
- Propiedad de los registros y matriz de permisos; caída del túnel.
- En el frontend: guardias de ruta, registro, selector de medio, checkout de Klap y de Flow, pantallas de pago, resultado y Ventas.

Pruebas manuales en ambiente de pruebas de los proveedores:

| Prueba | Resultado |
|---|---|
| Pago exitoso con Klap | Correcto |
| Cancelación de un pago de Klap (cierre de la ventana) | Correcto |
| Pago exitoso con Flow (Webpay) | Correcto |
| Pago de Klap aprobado y luego reversado | Detectó que el ambiente de pruebas repite el identificador de comercio en todos los pagos; se corrigió quitando la unicidad de `Pago.id_externo` |

**Estado al 17-10-2026 (domicilio tributario: backend PR #51 y frontend PR #18, fusionados)**

- 498 pruebas del backend pasan; `ruff`, el esquema OpenAPI y
  `makemigrations --check` quedan sin observaciones.
- 123 pruebas del frontend pasan; `typecheck` y `lint` limpios. Cubren que no
  aparezcan datos de ejemplo ni campos de dirección, el prellenado con los datos
  del propio cliente y los campos vacíos, el RUT, la forma del envío, los estados
  de la vista previa, los totales del pago por plan, el estado del trámite y la
  subida del firmado en el backoffice con sus permisos.
- Verificado en el navegador con datos ficticios: formulario, PDF real de tres
  páginas, edición del borrador (versión 2 con el mismo folio), confirmación,
  pago aplicado por el mismo camino que un webhook, firma subida por un ejecutivo
  y trámite completado con el contrato vigente.
- Verificado manualmente por el equipo desde el entorno local: el checkout en el
  ambiente de pruebas del proveedor de pago con los planes nuevos, con el resultado
  recibido por webhook a través del túnel.
- No verificado: la fidelidad del PDF frente al Word original.

**Corrección de la sesión del panel de administración (PR #52, fusionado)**

- 5 pruebas nuevas: las sesiones del panel y de la aplicación web conviven en
  ambos órdenes; una sesión web del personal no da acceso al panel; cerrar sesión
  en el panel mantiene la sesión web; nombre, ruta y HttpOnly de la cookie del
  panel.
- La suite completa del backend pasa: 509 pruebas. Verificado además contra el
  servidor local con cuentas sintéticas temporales.
- Defecto conocido, sin relación con este cambio: una prueba de búsqueda del
  panel de clientes falla al ejecutarse sola, también en la rama principal, y
  pasa en la suite completa. Queda registrada para corregirla.

**Cambio de sesión entre personal y cliente (frontend, PR #19, fusionado)**

- Pruebas nuevas de guardias, ingreso y registro: el aviso ofrece la acción
  correcta en ambos sentidos y, al usarla, cierra la sesión y lleva al ingreso
  correcto volviendo a la página original; las pantallas de ingreso y registro
  solo redirigen una sesión del mismo tipo.
- 128 pruebas del frontend pasan; `typecheck` y `lint` limpios.
- Verificado en el navegador contra el entorno local, con cuentas sintéticas
  temporales: cambio de personal a cliente desde el aviso y desde el ingreso.

**Dashboard con datos reales (backend PR #54 y frontend PR #21, fusionados)**

- Pruebas nuevas de vencimientos de contratos, indicadores de trámites,
  documentos pendientes de firma y ventas pagadas; la API del dashboard se
  reescribe para cubrir la forma de la respuesta, las cifras reales, la fecha de
  referencia, el acceso por rol, el Excel sin "Valor de ejemplo" y un número de
  consultas acotado.
- 545 pruebas del backend pasan.
- Frontend: el panel no muestra textos de ejemplo; muestra la fecha de las cifras,
  el desglose de pendientes por motivo, el grupo "En línea (sin ejecutivo)", la
  nota de que solo cuentan las ventas pagadas en línea y el estado vacío. 131
  pruebas del frontend pasan; `typecheck` y `lint` sin errores.
- Verificado en el navegador como Administrador y como Ejecutivo con datos locales
  ficticios.

**Bodega virtual con `PLT-02` (backend PR #60 y frontend PR #23, fusionados el 21-10-2026)**

- Pruebas nuevas o ampliadas:
  - requisitos de la bodega y de su autorizante (`inmuebles`);
  - ocupación de inmuebles (`contratos`);
  - generación de `PLT-02`, API de la bodega y formato de las duraciones
    (`documentos`);
  - trámite de bodega en el backoffice y en el portal (`tramites`);
  - siembra de los planes de la bodega (`ventas`).
- Se ajustaron las pruebas de `ventas` que suponían que la bodega no se vendía en
  línea.
- 637 pruebas del backend pasan, incluidas las 546 que existían antes del cambio.
  Ruff y `makemigrations --check` sin observaciones.
- Verificado en local con Celery y Gotenberg, por la API: el trámite del
  backoffice completo hasta el PDF firmado, y en el portal el plan trimestral
  hasta la venta de 59.990. Los PDF salieron con todos los campos llenos.
- No verificado: un pago real con Klap o Flow para la bodega (cubierto con el
  proveedor simulado) ni las pantallas en el navegador. CI del PR #60: el linter
  pasó y pytest seguía en curso al abrir el PR.
- Frontend: 150 pruebas pasan, incluidas las 131 anteriores; `tsc -b` y oxlint
  sin observaciones.
  - Nuevas: la pantalla "Nuevo trámite" del backoffice, incluido el caso con
    planes; el formulario genérico del portal; los casos de bodega en el detalle
    del trámite y la tarjeta de bodega del catálogo.
  - Ajustadas: el estado del trámite, que ahora consulta el formulario genérico,
    y los datos de prueba, que suman el tipo de trámite, las líneas del contrato y
    los formularios de bodega.
- Defecto conocido: una prueba de "Nuevo trámite" falló una vez en una corrida
  completa y pasó en las tres siguientes. Parece un problema de tiempo bajo carga,
  sin confirmar.
- Pendiente: recorrer en el navegador los flujos de la bodega en el portal y en el
  backoffice.

**Auditoría (HU-30 y API de HU-31, issue #61, PR #62 abierto)**

- 59 pruebas nuevas; 696 del backend pasan sobre una base creada desde cero.
  `ruff` y `makemigrations --check` sin observaciones. Las migraciones se
  revirtieron y se volvieron a aplicar en la base local.
- Cubren:
  - creación, modificación y eliminación con el usuario, la IP y el origen
    correctos desde la API del personal, el portal, el admin y una tarea de
    Celery;
  - que una modificación sin cambios no registre nada;
  - que la contraseña y los demás campos excluidos nunca aparezcan;
  - que `UPDATE`, `DELETE` y `TRUNCATE` directos fallen, y que el modelo y el
    admin no permitan modificar ni eliminar;
  - que el contexto no pase de una petición a la siguiente en la misma conexión,
    y que una escritura fuera de transacción quede sin usuario;
  - la matriz de acceso de la API (Administrador 200; Ejecutivo, cliente y
    anónimo 403), los filtros, el historial agrupado y el número de consultas;
  - ingreso, ingreso fallido, cierre de sesión, descarga de documentos y acceso a
    la cédula;
  - que los disparadores de la base coincidan con la lista de tablas auditadas y
    que todo modelo esté declarado como auditado o no auditado.
- Verificado en la base local: un cambio a un cliente aparece en su historial con
  usuario, fecha, valor anterior y nuevo; el Ejecutivo recibe 403; un `DELETE`
  directo en psql falla.
- Pendiente: las pantallas de consulta (HU-31, frontend).

**Firma avanzada con el PSC de pruebas (backend PR #68 y frontend PR #27, fusionados)**

- Backend (CRM): 685 pruebas pasan en `main` (637 previas y 48 nuevas). Las 696
  de la auditoría corresponden a la rama del PR #62, todavía sin fusionar.
  - Unitarias (§3.1): el adaptador HTTP con `httpx.MockTransport`, el contrato de
    firma y el registro de proveedores.
  - Integración (§3.2): webhook firmado, deduplicado y fuera de orden; envío,
    rechazo, expiración, sincronización periódica, reenvío, cancelación,
    reintento y permisos; cierre con PDF, hash, estado y contrato vigente; RF-39
    con prefijo y hash distintos; un trámite de domicilio iniciado en el portal y
    pagado también va al proveedor y se completa; el nivel `asistida` no cambia.
  - Seguridad (§3.4): HMAC y ventana de tiempo; la configuración de producción
    rechaza la URL del PSC; el compose de producción no lo incluye; no hay
    imports cruzados entre el CRM y el PSC.
- PSC de pruebas: 62 pruebas. PKI y rotación; fallos de identidad y bloqueo;
  vencimiento e intentos del código; orden de firmantes; cada firma valida contra
  la raíz de pruebas y las anteriores siguen validando; un PDF alterado falla;
  sello de tiempo y revocación embebidos; API key y HMAC obligatorios; reintentos
  de webhooks; se niega a iniciar con configuración de producción.
- Punta a punta (`just e2e-firma`): pasó en local con todo el stack, con tres
  firmantes, códigos leídos en Mailpit, webhooks, trámite completado, contrato
  vigente y PDF final validado contra la raíz de pruebas. En CI corre solo en los
  PR que tocan la firma, más ejecución manual.
- No verificado: el resultado final de los workflows de CI del PR #68 antes de la
  integración (`linter` y `firma-sandbox` en verde; `pytest` y `e2e` en curso en
  la última revisión). Falta adjuntar el enlace a la ejecución como evidencia.

Pendiente: pruebas de rendimiento, el resto de las pruebas de seguridad, las
pruebas de aceptación con Easy Office y las evidencias reproducibles de cada
ejecución (fecha, commit, entorno y resultado).

---

## Control de cambios

| Versión | Fecha | Descripción | Autor |
|---|---|---|---|
| 1.0 | 24-09-2026 | Emisión inicial | Equipo de proyecto |
| 1.1 | 12-10-2026 | Estado actual con las pruebas de cuentas y pagos; nuevos casos de integración | Equipo de proyecto |
| 1.2 | 17-10-2026 | Estado de las pruebas del domicilio tributario de punta a punta | Equipo de proyecto |
| 1.3 | 17-10-2026 | Pruebas del frontend del domicilio tributario (123) y estado fusionado | Equipo de proyecto |
| 1.4 | 17-10-2026 | Pruebas de la sesión separada del panel de administración (509 en total) y defecto conocido de una prueba aislada | Equipo de proyecto |
| 1.5 | 17-10-2026 | Pruebas del cambio de sesión entre personal y cliente (128 en el frontend) | Equipo de proyecto |
| 1.6 | 18-10-2026 | Pruebas del dashboard con datos reales (545 en el backend, 131 en el frontend) | Equipo de proyecto |
| 1.7 | 20-10-2026 | Estados de los PR de sesión y del dashboard: fusionados | Equipo de proyecto |
| 1.8 | 21-10-2026 | Pruebas de la bodega virtual con `PLT-02` (637 en el backend, 150 en el frontend) y un defecto conocido de tiempo en una prueba del frontend | Equipo de proyecto |
| 1.9 | 22-10-2026 | Estado de los PR de la bodega virtual: fusionados | Equipo de proyecto |
| 1.10 | 22-10-2026 | Pruebas de la auditoría (696 en el backend, PR #62 abierto); dos casos de seguridad de §3.4 automatizados | Equipo de proyecto |
| 1.11 | 27-10-2026 | Pruebas de la firma avanzada con el PSC de pruebas (685 en el backend en `main`, 62 del PSC y la prueba de punta a punta) | Equipo de proyecto |
