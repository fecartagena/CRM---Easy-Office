# Manifest · 2026-11-03 · auditoría (HU-30, HU-31)

| Campo | Valor |
|---|---|
| Candidato y fecha real | Rama principal con la auditoría fusionada · 03-11-2026, entre las 22:33 y las 22:37 (hora de Chile) |
| Commit del backend | `cfa51fa` (fusión del PR #62), cabeza de `main` ese día |
| Commit del frontend | `f37c58d` (fusión del PR #31), cabeza de `main` ese día |
| Commit de la documentación | `0920b02` |
| Ambiente, navegador y versiones | Local en el equipo Windows de Fernando Cartagena: backend con Docker Compose (`docker-compose.local.yml`) y frontend con Vite (`http://localhost:5173`). Google Chrome con la extensión Claude in Chrome, ventana de 1440 × 900. Versiones de Docker y Node no registradas |
| Pago, PDF y firma | No intervienen en este caso |
| Datos | Sintéticos: cuentas de demostración (Administrador Demo, Ejecutivo Demo), el cliente de prueba `CLI-000023` y el correo inventado `prueba.limite@example.test` |
| Quién ejecutó y quién revisó | Ejecutó Claude (Claude in Chrome) a pedido de Fernando Cartagena; los ingresos con cuentas de demostración los hizo Fernando. Revisor: Nicolás Zapata |
| Casos ejecutados, aprobados, fallidos y bloqueados | 1 ejecutado (EVI-T11): aprobado, con los límites de `resultados.md` |
| Defectos y decisión | Sin defectos en la pantalla. Decisión en `EVI-001` |

Que el código local corresponda a esos commits lo declaró Fernando Cartagena ("ya
está fusionado en main"); no se ejecutó `git rev-parse HEAD` en ese equipo.
