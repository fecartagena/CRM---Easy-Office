# Documentación del proyecto · CRM Easy Office

Estado al **27 de octubre de 2026** (semana 12 del proyecto, Sprint 3 en ejecución). La reunión de validación con Easy Office del 8 de octubre está en `ACTA-002`.

---

## Estructura

```
Documentación/
├── 01_levantamiento/      evidencia del levantamiento con la contraparte
├── 02_requerimientos/     requerimientos, reglas de negocio, trazabilidad
├── 03_alcance/            alcance del MVP y visión de producto
├── 04_metodologia/        artefactos Scrum
├── 05_planificacion/      Carta Gantt e hitos
├── 06_ux_mockups/         prototipos
├── 07_arquitectura/       arquitectura, UML y recomendación técnica
├── 08_datos/              modelo de datos y migración
├── 09_pruebas/            plan de pruebas (las evidencias están pendientes)
├── 10_riesgos/            registro de riesgos
├── 11_actas/              actas de reunión y checklists
├── 12_evidencias/         respaldos del proyecto (hoy solo su README)
└── 13_plantillas/         plantillas documentales anonimizadas y diccionario de campos
```

---

## Documentos disponibles

### Artefactos de la metodología (Scrum)

| Código | Documento | Carpeta | Versión |
|---|---|---|---|
| PV-001 | Product Vision | 03 | 2.0 |
| BKL-001 | Product Backlog | 04 | 2.11 |
| SPR-001 | Sprint Backlog · sprints 1 a 3 | 04 | 2.1 |
| DOD-001 | Definition of Done | 04 | 1.0 |
| DIS-001 | Documento de diseño | 07 | 1.5 |
| RET-001 | Registro de retrospectivas | 04 | 1.1 |
| GNT-001 | Carta Gantt, hitos, plan de trabajo y sprints (.xlsx + .pdf) | 05 | 1.1 |
| PLA-001 | Plan de pruebas | 09 | 1.10 |

### Elementos obligatorios independientes de la metodología

| Código | Documento | Carpeta | Versión |
|---|---|---|---|
| DIP-001 | Documento de inicio de proyecto | 03 | 1.0 |
| ARQ-001 | Arquitectura: diagrama, componentes UML, comunicación, secuencia | 07 | 1.6 |
| MOD-001 | Modelo de datos (ER) | 08 | 1.10 |
| UML-002 | Diagrama de clases | 07 | 1.1 |
| UML-001 | Casos de uso | 07 | 1.2 |
| MRQ-001 | Requerimientos, incluidos los no funcionales | 02 | 2.6 |

### Documentación complementaria

| Código | Documento | Carpeta | Versión |
|---|---|---|---|
| ACTA-001 | Acta de levantamiento de requerimientos | 01 | 1.0 |
| ESP-DOC | Especificaciones de documentos entregadas por Easy Office | 01 | — |
| CAT-001 | Catálogo de identificadores | 02 | 1.5 |
| RN-001 | Reglas de negocio | 02 | 2.8 |
| TRZ-001 | Matriz de trazabilidad | 02 | 2.12 |
| ALC-001 | Alcance del MVP | 03 | 2.0 |
| FUN-001 | Ficha funcional del servicio piloto | 03 | 1.1 |
| REC-001 | Recomendación técnica | 07 | 1.0 |
| RSK-001 | Registro de riesgos | 10 | 1.5 |
| CHK-001 | Checklist de validación con la contraparte | 11 | — |
| ACTA-002 | Reunión de validación con Easy Office, 08-10-2026 | 11 | 1.8 |
| PLT-000 | Catálogo de plantillas, diccionario de campos y regla del folio | 13 | 1.5 |
| PLT-01 a PLT-05 | Plantillas documentales anonimizadas | 13 | — |

Las versiones anteriores de MRQ-001, ALC-001, RN-001, TRZ-001 y PV-001 se
conservan en `_historico/` como evidencia de la evolución del proyecto.

**Los identificadores del proyecto están definidos en `CAT-001`.** No se
reutilizan entre versiones.

## Orden de lectura sugerido

Para entender el proyecto de cero, en este orden:

0. `CAT-001` — qué significa cada código
1. `ACTA-001` — qué dijo la contraparte
   - `ACTA-002` — qué confirmó en la validación, y `13_plantillas`, con qué
     documentos se trabaja
2. `PV-001` — qué se va a construir y por qué
3. `ALC-001 v2` — qué entra y qué no
4. `FUN-001` — cómo funciona el servicio piloto
5. `MRQ-001 v2` — qué exactamente
6. `RN-001 v2` — bajo qué reglas
7. `REC-001` — con qué tecnología y por qué
8. `ARQ-001` — cómo se estructura
9. `MOD-001` — cómo se guardan los datos, y `UML-002`, cómo quedaron las clases en el código
10. `UML-001` — quién hace qué
11. `BKL-001` + `DOD-001` — cómo se construye
12. `RSK-001` — qué puede fallar
13. `TRZ-001` — cómo se demuestra que se cumplió

