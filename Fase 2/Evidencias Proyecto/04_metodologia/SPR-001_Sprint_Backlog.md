# Sprint Backlog

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | SPR-001 · Sprint Backlog |
| **Sprint** | 1 · Núcleo de datos y accesos |
| **Período** | 22 de septiembre – 3 de octubre de 2026 |
| **Versión** | 1.0 |
| **Estado** | En ejecución |
| **Preparado por** | Equipo de proyecto |

---

## 1. Objetivo del sprint

> Dejar el entorno de desarrollo operativo y el núcleo de datos y accesos
> funcionando: un usuario con rol puede autenticarse, registrar un cliente con su
> empresa y su representante legal, y cada acción queda registrada en auditoría.

Es la base sobre la que se construye todo lo demás. No incluye trámites,
documentos ni portal.

## 2. Contexto

Las semanas 1 a 6 del proyecto (10 de agosto – 20 de septiembre) se dedicaron al levantamiento, la especificación y el diseño.
Este es el **primer sprint de construcción**. El calendario de sprints proyectado
es:

| Sprint | Período | Foco |
|---|---|---|
| **1** | 22 sep – 3 oct | Entorno, núcleo de datos y accesos |
| 2 | 6 – 17 oct | Servicios contratados, vigencias y migración |
| 3 | 20 – 31 oct | Motor configurable y generación documental |
| 4 | 3 – 14 nov | Portal de contratación e integraciones |
| 5 | 17 – 28 nov | Pruebas, despliegue y estabilización |

## 3. Capacidad

| Integrante | Disponibilidad estimada | Puntos comprometidos |
|---|---|---|
| Fernando Cartagena | ~10 h/semana | 8 |
| Nicolás Zapata | ~10 h/semana | 13 |
| Marcos Álvarez | ~10 h/semana | 8 |
| **Total** | **~60 h** | **29** |

Al ser el primer sprint no hay velocidad histórica. La estimación es conservadora
a propósito: el resultado de este sprint calibra los siguientes.

---

## 4. Trabajo comprometido

### Preparación del entorno

| ID | Tarea | Resp. | Pts | Estado |
|---|---|---|---|---|
| T-01 | Crear los repositorios de backend y frontend, públicos, con su contexto y README | Fernando | 1 | Pendiente |
| T-02 | Generar el proyecto Django con su estructura base y podar dependencias no utilizadas | Nicolás | 2 | Pendiente |
| T-03 | Levantar el entorno con Docker Compose: PostgreSQL, Redis, aplicación y worker | Nicolás | 3 | Pendiente |
| T-04 | Configurar GitHub Projects con las historias del sprint y el flujo de estados | Marcos | 1 | Pendiente |
| T-05 | Definir la plantilla de Pull Request con la Definition of Done como lista de verificación | Marcos | 1 | Pendiente |

### Historias de usuario

| ID | Historia | Req. | Resp. | Pts | Estado |
|---|---|---|---|---|---|
| HU-01 | Como ejecutivo, quiero iniciar sesión con mis credenciales, para acceder al sistema con mi identidad | RF-01 | Nicolás | 2 | Pendiente |
| HU-02 | Como administrador, quiero crear y desactivar usuarios internos, para controlar quién accede | RF-03 | Nicolás | 2 | Pendiente |
| HU-04 | Como administrador, quiero asignar un rol a cada usuario, para que acceda solo a lo que le corresponde | RF-02 | Nicolás | 3 | Pendiente |
| HU-05 | Como supervisor, quiero que los ejecutivos no puedan eliminar registros de clientes, para evitar pérdidas de información | RF-04 | Nicolás | 2 | Pendiente |
| HU-06 | Como ejecutivo, quiero registrar un cliente con sus datos de contacto, para tenerlo disponible en el sistema | RF-05 | Fernando | 3 | Pendiente |
| HU-51 | Como sistema, quiero asignar un identificador único a cada cliente, para poder referenciarlo de forma inequívoca | RF-07 | Fernando | 1 | Pendiente |
| HU-08 | Como ejecutivo, quiero registrar una empresa con su RUT, razón social y representante legal, para emitir documentos a su nombre | RF-04 | Fernando | 3 | Pendiente |
| HU-09 | Como ejecutivo, quiero registrar la vigencia de la representación legal, para saber si quien firma está facultado | RN-06 | Marcos | 2 | Pendiente |
| HU-30 | Como sistema, quiero registrar qué usuario realizó cada acción, con fecha, valor anterior y valor nuevo, para dejar trazabilidad | RF-15, RF-16 | Marcos | 3 | Pendiente |
| HU-45 | Como equipo, quiero pruebas de control de acceso por rol, para verificar que los permisos se respetan | RS-02 | Marcos | 2 | Pendiente |

