# Documentación del proyecto · CRM Easy Office

Estado al **9 de octubre de 2026** (semana 9 del proyecto), después de la reunión de validación con Easy Office (`ACTA-002`).

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
├── 09_pruebas/            plan y evidencias de pruebas
├── 10_riesgos/            registro de riesgos
├── 11_actas/              actas de reunión y checklists
├── 12_evidencias/         transcripciones y respaldos
└── 13_plantillas/         plantillas documentales anonimizadas y diccionario de campos
```

---

## Documentos disponibles

### Artefactos de la metodología (Scrum)

| Código | Documento | Carpeta | Versión |
|---|---|---|---|
| PV-001 | Product Vision | 03 | 2.0 |
| BKL-001 | Product Backlog | 04 | 2.5 |
| SPR-001 | Sprint Backlog · Sprint 1 | 04 | 1.1 |
| DOD-001 | Definition of Done | 04 | 1.0 |
| DIS-001 | Documento de diseño | 07 | 1.2 |
| RET-001 | Registro de retrospectivas | 04 | 1.0 |
| GNT-001 | Carta Gantt, hitos, plan de trabajo y sprints (.xlsx + .pdf) | 05 | 1.1 |
| PLA-001 | Plan de pruebas | 09 | 1.2 |

### Elementos obligatorios independientes de la metodología

| Código | Documento | Carpeta | Versión |
|---|---|---|---|
| DIP-001 | Documento de inicio de proyecto | 03 | 1.0 |
| ARQ-001 | Arquitectura: diagrama, componentes, comunicación, secuencia | 07 | 1.2 |
| MOD-001 | Modelo de datos (ER) | 08 | 1.5 |
| UML-001 | Casos de uso | 07 | 1.2 |
| MRQ-001 | Requerimientos, incluidos los no funcionales | 02 | 2.4 |

### Documentación complementaria

| Código | Documento | Carpeta | Versión |
|---|---|---|---|
| ACTA-001 | Acta de levantamiento de requerimientos | 01 | 1.0 |
| ESP-DOC | Especificaciones de documentos entregadas por Easy Office | 01 | — |
| CAT-001 | Catálogo de identificadores | 02 | 1.4 |
| RN-001 | Reglas de negocio | 02 | 2.4 |
| TRZ-001 | Matriz de trazabilidad | 02 | 2.4 |
| ALC-001 | Alcance del MVP | 03 | 2.0 |
| FUN-001 | Ficha funcional del servicio piloto | 03 | 1.0 |
| REC-001 | Recomendación técnica | 07 | 1.0 |
| RSK-001 | Registro de riesgos | 10 | 1.5 |
| CHK-001 | Checklist de validación con la contraparte | 11 | 1.0 |
| ACTA-002 | Reunión de validación con Easy Office, 08-10-2026 | 11 | 1.5 |
| PLT-000 | Catálogo de plantillas, diccionario de campos y regla del folio | 13 | 1.2 |
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
9. `MOD-001` — cómo se guardan los datos
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
- `T1`, `T2` — sesiones de levantamiento, transcripciones en `12_evidencias/`
- `A2` — reunión de validación del 8 de octubre de 2026, registrada en `ACTA-002`
- `EQ` — decisión del equipo

---

## Documentos pendientes

| Documento | Cuándo corresponde |
|---|---|
| UML: diagrama de clases | Cuando exista el modelo implementado |
| UML: diagrama de componentes en notación UML | Sprint 2 |
| Dockerfile, docker-compose y variables de entorno | Repositorio de backend |
| Evidencia de la retrospectiva del Sprint 1 | Al cierre del sprint |
| Manual técnico y de despliegue | Etapa de cierre (semanas 16 a 18) |
| Sección de innovación | Documento de cierre |
