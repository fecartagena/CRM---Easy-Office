# Definition of Done

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | DOD-001 · Definition of Done |
| **Versión** | 1.0 |
| **Fecha** | 24 de septiembre de 2026 |
| **Ámbito** | Aplica a toda historia de usuario del Product Backlog |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## Qué es y para qué sirve

La Definition of Done es el acuerdo del equipo sobre qué significa que una
historia está terminada. No describe qué hace la funcionalidad — eso son los
criterios de aceptación de cada historia — sino qué condiciones debe cumplir
cualquier trabajo antes de considerarse cerrado.

Existe para evitar la situación más común en proyectos de este tipo: historias
marcadas como terminadas que en realidad están a medias, y que reaparecen como
deuda en el sprint siguiente.

Una historia que no cumple todos los puntos **no se mueve a Hecho**, aunque el
código funcione.

---

## Criterios

### Funcionalidad

- [ ] Los criterios de aceptación de la historia se cumplen en su totalidad
- [ ] La funcionalidad se probó manualmente sobre el entorno local levantado con
      `docker-compose`
- [ ] Los casos de error previstos están manejados y no exponen trazas internas
      al usuario

### Código

- [ ] El código está integrado en la rama principal mediante Pull Request
- [ ] El Pull Request fue revisado y aprobado por otro integrante del equipo
- [ ] No hay código comentado ni depuración olvidada
- [ ] Los nombres del modelo de dominio están en español y el resto en inglés,
      según la convención del proyecto
- [ ] No se agregaron dependencias nuevas sin acuerdo del equipo

### Pruebas

- [ ] La lógica de negocio nueva tiene pruebas unitarias y pasan
- [ ] La suite completa de pruebas pasa
- [ ] Si la historia toca permisos, existe una prueba que verifica que un rol sin
      la facultad correspondiente recibe un error de autorización

### Trazabilidad

- [ ] La historia tiene su issue en GitHub Projects
- [ ] Los commits referencian el issue
- [ ] La matriz de trazabilidad `TRZ-001` está actualizada con el issue, el
      commit y la prueba correspondientes

### Documentación

- [ ] La documentación afectada está actualizada: `MRQ-001`, `MOD-001`, `ARQ-001`
      o el README, según corresponda
- [ ] Si la historia cambió el modelo de datos, la migración está incluida y
      corre limpia desde una base vacía

### Entorno

- [ ] El sistema levanta desde cero con `docker-compose` sin pasos manuales no
      documentados
- [ ] Las variables de entorno nuevas están documentadas en el archivo de ejemplo

### Seguridad y datos

- [ ] No se versionaron credenciales, claves ni secretos
- [ ] No se versionaron datos personales reales, en ningún formato: fixtures,
      pruebas, capturas, comentarios o migraciones
- [ ] Si la historia toca información de clientes, la acción queda registrada en
      auditoría

---

## Definición de "listo para empezar"

Complemento de la anterior. Una historia no entra a un sprint si no cumple:

- [ ] Está redactada en formato historia de usuario, con su beneficio explícito
- [ ] Tiene criterios de aceptación escritos
- [ ] Sus dependencias están identificadas y resueltas o planificadas
- [ ] Está estimada por el equipo
- [ ] No depende de información que todavía no entrega la contraparte, o bien esa
      dependencia está explícitamente acotada mediante una implementación
      simulada o datos sintéticos

Este último punto es el que más aplica en este proyecto: varias historias
dependen de las plantillas, de la planilla Excel o de las credenciales de firma.
Una historia bloqueada por una dependencia externa no entra al sprint hasta que
el equipo defina cómo avanzar sin ella.

---

## Excepciones

No hay excepciones al bloque de seguridad y datos. El repositorio es público y
el sistema manejará información personal de clientes reales.

Cualquier otra excepción se acuerda explícitamente en el Sprint Planning, se
registra en el issue de la historia y se resuelve antes del cierre del sprint
siguiente.

---

## Revisión

Esta definición se revisa en cada retrospectiva. Si un criterio resulta
inaplicable o si aparece un problema recurrente que no está cubierto, se ajusta y
se registra la nueva versión acá.

| Versión | Fecha | Cambios |
|---|---|---|
| 1.0 | 24-09-2026 | Versión inicial |
