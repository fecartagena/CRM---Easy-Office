# Documento de diseño

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | DIS-001 · Documento de diseño |
| **Versión** | 1.0 |
| **Fecha** | 24 de septiembre de 2026 |
| **Estado** | Emitido para revisión |
| **Preparado por** | Equipo de proyecto |
| **Clasificación** | Uso interno del proyecto |

---

## 1. Propósito y estructura

Este documento consolida el diseño de la solución. No duplica el contenido de los
documentos especializados: los organiza, explica cómo se relacionan y registra las
decisiones transversales.

| Vista | Documento | Contenido |
|---|---|---|
| Funcional | `FUN-001` | Diseño detallado del servicio piloto: qué se vende, qué documentos se emiten, quién firma, cuándo se paga, excepciones |
| Comportamiento | `UML-001` | Actores, casos de uso y sus especificaciones |
| Lógica | `ARQ-001` | Estilo arquitectónico, módulos, comunicación, despliegue |
| Datos | `MOD-001` | Modelo entidad-relación, entidades, índices, migración |
| Tecnológica | `REC-001` | Stack recomendado y su justificación frente a los criterios del cliente |
| Requerimientos | `MRQ-001`, `RN-001` | Qué debe hacer el sistema y bajo qué reglas |

**Lectura recomendada:** `FUN-001` → `UML-001` → `ARQ-001` → `MOD-001`.

---

## 2. Principios de diseño

Cinco decisiones transversales orientan el resto del diseño.

**2.1 Un solo dominio, dos interfaces.** El CRM y el portal operan sobre una misma
base de datos. No son sistemas que se sincronizan. Esto responde al requisito de
que la información ingresada por el cliente no se vuelva a digitar internamente:
sincronizar introduciría precisamente el problema que se busca eliminar.

**2.2 La configuración es dato, no código.** Tipos de servicio, tipos de trámite,
campos, validaciones, estados, transiciones, plantillas y roles se administran
como registros. Incorporar un servicio nuevo no debería requerir un despliegue.

**2.3 Los documentos emitidos son inmutables.** Un documento queda asociado a la
versión de plantilla con que se generó y a una copia congelada de los datos
usados. Modificar la ficha de un cliente no altera documentos ya emitidos.

**2.4 Las dependencias externas viven tras interfaces.** Firma electrónica y
medios de pago se consumen a través de contratos definidos por el sistema, con
implementaciones intercambiables. Ninguna decisión pendiente del cliente bloquea
el desarrollo.

**2.5 Lo que se registra no se altera.** La auditoría es un registro de solo
escritura. No existe funcionalidad de modificación ni de eliminación, por diseño.

---

## 3. Vista funcional

El comportamiento del sistema se organiza en diecisiete casos de uso agrupados en
cuatro bloques: portal de contratación, CRM interno, administración y procesos
automáticos. Su especificación está en `UML-001`.

El caso de uso central es **UC-01 Contratar servicio en línea**, que recorre desde
la selección del servicio hasta el registro del servicio contratado con su
vigencia. Los demás casos de uso del portal son derivaciones o consultas sobre
ese flujo.

El servicio piloto, domicilio tributario, está diseñado en detalle en `FUN-001`,
incluidos sus documentos, firmantes, momento del pago, vigencia, renovación y
excepciones.

> **Estado.** `FUN-001` registra que el servicio piloto está descrito de forma
> divergente entre el levantamiento, las especificaciones del cliente y los
> prototipos. Ocho preguntas quedan abiertas, cuatro de las cuales bloquean la
> implementación. El diseño funcional no se considera cerrado hasta resolverlas.

---

## 4. Vista lógica

**Estilo: monolito modular.** Una aplicación, una base de datos, un despliegue,
con módulos de fronteras explícitas por dentro.

| Módulo | Responsabilidad | Depende de |
|---|---|---|
| `core` | Usuarios, roles, permisos, auditoría | — |
| `clientes` | Personas, empresas, representación legal | `core` |
| `servicios` | Servicios contratados, vigencias, alertas | `core`, `clientes` |
| `inmuebles` | Oficinas, rol de avalúo, domicilios asignados | `core`, `servicios` |
| `tramites` | Tipos configurables, trámites, máquina de estados | `core`, `clientes`, `servicios` |
| `documentos` | Plantillas versionadas, generación, firmantes | `core`, `tramites` |
| `integrations` | Interfaces de firma y pago | `core` |
| `migracion` | Carga desde planillas | `clientes`, `servicios` |

Los módulos se comunican mediante funciones de servicio públicas, no consultando
los modelos de otro módulo. La dirección de dependencia es descendente y no
admite ciclos.

La justificación de descartar una arquitectura distribuida, las vistas de
componentes y despliegue, y el detalle de la comunicación asíncrona están en
`ARQ-001`.

---

## 5. Vista de datos

El modelo relacional cubre ocho áreas: usuarios y permisos, clientes, servicios
contratados, inmuebles, trámites, documentos, pagos y auditoría. El diagrama
entidad-relación y el detalle de cada entidad están en `MOD-001`.

Decisiones de modelado que conviene destacar:

| Decisión | Motivo |
|---|---|
| Trámite y servicio contratado son entidades distintas | El trámite es la operación con su flujo; el servicio es lo que el cliente tiene vigente. Una renovación crea un servicio nuevo sin borrar el anterior |
| Persona, empresa y representación legal son distintas | Un contrato lo firma el representante legal, con una vigencia que importa |
| La máquina de estados es una tabla | Agregar un estado a un servicio nuevo no requiere modificar código |
| Configuración y datos variables en campos JSONB | Permite estructura variable por servicio sin renunciar a consultarla |
| Auditoría append-only mediante disparadores de base de datos | Más difícil de eludir que el código de aplicación |