**Total comprometido: 8 puntos de preparación + 23 de historias = 31.**

Dos puntos sobre la capacidad estimada. Se asume porque las tareas de entorno
tienen alta incertidumbre y pueden resultar menores de lo estimado. Si al cierre
de la primera semana el avance va por debajo de la mitad, se descarta HU-09 por
ser la de menor dependencia.

---

## 5. Criterios de aceptación del sprint

El sprint se considera cumplido si al 3 de octubre:

- [ ] `docker compose up` levanta el entorno completo desde cero en un equipo
      limpio
- [ ] Las migraciones corren sin errores sobre una base vacía
- [ ] Un usuario administrador puede crear usuarios y asignarles rol
- [ ] Un usuario con rol ejecutivo no puede eliminar un cliente, y la restricción
      está cubierta por una prueba automatizada
- [ ] Se puede registrar un cliente persona natural y uno persona jurídica con su
      representante legal
- [ ] Toda creación y modificación queda registrada en auditoría con usuario,
      fecha, valor anterior y valor nuevo
- [ ] El registro de auditoría no admite modificación ni eliminación
- [ ] Las pruebas del sprint pasan
- [ ] Ningún dato personal real ni credencial fue versionado

---

## 6. Fuera de este sprint

Se declara para evitar ambigüedad:

| Elemento | Sprint previsto |
|---|---|
| Servicios contratados y vigencias | 2 |
| Búsqueda, filtros y ficha del cliente | 2 |
| Migración desde planillas | 2 |
| Motor configurable | 3 |
| Plantillas y generación documental | 3 |
| Portal de contratación | 4 |
| Integraciones de firma y pago | 4 |
| Dashboard de indicadores | 4 |
| Alertas de vencimiento | 2 |

---

## 7. Dependencias y bloqueos

| # | Dependencia | Afecta a | Estado |
|---|---|---|---|
| D-01 | Listado definitivo de campos del cliente | HU-06, HU-08 | Pendiente. Se avanza con los campos conocidos del levantamiento y se amplía al recibirlo |
| D-02 | Definición de roles adicionales al Administrador y Ejecutivo | HU-04 | Pendiente. Se implementan los dos confirmados |
| D-03 | Estructura de la planilla a migrar | Sprint 2 | Pendiente |

Ninguna dependencia bloquea el sprint en curso.

---

## 8. Riesgos activos en este sprint

| ID | Riesgo | Acción en el sprint |
|---|---|---|
| R-06 | Alcance excesivo | El sprint excluye explícitamente todo lo listado en la sección 6 |
| R-08 | Carga del equipo | Estimación conservadora; HU-09 identificada como descartable |
| R-05 | Datos reales en repositorio público | Verificación incorporada a la plantilla de Pull Request |

---

## 9. Seguimiento

| Fecha | Instancia | Participantes |
|---|---|---|
| 22 sep | Sprint Planning | Equipo |
| 24, 26, 29 sep · 1, 3 oct | Seguimiento asíncrono | Equipo |
| 3 oct | Sprint Review con demostración | Equipo |
| 3 oct | Retrospectiva | Equipo · se registra en `RET-001` |

---

## Control de cambios

| Versión | Fecha | Descripción | Autor |
|---|---|---|---|
| 1.0 | 24-09-2026 | Emisión inicial | Equipo de proyecto |
