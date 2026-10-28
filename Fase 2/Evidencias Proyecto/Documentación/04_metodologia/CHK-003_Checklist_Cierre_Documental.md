# Checklist de cierre documental

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | CHK-003 · Checklist de cierre documental |
| **Versión** | 1.0 |
| **Fecha** | 27 de octubre de 2026 |
| **Estado** | Vigente. Estado de cada tarea contrastado con la rama principal de los tres repositorios al 27-10-2026 |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## Cómo se usa

Parte de una revisión externa de los tres repositorios (60 tareas, `CK-01` a
`CK-60`). Cada tarea se contrastó con el estado real; donde la revisión estaba
desactualizada, se indica en la columna *Estado*.

- **Prioridad:** P0 corregir antes de usar la documentación como estado oficial;
  P1 completar para la próxima revisión del incremento; P2 antes de la entrega
  final.
- **Estado:** **Hecho** (cumple el criterio de cierre), **Parcial** (avanzado,
  falta lo indicado) o **Pendiente**.
- **Responsable sugerido:** F Fernando, N Nicolás, M Marcos, E equipo, I cada
  integrante. Es una sugerencia; el responsable real se registra al cerrar.
- **Depende de:** *Equipo* (se avanza sin nadie más), *Ejecución* (hay que usar o
  probar el sistema) o *Externo* (solo esa parte depende de Easy Office).

Una tarea se marca Hecho solo al cumplir su criterio, no al empezar a redactarla.

---

## Resumen al 27-10-2026

| Estado | Tareas |
|---|---|
| Hecho | 6 |
| Parcial | 21 |
| Pendiente | 33 |

**Correcciones a la revisión externa:**

