# Gestión del trabajo, aportes individuales y preparación del cierre

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | GES-001 · Gestión, aportes y cierre |
| **Versión** | 0.1 |
| **Fecha** | 27 de octubre de 2026 |
| **Estado** | **Formatos y textos propuestos.** No registra reuniones ni evaluaciones que no ocurrieron. La tabla de aportes (§5) está tomada del historial de Git al 27-10-2026 |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## 1. Estado metodológico, para el informe

> El proyecto usa un enfoque ágil adaptado a tres integrantes, con Product
> Backlog, Sprint Backlog, Definition of Done, registro de retrospectivas y
> planificación temporal. `SPR-001` reconoce que los sprints 2 y 3 se
> reconstruyeron después de iniciados; esa reconstrucción se usa como evidencia de
> seguimiento, no como planificación previa. Para el siguiente ciclo, el objetivo,
> las historias, la capacidad y los responsables se fijan antes de empezar, y el
> cierre incluye demostración, revisión de evidencia y una retrospectiva real.

> Ante la participación limitada de Easy Office, el equipo adopta decisiones
> técnicas fundamentadas (`DEC-001`) y hace aceptación técnica interna
> (`EVI-001` §8), según la indicación docente informada por Fernando Cartagena.
> Cada decisión se distingue de una confirmación del cliente.

---

## 2. Planificación del siguiente ciclo

**Objetivo propuesto:** un incremento demostrable y una documentación coherente
del piloto, con evidencia ligada a versiones y una lista explícita de lo que
falta. Fechas y puntos no se asignan después del hecho.

