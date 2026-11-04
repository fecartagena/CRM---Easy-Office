# Resultados · 2026-11-03 · auditoría

## EVI-T11 · Integrar la auditoría, modificar un cliente, consultar el registro e intentar alterarlo

Resultado esperado: actor, fecha y cambio registrados; el historial no se puede
editar; solo el Administrador consulta.

| Paso | Resultado observado | Captura |
|---|---|---|
| Como Administrador, editar el cliente de prueba `CLI-000023` y agregar el teléfono ficticio +56 9 1234 5678 | Guardado; la ficha muestra la nueva "Última modificación" | — |
| Ver el historial en la ficha del cliente | La sección "Registro de auditoría" lista la modificación con fecha y hora de Chile, usuario, canal "Backoffice", entidad "Persona" e IP | `capturas/EVI-T11_03-historial-en-ficha.jpg` |
| Abrir el detalle de la fila | "Telefono: (vacío) → +56 9 1234 5678", con usuario, canal, registro e IP | `capturas/EVI-T11_02-detalle-modificacion.jpg` |
| "Ver solo este registro" | `/backoffice/auditoria?entidad=clientes.persona&registro_id=25`, con las tres modificaciones de esa persona | `capturas/EVI-T11_01-registro-filtrado.jpg` |
| Filtrar con la fecha final antes de la inicial | "La fecha final no puede ser anterior a la inicial", junto al filtro; no se muestran filas | `capturas/EVI-T11_04-error-hasta-antes-de-desde.jpg` |
| Cerrar sesión y probar 11 ingresos con un correo inventado y una contraseña errónea | Del 1 al 10: "Correo o contraseña incorrectos". El 11: "Hiciste demasiados intentos de ingreso. Espera un momento e intenta nuevamente." | `capturas/EVI-T11_06-ingreso-429.jpg` |
| Ingresar como Ejecutivo | El menú no muestra "Auditoría" | `capturas/EVI-T11_05-ejecutivo-sin-acceso.jpg` |
| Abrir `/backoffice/auditoria` como Ejecutivo | Pantalla "Sin acceso"; `GET /api/auditoria/` con esa sesión responde `403` | `capturas/EVI-T11_05-ejecutivo-sin-acceso.jpg` |

**No repetido en esta ejecución** (sí en la del 02-11-2026, sobre las mismas cabezas
antes de fusionar): que `POST`, `PUT`, `PATCH` y `DELETE` respondan `405` o `404`
sin cambiar el total de filas; que el intento 11 no deje fila; y que el ingreso y el
cierre de sesión aparezcan en el registro.

**Límite.** En local la IP registrada es la de la red de Docker (`172.18.0.1`): no
prueba la dirección real detrás del proxy. Queda pendiente comprobarlo en staging.
