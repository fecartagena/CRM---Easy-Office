# Plan de pruebas

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | PLA-001 · Plan de pruebas |
| **Versión** | 1.0 |
| **Fecha** | 24 de septiembre de 2026 |
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
| Una notificación de pago duplicada no genera un segundo registro | RF-21 |
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
| Un ejecutivo intenta consultar el registro de auditoría | RN-31 |
| Un usuario no autenticado accede a un endpoint protegido | RS-01 |
| Un cliente del portal intenta consultar el expediente de otro | RS-02 |
| Intento de modificación de un registro de auditoría | RF-16, RN-35 |
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
| Reporte de cobertura | `docs/09_pruebas/sprint-N/` |
| Registro de defectos encontrados y su resolución | Issues del repositorio |
| Acta de la demostración de cierre de sprint | `docs/11_actas/` |
| Reporte de rendimiento | `docs/09_pruebas/rendimiento/` |
| Reporte de seguridad | `docs/09_pruebas/seguridad/` |

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

Sprint 1 en ejecución. Las primeras pruebas corresponden a control de acceso por
rol (HU-45) y se ejecutan al cierre del sprint, el 3 de octubre. Este documento se
actualiza con la evidencia de cada iteración.

---

## Control de cambios

| Versión | Fecha | Descripción | Autor |
|---|---|---|---|
| 1.0 | 24-09-2026 | Emisión inicial | Equipo de proyecto |