| Trabajo | Resultado verificable | Dependencia | Responsable |
|---|---|---|---|
| Adoptar las decisiones de `DEC-001` | Registro de adopción completo | Sesión del equipo | Por acordar |
| Fusionar la auditoría (backend #62 y su frontend) | Commit en la rama principal y `EVI-T11` | Revisión del PR | Por acordar |
| Candidato del backend y del frontend | Commits y resultados de pruebas y compilación | Integración | Por acordar |
| Recorrido del piloto y capturas | `EVI-T01`, `EVI-T10` y anexos A.2 a A.8 | Ambiente funcionando | Por acordar |
| Importador sintético | `MIG-T01` a `MIG-T09` | Ninguna externa para el ensayo | Por acordar |
| Textos personales de la guía | Seis bloques auténticos | Cada integrante | Cada autor |

---

## 3. Registro de Sprint Review

| Campo | Contenido |
|---|---|
| Sprint, fecha y participantes reales | Por completar |
| Objetivo comprometido y cambios | Referencia a la planificación previa, o indicar que se reconstruyó |
| Candidato | Commits de backend, frontend y documentación |
| Demostraciones | Casos y evidencias del incremento |
| Aceptado técnicamente | Solo casos revisados con criterio y resultado |
| Pendientes de la Definition of Done | Pruebas, revisión, documentación, auditoría u otros |
| Retroalimentación | Indicar si es interna, docente o de Easy Office |
| Acciones | Responsable que la acepta y fecha acordada |

---

## 4. Retrospectiva

Sesión breve. Se registra lo que el equipo diga:

1. **Qué funcionó:** el hecho y su evidencia.
2. **Qué dificultó el trabajo:** la situación concreta y su efecto, sin culpas
   generales.
3. **Qué cambiar:** hasta tres acciones comprobables en el siguiente ciclo.

| Problema observado | Acción acordada | Responsable | Fecha acordada | Cómo se verá la mejora | Revisión |
|---|---|---|---|---|---|
| Por completar en la sesión | — | — | — | — | — |

Si un sprint anterior cerró sin retrospectiva, se registra la ausencia (`RET-001`)
y se hace una revisión actual con su fecha real.

---

## 5. Aportes verificables por integrante

Autoría de los commits de cada PR fusionado en la rama principal al 27-10-2026,
no de quién hizo el merge. Las cuentas `fecartagena` y `Fernando Cartagena`
corresponden a Fernando; `Nicolás Zapata`, `Nicozapata1811` y `ni.zapatat`, a
Nicolás.

| Integrante | Backend (PR) | Frontend (PR) | Documentación |
|---|---|---|---|
| Fernando Cartagena | #40 datos civiles (HU-08), #42 inmueble y autorizante (HU-66), #43 configuración de la empresa (HU-65), #44 y #48 catálogo (HU-23), #45 contrato y folio (HU-60), #51 domicilio con `PLT-01` (HU-24), #54 panel real (HU-41), #60 bodega con `PLT-02` (HU-15) | #16 catálogo (HU-23), #18 domicilio (HU-24), #21 panel real (HU-41), #23 bodega (HU-15), #25 identidad visual (HU-59) | 27 commits |
| Nicolás Zapata | #15 ingreso (HU-01), #18 gestión de clientes (HU-06), #24 cuentas de cliente (HU-03), #26 pago Klap, #28 pago Flow y #30 túnel (HU-39), #32 skill de exportación, #52 sesión del administrador, #68 firma avanzada y PSC de pruebas (IN-01); #62 auditoría (HU-30), abierto | #3 panel (HU-41), #5 gestión de clientes (HU-06), #8 cuentas de cliente (HU-03), #10 pago Klap y #12 pago Flow (HU-39), #14 skill de exportación, #19 cambio de perfil, #27 firma avanzada (IN-01) | 28 commits |
| Marcos Álvarez | Sin commits | Sin commits | 6 commits, entre ellos el README del índice, `FUN-001` v1.1, `PLT-000` v1.5 y `12_evidencias/README.md` (27-10-2026) |

El número de commits no mide el esfuerzo por sí solo: un PR puede incluir trabajo
y revisión de varios integrantes. El desarrollo usó asistencia de IA (Claude
Code), que figura como coautor en los commits; eso se declara en la guía.

Para cada aporte que se cite en el informe, una fila:

| Integrante | Problema u objetivo | Trabajo concreto | PR, commit o documento | Resultado observado | Revisión de otro integrante | Aprendizaje, en palabras del autor |
|---|---|---|---|---|---|---|
| Por completar | — | — | — | — | — | — |

---

## 6. Bloques en inglés de la guía 2.4

Cada integrante tiene dos bloques pendientes: contribución y conclusión, y
reflexión. Las plantillas solo ordenan el texto; cada autor lo escribe con sus
propios hechos y lo revisa.

**Plantilla A · Contribution and conclusion**

> During this phase, my main contribution was [specific task actually completed],
> linked to [requirement or user story]. The evidence is in [PR, commit or
> document]. I worked on [concrete implementation or analysis] and collaborated
> with [person and verified activity]. The result was [observable outcome], which
> we checked through [test, review or demonstration]. One difficulty was [real
> difficulty]; I addressed it by [action taken], although [remaining limitation]
> still needs attention. In the next iteration I will improve [specific practice
> or deliverable], and I will consider it done when [measurable criterion].

**Plantilla B · Professional and teamwork reflection**

> A lesson I learned in this phase was [personal lesson], especially when [real
> situation]. Before, I [previous approach]; after [evidence or feedback], I
> changed [specific practice]. Working with the team required [actual
> coordination or review]. I contributed by [verified action] and received
> support through [verified interaction]. For the next phase I plan to [concrete
> improvement], checked through [review or evidence]. A limitation I still need
> to address is [honest gap].

Antes de entregar: reemplazar todos los corchetes, confirmar la extensión que
pide la pauta y revisar que ningún texto se atribuya trabajo de otro integrante.

---

## 7. Correspondencia con la pauta de la Fase 2

Fuente: `PLANILLA DE EVALUACION AVANCE FASE 2.xlsx`, hoja RUBRICA. Organiza la
evidencia; no es una nota.

| Criterio, resumido | Evidencia |
|---|---|
| Ajustes, dificultades, facilitadores y retroalimentación | `DEC-001`, cambios de alcance y retrospectiva real |
| Metodología aplicada a los objetivos | `BKL-001`, `SPR-001`, `DOD-001` y registros de review |
| Documentación, programación y datos | `TRZ-001`, modelos, PR, pruebas y documentos generados |
| Lenguaje técnico preciso | Firma asistida, avanzada de pruebas y con validez legal; diseño frente a implementación; plan frente a resultado |
| Redacción, ortografía y referencias | Guía coherente, resumen y abstract equivalentes |
| Estructura y evidencia en Git | Índice vigente, rutas y versiones |
| Contribución individual y equitativa | §5 de este documento y la reflexión de cada autor |
| Conocimiento y trabajo en equipo | Ensayo de la defensa y explicación compartida del flujo |

---

## 8. Preparación de la Fase 3

Las carpetas de la Fase 3 solo tienen `.gitkeep`. Índice provisional, a ajustar
cuando se publique la pauta oficial:

1. Informe final: problema, objetivos, alcance vigente y resultados.
2. Arquitectura y modelo de datos de la versión entregada.
3. Manuales técnico, de usuario y de operación, revisados ejecutándolos.
4. Evidencia de pruebas, migración, seguridad, rendimiento y recuperación
   efectivamente realizadas.
5. Limitaciones, decisiones pendientes de confirmación y trabajo futuro.
6. Presentación y demostración con guion, datos sintéticos y alternativa si un
   sandbox falla.
7. Evidencia y reflexión de cada integrante.
8. Inventario de entrega, commits y acta de traspaso cuando ocurra.

### Cierre por objetivo

| Objetivo | Entregable | Evidencia | Resultado | Pendiente |
|---|---|---|---|---|
| Centralizar la información | CRM con expediente relacionado | Por completar | No se evalúa solo por tener tablas | Por completar |
| Automatizar documentos | Generación de `PLT-01` y `PLT-02` | Por completar | Generación y firma se evalúan por separado | Por completar |
| Configurabilidad | Alta del caso acordado | Por completar | Declarar exactamente qué requirió código | Por completar |
| Migración | Importación trazable e idempotente | Por completar | Ensayo sintético separado de carga real | Por completar |
| Operación | Despliegue, manuales y recuperación | Por completar | Demostración separada de operación entregada | Por completar |

No se calcula un porcentaje global de logro sin definir qué se cuenta y cómo se
verifica.

---

## Historial de versiones

| Versión | Fecha | Cambios |
|---|---|---|
| 0.1 | 27-10-2026 | Versión inicial: estado metodológico, planificación, review, retrospectiva, aportes por integrante desde el historial de Git, plantillas en inglés, pauta y preparación de la Fase 3 |
