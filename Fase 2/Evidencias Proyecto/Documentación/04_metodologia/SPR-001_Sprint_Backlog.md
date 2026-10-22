# Sprint Backlog

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | SPR-001 · Sprint Backlog |
| **Sprints** | 1 · Núcleo de datos y accesos — cerrado<br>2 · Domicilio tributario de punta a punta — cerrado<br>3 · Segundo documento y motor configurable — en ejecución |
| **Período** | 22 de septiembre – 31 de octubre de 2026 |
| **Versión** | 2.0 |
| **Fecha** | 22 de octubre de 2026 |
| **Preparado por** | Equipo de proyecto |

---

## Cómo leer este documento

El Sprint 1 se planificó por escrito antes de empezar (versión 1.0 de este
documento, 24-09-2026). Los sprints 2 y 3 **no tuvieron un Sprint Backlog escrito
al inicio**: el trabajo se organizó por issues de GitHub después de la reunión de
validación del 8 de octubre (`ACTA-002`). Sus tablas se reconstruyen aquí a partir
de los issues, los pull requests fusionados y los commits de los repositorios de
backend y frontend, con la fecha de fusión de cada PR. Lo que no se puede
comprobar en los repositorios se marca `[CONFIRMAR]`.

Estados usados: **Hecho** (fusionado en la rama principal), **Parcial**
(fusionado, con parte de los criterios pendientes), **Traspasado** (no se terminó
y pasa a un sprint siguiente), **En curso** y **Pendiente**.

## Calendario de sprints

| Sprint | Período | Foco planificado (v1.0) | Foco real |
|---|---|---|---|
| 1 | 22 sep – 3 oct | Entorno, núcleo de datos y accesos | Entorno, sesión del personal y roles |
| 2 | 6 – 17 oct | Servicios contratados, vigencias y migración | Clientes, pagos en línea, contratos con folio y domicilio tributario de punta a punta |
| 3 | 20 – 31 oct | Motor configurable y generación documental | Dashboard con datos reales, segundo documento (`PLT-02`) y lo que se defina en la sección 3.4 |
| 4 | 3 – 14 nov | Portal de contratación e integraciones | — |
| 5 | 17 – 28 nov | Pruebas, despliegue y estabilización | — |

**Por qué cambió el foco del Sprint 2.** En la reunión del 8 de octubre Easy
Office confirmó el domicilio tributario como servicio piloto, entregó las
plantillas reales (`PLT-01` a `PLT-05`) y la estructura de su planilla, pero no la
planilla completa anonimizada. Con eso el equipo priorizó un flujo completo y
demostrable del piloto, incluido el pago, por sobre la migración, que quedó
bloqueada por la planilla (`RF-43`).

---

## 1. Sprint 1 · Núcleo de datos y accesos

**22 de septiembre – 3 de octubre de 2026 · Cerrado**

### 1.1 Objetivo

> Dejar el entorno de desarrollo operativo y el núcleo de datos y accesos
> funcionando: un usuario con rol puede autenticarse, registrar un cliente con su
> empresa y su representante legal, y cada acción queda registrada en auditoría.

### 1.2 Capacidad

| Integrante | Disponibilidad estimada | Puntos comprometidos |
|---|---|---|
| Fernando Cartagena | ~10 h/semana | 8 |
| Nicolás Zapata | ~10 h/semana | 14 |
| Marcos Álvarez | ~10 h/semana | 9 |
| **Total** | **~60 h** | **31** |

La versión 1.0 indicaba 8, 13 y 8 puntos (29 en total), pero el detalle de tareas
e historias sumaba 31. Esta tabla queda con la suma del detalle.

### 1.3 Trabajo comprometido y resultado

**Preparación del entorno**

| ID | Tarea | Resp. | Pts | Resultado | Evidencia |
|---|---|---|---|---|---|
| T-01 | Crear los repositorios de backend y frontend, con su contexto y README | Fernando | 1 | Hecho, por Nicolás | Primeros commits de ambos repositorios, 20-09 |
| T-02 | Generar el proyecto Django con su estructura base y podar dependencias | Nicolás | 2 | Hecho | Commits del 20-09 (cookiecutter-django, poda de dependencias, estructura modular) |
| T-03 | Levantar el entorno con Docker Compose: PostgreSQL, Redis, aplicación y worker | Nicolás | 3 | Hecho | Commit del 20-09 que verifica el entorno local; CI con linter y pruebas |
| T-04 | Configurar GitHub Projects con las historias del sprint | Marcos | 1 | `[CONFIRMAR]` | No verificable desde los repositorios |
| T-05 | Plantilla de Pull Request con la Definition of Done | Marcos | 1 | Traspasado | No existe `.github/pull_request_template.md` en ningún repositorio; la DoD se revisa a mano en cada PR |