---

## Convenciones

**Estados usados en todos los documentos**

- *Confirmado* — declarado o escrito por la contraparte
- *Propuesto* — decisión o propuesta del equipo
- *Pendiente de validación* — sin resolver

**Marcas**

- `[PENDIENTE DE VALIDAR CON EASY OFFICE]` — falta información de la contraparte
- `[PROPUESTA – VALIDAR CON EASY OFFICE]` — regla inferida, no enunciada

**Fuentes citadas**

- `ESP §n` — especificaciones formales entregadas por Easy Office
- `T1`, `T2` — sesiones de levantamiento (`ACTA-001`). Sus transcripciones anonimizadas están pendientes de versionarse en `12_evidencias/`
- `A2` — reunión de validación del 8 de octubre de 2026, registrada en `ACTA-002`
- `EQ` — decisión del equipo

---

## Documentos pendientes

Estado al 27 de octubre de 2026. Cada documento pendiente debe describir lo que
efectivamente se hizo: lo que no se ejecutó se registra como no ejecutado.

| Documento | Qué falta | Cuándo corresponde |
|---|---|---|
| Retrospectivas en `RET-001` | Las secciones del Sprint 1 conservan «Por completar el 3 de octubre» y el Sprint 2 no está registrado. Registrar lo que efectivamente se hizo; si una sesión no ocurrió, indicarlo y hacer una revisión posterior con su fecha real | Con el aporte de los tres integrantes |
| Evidencia de pruebas (`PLA-001` §6) | Resultado de la integración continua, cobertura (`09_pruebas/sprint-N/`), rendimiento (`09_pruebas/rendimiento/`), seguridad (`09_pruebas/seguridad/`) y aceptación. Cada evidencia con fecha, commit, entorno y resultado. Las capturas del mockup no prueban el funcionamiento del sistema | Pruebas, correcciones y validación: semanas 9 a 15 (`GNT-001`) |
| Evidencias de las sesiones | Transcripciones `T1` y `T2` anonimizadas, confirmación escrita de `ACTA-002` y capturas de las demostraciones, en `12_evidencias/`. `ACTA-001` afirma que las transcripciones se conservan ahí, y hoy no están | Por planificar |
| Manual técnico y de despliegue | Procedimiento integrado para frontend, backend, HTTPS, variables de entorno, migraciones, workers, almacenamiento documental, respaldos, restauración y reversión de una actualización. Los README de los repositorios son el punto de partida. Depende del alojamiento productivo (TA-09) | Etapa de cierre, semanas 16 a 18 (hito H8) |
| Manual de usuario y aceptación | Manual operativo para el Administrador, el Ejecutivo y el cliente del portal, con los recorridos principales y los errores frecuentes, y una lista de aceptación que registre qué probó y aprobó Easy Office | Por planificar |
| Plan de migración | Mapeo de la planilla Excel al modelo, duplicados, folios históricos (TA-03), fechas inconsistentes, registros rechazados, conciliación de cantidades y reversión. El diccionario de campos y la muestra sintética ya existen en `13_plantillas`. Depende de la planilla completa anonimizada (TA-10) | Antes del hito H5 (31-10-2026) |
| Plantilla de Pull Request y CI del frontend | Plantilla de PR con la checklist de `DOD-001` en los repositorios de código, y un flujo de CI en el frontend que ejecute pruebas, tipos, lint y compilación. Hoy el frontend no tiene CI | Por planificar |
| Sección de innovación | Pendiente | Documento de cierre |

---

## Límites funcionales del estado actual

Estos tres límites deben quedar explícitos en la entrega. Verificados contra la
rama principal del backend el 27-10-2026.

1. **La firma es asistida.** Un ejecutivo sube el PDF firmado. Eso no acredita
   integración automática con tufirma.digital: las credenciales siguen pendientes
   (TA-07). Existe además una firma avanzada contra un prestador de pruebas propio
   y ficticio, sin validez legal.
2. **El motor configurable está parcialmente construido.** Hoy funcionan de
   extremo a extremo dos plantillas, `PLT-01` y `PLT-02`, cada una con su propio
   convertidor. `docs/variation-points.md`, en el repositorio del backend, registra
   qué difiere entre ambas. No debe prometerse que cualquier documento se
   incorpora sin código.
3. **La auditoría no está en la rama principal del backend.** El código marca los
   puntos donde se registrará (`TODO(RF-15)`, HU-30). Las 696 pruebas que cita
   `PLA-001` v1.10 corresponden a la rama de auditoría, sin fusionar, y no a la
   rama principal.