1. **La firma no es solo asistida.** La firma avanzada contra el PSC de pruebas
   está fusionada (backend PR #68, frontend PR #27). La revisión se hizo sobre
   commits del 21 y 22 de octubre, anteriores a esos PR.
2. **Cifras de pruebas.** La rama principal del backend tiene 685 pruebas (más 62
   del PSC); las 696 son de la rama de auditoría (PR #62, abierto). La cifra
   «637 en main» quedó atrás.
3. **El CSV sintético no se corrige.** `PLT-000` (OB-03) registra que en la
   planilla el vencimiento se digita a mano y no sigue una regla; la muestra lo
   replica a propósito. Sus tres filas son el caso de prueba `MIG-T07`.
4. **Límite de archivos.** Hay dos límites: 5 MB para adjuntos
   (`core/validators.py`) y 10 MB para el PDF firmado.
5. **Identificadores de decisiones.** Las decisiones del equipo se numeran
   `DE-nn`: `D-nn` ya es una dependencia de `SPR-001` en `CAT-001`.

---

## 1. Gobernanza y alcance

| ID | Pri. | Tarea | Estado | Qué hay y qué falta | Resp. | Depende de |
|---|---|---|---|---|---|---|
| CK-01 | P0 | Registrar la indicación docente | **Hecho** | `DEC-001` §1, atribuida al relato de Fernando, sin fecha ni acta inventadas | F | Equipo |
| CK-02 | P0 | Resolver las suposiciones activas | Pendiente | `DEC-001` propone DE-01 a DE-14. Falta la sesión que las adopte y complete el §5 con fecha, autor y revisor | E | Equipo |
| CK-03 | P0 | Un alcance vigente único | Parcial | `FUN-001` v1.2 y los límites funcionales del índice ya separan lo previsto de lo implementado. Falta el apartado de estado en `ALC-001` y `PV-001` | F | Equipo |
| CK-04 | P0 | Precisar la promesa del motor configurable | Parcial | Índice (límite 2) y `DEC-001` DE-04. Falta llevarlo a `ALC-001` y `PV-001` | F/N | Equipo |
| CK-05 | P0 | Separar las modalidades de firma | **Hecho** | Asistida, avanzada contra el PSC de pruebas y con validez legal: `FUN-001` §4, `DEC-001` DE-05, `ARQ-001` AD-16, `RN-001` v2.9. La guía 2.4 se corrige en CK-09. La marca visible de prueba en los PDF del PSC está en el prompt de pendientes de los repositorios | F/E | Equipo |
| CK-06 | P0 | Actualizar `FUN-001` | **Hecho** | `FUN-001` v1.1 (Marcos, 27-10), con ajustes menores en v1.2 | F | Equipo |
| CK-07 | P1 | Revisar `MRQ-001`, `RN-001` y `CAT-001` | Parcial | `RN-001` v2.9 y `CAT-001` v1.6 (prefijos y documentos nuevos). `MRQ-001` se actualiza al adoptar `DEC-001` | F/E | Equipo |
| CK-08 | P1 | Depurar `ACTA-002` y `CHK-001` | Pendiente | Seguimiento fechado y pendientes ya resueltos. La modalidad y los asistentes de la reunión los aporta el equipo | F | Equipo/Externo |

## 2. Entrega académica de la Fase 2

| ID | Pri. | Tarea | Estado | Qué hay y qué falta | Resp. | Depende de |
|---|---|---|---|---|---|---|
| CK-09 | P0 | Precisar el cumplimiento de OE-1 en la guía 2.4 | Pendiente | Nueva versión de la guía, sin reescribir la entrega del 20-10: temas tratados, acuerdos, puntos abiertos y confirmación escrita pendiente | F/E | Equipo |
| CK-10 | P0 | Distinguir datos reales de datos de prueba | Pendiente | En la guía: «panel conectado a la base de datos; validado con datos sintéticos» | F/E | Equipo |
| CK-11 | P0 | Fijar el corte de las cifras de pruebas | Parcial | Cortes registrados en `EVI-001` §1 y `PLA-001` v1.11. Falta aplicarlos en la nueva versión de la guía | F/N | Equipo |
| CK-12 | P0 | Seis bloques personales en inglés | Pendiente | Plantillas en `GES-001` §6; cada autor escribe y revisa los suyos | I | Equipo |
| CK-13 | P0 | Nueve capturas del anexo A | Parcial | A.1 y A.9 tomadas el 27-10. Las otras siete necesitan la aplicación corriendo y una sesión iniciada por el usuario | M/E | Ejecución |
| CK-14 | P1 | Resumen y abstract equivalentes | Pendiente | Con la nueva versión de la guía | F/E | Equipo |
| CK-15 | P1 | Revisar los tres diarios | Pendiente | Los tres están en el repositorio; cada autor confirma el suyo | I | Equipo |
| CK-16 | P1 | Formato y referencias de la guía | Pendiente | Índice, pies, referencias y exportación a PDF revisada | F/E | Equipo |

## 3. Planificación y método

| ID | Pri. | Tarea | Estado | Qué hay y qué falta | Resp. | Depende de |
|---|---|---|---|---|---|---|
| CK-17 | P0 | Nuevo corte de `GNT-001` | Pendiente | Sigue al 09-10. Regenerar el PDF desde el mismo XLSX, sin borrar la línea base | F/E | Equipo |
| CK-18 | P0 | Conciliar los hitos H3 y H4 | Pendiente | Logrado con evidencia, parcial o reprogramado, con motivo | E | Equipo |
| CK-19 | P1 | Revisar los hitos restantes | Pendiente | H5 migración (31-10) es esta semana y no hay importador | E | Equipo |
| CK-20 | P0 | Separar merge de Done | **Hecho** | `SPR-001` v2.2: fusionado, cumple la Definition of Done y aceptado son niveles distintos | F/E | Equipo |
| CK-21 | P1 | Revisar el cierre del Sprint 1 | **Hecho** | `SPR-001` v2.2: 6 puntos al corte del 03-10 y 11 al 05-10 | F/E | Equipo |
| CK-22 | P1 | Planificar el siguiente ciclo antes de ejecutarlo | Pendiente | Propuesta en `GES-001` §2 | E | Equipo |
| CK-23 | P1 | Retrospectiva real | Parcial | `RET-001` v1.2 ya dice «sesión no registrada» en lugar de «por completar». Falta hacer la sesión (`GES-001` §4) | E | Equipo |
| CK-24 | P1 | Regularizar aportes y responsables | Parcial | `GES-001` §5, tabla desde el historial de Git; `SPR-001` v2.2 corrigió HU-24 y HU-41 (Fernando). Falta conciliar responsables planificados y revisores | F/E | Equipo |

## 4. Trazabilidad y evidencia técnica

| ID | Pri. | Tarea | Estado | Qué hay y qué falta | Resp. | Depende de |
|---|---|---|---|---|---|---|
| CK-25 | P0 | Actualizar `TRZ-001` | Parcial | v2.14: estados revisados contra el código, cobertura al 27-10 y definición de estados. Falta el enlace a casos y resultados | F/N/M | Equipo |
| CK-26 | P1 | Índice real de evidencias | Parcial | Formato en `12_evidencias/README.md` y carpetas en `EVI-001` §2. No hay evidencias cargadas | E | Equipo |
| CK-27 | P1 | Ejecución reproducible del candidato | Pendiente | Pruebas, tipos, lint y compilación de ambos repositorios, con registros | N/M | Ejecución |
| CK-28 | P1 | Corregir la afirmación sobre CI | Parcial | El frontend sigue sin CI. Está en el prompt de pendientes de los repositorios (parte 2) | N/M | Equipo |
| CK-29 | P1 | Domicilio de punta a punta | Pendiente | `EVI-T01`. La prueba de punta a punta de firma avanzada (`just e2e-firma`) pasó en local, pero cubre bodega | F/N/M | Ejecución |
| CK-30 | P1 | Bodega en el navegador | Pendiente | `EVI-T08` y `EVI-T09` | N/M | Ejecución |
| CK-31 | P1 | Auditoría en la rama principal | Pendiente | Backend PR #62 abierto; pantallas del frontend pendientes | F/N/M | Ejecución |
| CK-32 | P1 | Alta de un tipo por configuración | Pendiente | `EVI-T12` | N/E | Ejecución |
| CK-33 | P1 | Informe de seguridad | Pendiente | `EVI-001` §5 y el prompt de pruebas de seguridad y rendimiento | N/M | Ejecución |
| CK-34 | P1 | Informe de rendimiento | Pendiente | Metas en `DEC-001` DE-13 y `EVI-001` §4 | N/M | Ejecución |
| CK-35 | P1 | Aceptación técnica interna | Pendiente | Acta en `EVI-001` §8 | E | Ejecución |
| CK-36 | P2 | Medir el beneficio | Pendiente | `EVI-001` §9 | F/E | Ejecución |

## 5. Datos, documentos y migración

| ID | Pri. | Tarea | Estado | Qué hay y qué falta | Resp. | Depende de |
|---|---|---|---|---|---|---|
| CK-37 | P1 | Mapeo físico de la migración | Parcial | `MIG-001` v0.1 mapea las 34 columnas a los modelos reales. Falta que Nicolás lo revise y defina `RepresentanteLegal.vigente_desde` | N | Equipo |
| CK-38 | P1 | Vencimientos del CSV sintético | **Hecho** | No se corrigen: replican la planilla (`PLT-000` OB-03) y quedan como caso `MIG-T07` (`MIG-001` §2) | N | Equipo |
| CK-39 | P1 | IVA y precios de ejemplo | Pendiente | `DEC-001` DE-07 propone el criterio. `PLT-000` dice «sin IVA», RN-44 lo contrario y `PLT-01` dice «MÁS I.V.A.» | N/F | Equipo |
| CK-40 | P1 | Desarrollar el importador | Pendiente | La app `migracion` está vacía | N | Ejecución |
| CK-41 | P1 | Ensayo sintético de migración | Pendiente | `MIG-T01` a `MIG-T09` | N | Ejecución |
| CK-42 | P1 | Folios heredados y renovaciones | Parcial | `DEC-001` DE-10 y `MIG-001` §2: hace falta una vía para importar un folio histórico y ajustar `CorrelativoFolio` | F/N | Equipo |
| CK-43 | P1 | Fidelidad de `PLT-01` y `PLT-02` | Pendiente | `EVI-T16` | N/M | Ejecución |
| CK-44 | P1 | Documentos adicionales del MVP | Parcial | `PLT-000` v1.5 ya separa `PLT-01`/`PLT-02` implementadas de `PLT-03` a `PLT-05`. Falta decidir qué demuestra la configurabilidad y registrar el ajuste de alcance | F/E | Equipo |
| CK-45 | P1 | Sincronizar `MOD-001` y `UML-002` | Parcial | v1.11 y v1.2 con la firma avanzada. Falta revisar contratos, ventas e inmuebles y generar el esquema como evidencia | N | Equipo |

## 6. Arquitectura, manuales y operación

| ID | Pri. | Tarea | Estado | Qué hay y qué falta | Resp. | Depende de |
|---|---|---|---|---|---|---|
| CK-46 | P1 | Actualizar `ARQ-001`, `DIS-001`, `REC-001` y `UML-001` | Parcial | `ARQ-001` v1.7 y `DIS-001` v1.6 con el PSC, la secuencia de firma y AD-16. Falta `REC-001` y `UML-001` | F/N/M | Equipo |
| CK-47 | P1 | Validar `MTE-001` en una instalación limpia | Parcial | `MTE-001` v0.1, comandos contrastados con los repositorios. Falta la instalación por otro integrante | N/M | Ejecución |
| CK-48 | P1 | `MUS-001` con capturas | Parcial | `MUS-001` v0.1. Falta el recorrido por rol y las capturas | M/E | Ejecución |
| CK-49 | P1 | Arquitectura de despliegue y costos | Parcial | `DEC-001` DE-12 y `MOP-001` §1. Falta elegir proveedor y costo (TA-09) | F/N | Equipo |
| CK-50 | P1 | Validar `MOP-001` y la restauración | Pendiente | `MOP-001` §4; ensayo no ejecutado | N | Ejecución |
| CK-51 | P1 | Completar `RSK-001` | Parcial | v1.6: R-03 ya no afirma que pasar al proveedor real sea solo configuración. Faltan datos, participación del cliente y rendimiento | F/N | Equipo |
| CK-52 | P1 | Unificar los README | Parcial | Índice al 27-10 (Marcos) y README raíz en este paquete. Frontend: prompt de pendientes, parte 3. Backend: dice siete servicios y 684 pruebas; son once y 685 | E | Equipo |
| CK-53 | P1 | Plantilla de PR | Parcial | Repositorio de documentación: en este paquete. Backend y frontend: prompt de pendientes, parte 1 | E | Equipo |
| CK-54 | P2 | Entrega operativa | Pendiente | `MOP-001` §6 | F/N/M | Equipo/Ejecución |

## 7. Fase 3, calidad y cierre

| ID | Pri. | Tarea | Estado | Qué hay y qué falta | Resp. | Depende de |
|---|---|---|---|---|---|---|
| CK-55 | P2 | Estructura de la Fase 3 | Pendiente | Índice provisional en `GES-001` §8 | E | Equipo |
| CK-56 | P2 | Cierre por objetivo | Pendiente | Matriz en `GES-001` §8 | F/E | Equipo |
| CK-57 | P2 | Ensayo de la defensa | Pendiente | — | I | Equipo |
| CK-58 | P2 | Nombres y duplicados | Pendiente | `Fase 1/Evidencias Grupales` tiene dos copias idénticas de la guía 1.5 que solo difieren en cómo está codificada la «ñ» del nombre. Borrar una sin tocar la otra | E | Equipo |
| CK-59 | P1 | Revisión cruzada de este paquete | Pendiente | Un integrante que no lo preparó lo revisa antes de fusionar | E | Equipo |
| CK-60 | P2 | Congelar una versión verificable | Pendiente | Commits de los tres repositorios y manifest de evidencias (`EVI-001` §2) | E | Equipo/Ejecución |

---

## Orden recomendado para esta semana

1. **Adoptar decisiones y revisar este paquete:** CK-02, CK-59.
2. **Fusionar la auditoría y tomar evidencia:** CK-31, CK-13, CK-27, CK-29, CK-30.
3. **Migración (hito H5, 31-10):** CK-37, CK-40, CK-41.
4. **Nueva versión de la guía 2.4:** CK-09 a CK-12, CK-14.
5. **Planificación:** CK-17 a CK-19, CK-22, CK-23.

---

## Registro de cierre

| ID | Responsable | Fecha comprometida | Estado | Evidencia o PR | Revisó | Fecha de cierre |
|---|---|---|---|---|---|---|
| CK-__ | Por asignar | Sin compromiso | Pendiente | — | — | — |

---

## Historial de versiones

| Versión | Fecha | Cambios |
|---|---|---|
| 1.0 | 27-10-2026 | Versión inicial: 60 tareas de la revisión externa con su estado real al 27-10-2026 y las correcciones a esa revisión |
