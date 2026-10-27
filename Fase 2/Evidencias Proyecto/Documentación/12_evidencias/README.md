# Evidencias

Respaldo de las sesiones con Easy Office y de las decisiones del proyecto.

**Estado al 27 de octubre de 2026: esta carpeta contiene solo este README.** Lo
que figura en la tabla está previsto y todavía no se ha versionado.

| Contenido | Formato | Condición | Estado |
|---|---|---|---|
| Transcripciones de las sesiones de levantamiento (`T1`, `T2`) | .md / .pdf | Anonimizadas antes de versionarse | Pendiente. `ACTA-001` y `MRQ-001` las citan como fuente |
| Actas firmadas o confirmadas por correo | .pdf | Sin datos personales de clientes | Pendiente. Las decisiones de `ACTA-002` siguen sujetas a su confirmación escrita |
| Grabaciones de reuniones | Enlace a carpeta de acceso restringido | No se versionan en el repositorio | Pendiente: dejar el enlace |
| Capturas de demostraciones de cierre de sprint | .png | Solo con datos sintéticos | Pendiente |

Este repositorio es público: ningún archivo de esta carpeta puede contener datos
personales reales de clientes de Easy Office.

---

## Qué cuenta como evidencia

Las capturas de los prototipos (`06_ux_mockups/capturas`) muestran el diseño, no
el funcionamiento del sistema, y no sustituyen una evidencia de pruebas.

Las evidencias de pruebas se ubican según `PLA-001` §6: el reporte de cobertura
en `09_pruebas/sprint-N/`, el de rendimiento en `09_pruebas/rendimiento/` y el de
seguridad en `09_pruebas/seguridad/`. Esas carpetas tampoco existen todavía.

Toda evidencia de una ejecución registra:

| Campo | Qué registrar |
|---|---|
| Fecha y hora | Cuándo se ejecutó |
| Repositorio y commit | El commit probado (`git rev-parse HEAD`) |
| Entorno | Sistema operativo, versiones de Python y Node, base de datos, y si fue local, en CI o en un ambiente de pruebas |
| Qué se ejecutó | El comando exacto o el enlace a la ejecución de CI |
| Resultado | Pruebas que pasan y que fallan, con las cifras |
| Archivo o enlace | El reporte, la captura o el enlace que lo respalda |

Solo se registra lo que efectivamente se ejecutó. Una prueba que no se corrió se
indica como no ejecutada, con su motivo.