---

## 6. Vista tecnológica

| Capa | Tecnología | Justificación resumida |
|---|---|---|
| Frontend | React | Dos interfaces con componentes compartidos; amplia disponibilidad de profesionales |
| Backend | Django + Django REST Framework | Autenticación, permisos y panel de administración de fábrica; el panel es lo que permite configurar sin desarrollo |
| Base de datos | PostgreSQL | Integridad referencial y transacciones, con soporte JSONB indexable para la parte configurable |
| Asíncrono | Celery + Redis | Generación documental, llamadas externas y detección programada de vencimientos |
| Contenedores | Docker | Paridad entre entornos y portabilidad entre proveedores de alojamiento |

La evaluación completa frente a los once criterios planteados por el cliente
(base de datos, alojamiento, seguridad, respaldos, control de accesos, auditoría,
API, escalabilidad, costos recurrentes, mantenibilidad y dependencia de
proveedores) está en `REC-001`.

---

## 7. Diseño de seguridad

| Control | Implementación |
|---|---|
| Autenticación | Credenciales individuales; sin cuentas compartidas |
| Autorización | Permisos por rol, con la restricción de que el perfil ejecutivo no elimina registros |
| Sesión | Cookies httpOnly; no se usa almacenamiento del navegador para datos de sesión ni de negocio |
| Transporte | HTTPS obligatorio |
| Secretos | Variables de entorno, fuera del código y del repositorio |
| Datos en reposo | Cifrado de los documentos almacenados |
| Minimización | Solo se solicitan los campos que el servicio requiere |
| Trazabilidad | Registro inalterable de acciones sobre información de clientes |
| Desarrollo | Datos anonimizados; ningún dato real se versiona |

El repositorio del proyecto es público, lo que convierte la última línea en una
condición no negociable, incorporada a la Definition of Done.

---

## 8. Atributos de calidad

| Atributo | Cómo se aborda | Cómo se verifica |
|---|---|---|
| Seguridad | Controles de la sección 7 | Pruebas de control de acceso por rol |
| Rendimiento | Índices sobre los criterios de búsqueda declarados; procesamiento asíncrono de tareas costosas | Pruebas de carga sobre el volumen declarado |
| Escalabilidad funcional | Servicios y documentos definidos por configuración | Incorporar un servicio sin desplegar código |
| Escalabilidad de datos | Modelo relacional indexado | Prueba con volumen proyectado |
| Disponibilidad | Respaldos programados de base de datos y documentos | Prueba de restauración |
| Portabilidad | Entorno contenerizado y reproducible | Levantamiento desde cero en un entorno limpio |
| Mantenibilidad | Módulos delimitados; dependencias externas tras interfaces | Revisión de dependencias en cada Pull Request |

El detalle de cada verificación está en `PLA-001 Plan de pruebas`.

---

## 9. Decisiones de arquitectura registradas

| # | Decisión | Alternativa descartada |
|---|---|---|
| AD-01 | Monolito modular | Microservicios |
| AD-02 | Una sola base de datos para CRM y portal | Bases separadas con sincronización |
| AD-03 | Sesión con cookies httpOnly | Token en almacenamiento del navegador |
| AD-04 | Interfaces para firma y pago | Llamadas directas al proveedor |
| AD-05 | Generación documental y llamadas externas en cola asíncrona | Dentro del ciclo de petición |
| AD-06 | Tipos de trámite definidos por configuración | Desarrollo por cada documento |
| AD-07 | Almacenamiento de documentos separado del ciclo del contenedor | Sistema de archivos efímero |
| AD-08 | Auditoría mediante disparadores de base de datos | Señales de la aplicación |

El contexto y las consecuencias de cada una están en `ARQ-001 §8`.

---

## 10. Secuencia de implementación

El orden no es arbitrario y conviene explicitarlo.

1. **Núcleo de datos y accesos.** Usuarios, roles, clientes, empresas, auditoría.
   Sin esto no hay sobre qué construir.
2. **Servicios contratados y vigencias.** Habilita la detección de vencimientos y
   el dashboard.
3. **Servicio piloto, implementado de forma directa.** Domicilio tributario, casi
   sin abstracción.
4. **Segundo documento, copiando el primero.** Incluso duplicando código.
5. **Extracción del motor** a partir de lo que efectivamente se repite entre
   ambos.
6. **Documentos adicionales, solo por configuración.** Es la validación de que el
   motor funciona.
7. **Integraciones reales**, sustituyendo las implementaciones simuladas.
8. **Migración, pruebas y despliegue.**

Los pasos 3 a 5 invierten el orden intuitivo a propósito. Generalizar antes de
tener dos casos concretos produce abstracciones inventadas, y el riesgo de dedicar
el proyecto a un motor que no genera ningún documento real está registrado en
`RSK-001` como R-14.

---

## 11. Estado del diseño

| Vista | Estado | Bloqueo |
|---|---|---|
| Funcional | Preliminar | Ocho preguntas abiertas en `FUN-001` |
| Comportamiento | Preliminar | Estados y transiciones sin confirmar |
| Lógica | Propuesta | — |
| Datos | Preliminar | Listado definitivo de campos y estructura de la planilla |
| Tecnológica | Propuesta | Alojamiento y proveedor de pago sin definir |
| Seguridad | Propuesta | — |

---

## Control de cambios

| Versión | Fecha | Descripción | Autor |
|---|---|---|---|
| 1.0 | 24-09-2026 | Emisión inicial | Equipo de proyecto |