**Historias de usuario**

| ID | Historia (resumen) | Resp. | Pts | Resultado | Evidencia |
|---|---|---|---|---|---|
| HU-01 | Inicio de sesión del personal | Nicolás | 2 | Hecho | Commits del 30-09; backend PR #15 fusionado el 05-10; pantalla de login en el frontend (30-09) |
| HU-02 | Crear y desactivar usuarios internos | Nicolás | 2 | Parcial | Se hace desde el panel de administración de Django; no hay pantalla propia `[CONFIRMAR si basta para Easy Office]` |
| HU-04 | Asignar un rol a cada usuario | Nicolás | 3 | Hecho | Roles Administrador y Ejecutivo sembrados como grupos (30-09, PR #15) |
| HU-05 | Ejecutivos sin permiso para eliminar clientes | Nicolás | 2 | Traspasado → Sprint 2 | La API de clientes no expone eliminación (PR #18, 10-10) y el panel lo bloquea (14-10) |
| HU-06 | Registrar un cliente con sus datos de contacto | Fernando | 3 | Traspasado → Sprint 2 | PR #18 del backend y #5 del frontend, 10-10 |
| HU-51 | Identificador único del cliente | Fernando | 1 | Traspasado → Sprint 2 | Código del cliente en el PR #18 |
| HU-08 | Registrar una empresa con RUT, razón social y representante legal | Fernando | 3 | Traspasado → Sprint 2 | Modelo inicial el 20-09; datos civiles y personería en el PR #40, 14-10. Esperaba el listado de campos (D-01), recibido el 08-10 |
| HU-09 | Vigencia de la representación legal | Marcos | 2 | Parcial | El modelo registra `vigente_desde` y `vigente_hasta` desde el 20-09 y el servicio de clientes usa el representante vigente; sin pantalla propia |
| HU-30 | Registro de auditoría | Marcos | 3 | Traspasado → Sprint 3 | No implementado. El código marca los puntos donde se registrará (`TODO(RF-15)`) |
| HU-45 | Pruebas de control de acceso por rol | Marcos | 2 | Parcial | Pruebas de login, cookie de sesión y roles sembrados (30-09); falta la suite por rol y por recurso |

Fuera del compromiso, el frontend publicó el 20-09 el portal del cliente sobre el
prototipo y el 05-10 el dashboard con cifras de ejemplo (PR #3, HU-41).

### 1.4 Criterios de aceptación al 3 de octubre

| Criterio | Cumplido |
|---|---|
| `docker compose up` levanta el entorno completo desde cero | Sí |
| Las migraciones corren sin errores sobre una base vacía | Sí |
| Un administrador puede crear usuarios y asignarles rol | Sí, desde el panel de administración |
| Un ejecutivo no puede eliminar un cliente, con prueba automatizada | No: se cumplió en el Sprint 2 |
| Se registra un cliente persona natural y uno persona jurídica con su representante | No: el modelo existía, la API llegó en el Sprint 2 |
| Toda creación y modificación queda en auditoría | No: traspasado |
| El registro de auditoría no admite modificación ni eliminación | No: traspasado |
| Las pruebas del sprint pasan | Sí |
| Ningún dato personal real ni credencial fue versionado | Sí |

**Resultado.** 11 de 31 puntos hechos al cierre (T-01, T-02, T-03, HU-04 y HU-01,
este último fusionado dos días después) y tres historias parciales (HU-02, HU-09
y HU-45). La estimación resultó optimista para un primer sprint, como advertía la
versión 1.0: el entorno tomó menos de lo previsto, pero las historias de clientes
dependían del listado de campos que Easy Office entregó el 8 de octubre.

### 1.5 Dependencias del Sprint 1

| # | Dependencia | Afecta a | Estado |
|---|---|---|---|
| D-01 | Listado definitivo de campos del cliente | HU-06, HU-08 | Recibida el 08-10-2026: muestra de 34 campos (`ACTA-002`, `13_plantillas/README.md`) |
| D-02 | Roles adicionales a Administrador y Ejecutivo | HU-04 | Cerrada el 08-10-2026: solo existen esos dos (`ACTA-002`) |
| D-03 | Estructura de la planilla a migrar | Sprint 2 | Estructura recibida el 08-10-2026; la planilla completa anonimizada sigue pendiente |

---

## 2. Sprint 2 · Domicilio tributario de punta a punta

**6 – 17 de octubre de 2026 · Cerrado · Reconstruido**

### 2.1 Objetivo (reconstruido)

> Un cliente puede crear su cuenta, completar el formulario del domicilio
> tributario, revisar el contrato generado desde la plantilla real `PLT-01`,
> pagarlo en línea y descargarlo firmado, y el ejecutivo lo gestiona desde el
> backoffice.

### 2.2 Trabajo realizado

| Historias | Qué se entregó | Resp. | Backend | Frontend | Fusión |
|---|---|---|---|---|---|
| HU-06, HU-07, HU-51, HU-52, HU-05 · parcial HU-47, HU-49, HU-50 | API y pantallas de clientes: registro, código, RUT validado y no repetido, búsqueda, filtros por tipo y estado, ficha sin eliminación; exportación del dashboard a Excel | Nicolás | PR #18 | PR #5 | 10-10 |
| HU-03 | Cuenta del cliente en el portal: registro, vínculo por RUT e inicio de sesión | Nicolás | PR #24 | PR #8 | 10-10 |
| HU-38, HU-39, HU-40, HU-57, HU-63, HU-64 | Pago en línea con Klap: interfaz `PaymentService`, órdenes y notificaciones idempotentes, compras del cliente y ventas en el CRM | Nicolás | PR #26 | PR #10 | 10-10 |
| HU-39, HU-62 | Flow como segundo medio de pago, a elección del cliente | Nicolás | PR #28 | PR #12 | 10-10 |
| — | Túnel autorrenovable para recibir webhooks en local | Nicolás | PR #30 | — | 12-10 |
| HU-08 | Datos civiles de la persona y personería del representante | Fernando | PR #40 | — | 14-10 |
| HU-66 | Inmuebles por tipo (oficina o bodega) y su autorizante | Fernando | PR #42 | — | 14-10 |
| HU-65 | Datos de Easy Office, marcas y cuentas de pago | Fernando | PR #43 | — | 14-10 |
| HU-23 | Catálogo de servicios desde el formulario de Easy Office; servicios visibles y contratables; landing desde la API | Fernando | PR #44, #48 | PR #16 | 14-10 y 15-10 |
| HU-60 | Contratos con folio por sucursal y servicios contratados | Fernando | PR #45 | — | 14-10 |
| HU-24, HU-19, HU-20, HU-21, HU-26, HU-27, HU-28, HU-29, HU-56, HU-61 | Domicilio tributario de punta a punta con `PLT-01`: formulario, contrato en borrador, documento Word y PDF, revisión, pago, firma asistida, descarga y motivo de pendiente | Nicolás | PR #51 | PR #18 | 17-10 |
| HU-01, HU-03 | Sesión propia del panel de administración y cambio entre sesión de personal y de cliente | Nicolás | PR #52 | PR #19 | 17-10 |
| — | Herramienta del equipo para documentar cada cambio | Nicolás | PR #32 | PR #14 | 14-10 |

Responsable según quien fusionó el PR en GitHub. Varias ramas tuvieron commits de
más de un integrante; el detalle está en el historial de cada repositorio.

**Corrección a la versión 1.1.** La nota "trabajo adelantado" del Sprint 1
listaba HU-03, HU-38, HU-39, HU-40, HU-57, HU-62, HU-63 y HU-64. Sus commits son
del 7 y el 10 de octubre y sus PR se fusionaron el 10 de octubre: pertenecen al
Sprint 2.

### 2.3 Lo que no entró

| Elemento | Motivo | Destino |
|---|---|---|
| HU-48, HU-53, HU-42: servicios contratados, detección de vencimientos y alertas | El contrato con sus líneas (HU-60) cubre la base; el seguimiento y las alertas quedan para después del piloto | Sprint 4 `[CONFIRMAR]` |
| HU-32, HU-33: migración desde Excel | Falta la planilla completa anonimizada (D-03) | Bloqueado |
| HU-30: auditoría | Traspasada del Sprint 1, sin avance | Sprint 3 |
| HU-35, HU-37: firma con proveedor | Faltan las credenciales del proveedor de Easy Office (TA-07); se implementó la firma asistida | Bloqueado |

### 2.4 Hitos del sprint

| Fecha | Instancia |
|---|---|
| 08-10 | Reunión de validación con Easy Office (`ACTA-002`) |
| 12-10 | Avance técnico registrado en `ACTA-002` |
| 17-10 | Domicilio tributario fusionado en ambos repositorios |
| 17-10 | Sprint Review y retrospectiva `[CONFIRMAR fecha y registro en RET-001]` |

---

## 3. Sprint 3 · Segundo documento y motor configurable

**20 – 31 de octubre de 2026 · En ejecución · Reconstruido al 22-10**

### 3.1 Objetivo (propuesto)

> Demostrar que el sistema incorpora un segundo servicio sin código nuevo por
> documento, mostrar indicadores reales de la operación y cerrar la deuda de
> auditoría y de pruebas de seguridad antes de la presentación.

`[CONFIRMAR el objetivo en la planificación del equipo]`

### 3.2 Hecho al 22 de octubre

| Historias | Qué se entregó | Resp. | Backend | Frontend | Fusión |
|---|---|---|---|---|---|
| HU-41, HU-61 | Dashboard con datos reales, acceso del Ejecutivo y pendientes por motivo | Nicolás | PR #54 | PR #21 | 20-10 |
| HU-15 | Bodega virtual con `PLT-02`: segundo tipo de trámite, iniciado por un ejecutivo o desde el portal, con tres planes de precio provisorio | Fernando | PR #60 | PR #23 | 21-10 |

637 pruebas del backend y 150 del frontend pasan en la rama principal (`PLA-001`).

### 3.3 Por hacer en el sprint

Propuesto según los pendientes de `TRZ-001`, el checklist de cumplimiento y la
presentación. `[CONFIRMAR responsables y alcance en la planificación]`

| ID | Elemento | Resp. | Estado |
|---|---|---|---|
| HU-30, HU-31 | Registro de auditoría de solo escritura y su consulta (RF-15 a RF-17) | `[CONFIRMAR]` | Pendiente |
| HU-14, HU-16 | Pantalla para configurar tipos de trámite: campos, plantilla, pago y firma | `[CONFIRMAR]` | Pendiente |
| HU-59 | Identidad visual de Easy Office en el portal y el backoffice (RNF-12) | Marcos | Pendiente |
| HU-45 | Pruebas de control de acceso por rol y pruebas de seguridad | Marcos | Pendiente |
| HU-46 | Pruebas de rendimiento (RNF-10) | Marcos | Pendiente |
| T-05 | Plantilla de Pull Request con la Definition of Done | Marcos | Pendiente, traspasada del Sprint 1 |
| — | Documentación: Sprint Backlog, diagramas UML, retrospectivas | Fernando | En curso |
| — | Presentación final y guion | Equipo | En curso |

### 3.4 Bloqueos vigentes

| # | Bloqueo | Afecta a | Responsable |
|---|---|---|---|
| B-01 | Planilla completa anonimizada | HU-32, HU-33 | Easy Office |
| B-02 | Credenciales del proveedor de firma (TA-07) | HU-35, HU-37 | Easy Office |
| B-03 | Precios reales de los planes de la bodega (TA-15, TA-26) | HU-15, precios provisorios | Easy Office |
| B-04 | Medios de pago que se ofrecerán en producción | IN-02 | Easy Office |

---

## 4. Riesgos activos

| ID | Riesgo | Acción |
|---|---|---|
| R-06 | Alcance excesivo | El motor configurable avanza con casos reales (`PLT-01`, `PLT-02`) antes de una pantalla genérica |
| R-08 | Carga del equipo | La carga quedó concentrada en dos integrantes; el Sprint 3 asigna a Marcos pruebas e identidad visual |
| R-05 | Datos reales en repositorio público | Revisión en cada PR; las plantillas solo se versionan anonimizadas; falta automatizarla con la plantilla de PR (T-05) |

---

## 5. Seguimiento

| Instancia | Cómo se hace |
|---|---|
| Sprint Planning | Al inicio de cada sprint; la del Sprint 1 quedó escrita, las de los sprints 2 y 3 no |
| Seguimiento | Asíncrono, por issues y PR |
| Sprint Review | Demostración al cierre; con Easy Office el 8 de octubre |
| Retrospectiva | Al cierre de cada sprint, registrada en `RET-001` |

---

## Control de cambios

| Versión | Fecha | Descripción | Autor |
|---|---|---|---|
| 1.0 | 24-09-2026 | Emisión inicial: Sprint 1 | Equipo de proyecto |
| 1.1 | 12-10-2026 | Nota de avance: trabajo adelantado fuera del compromiso del sprint | Equipo de proyecto |
| 2.0 | 22-10-2026 | Cierre del Sprint 1 con su resultado real; sprints 2 y 3 reconstruidos desde issues, PR y commits; corrección de la nota de la versión 1.1; suma de capacidad corregida | Equipo de proyecto |
