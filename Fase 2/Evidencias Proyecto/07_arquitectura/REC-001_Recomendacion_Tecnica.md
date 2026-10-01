# Recomendación técnica

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | REC-001 · Recomendación técnica |
| **Versión** | 1.0 |
| **Fecha** | 24 de septiembre de 2026 |
| **Destinatario** | Easy Office |
| **Responde a** | §4.1 y §10 de las especificaciones del proyecto |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## Propósito

Easy Office no establece una tecnología obligatoria y solicita que el equipo
presente una recomendación antes de la implementación definitiva, considerando
base de datos y arquitectura, alojamiento, seguridad, respaldos, control de
accesos, auditoría, integración mediante API, escalabilidad, costos recurrentes,
mantenibilidad y dependencia de proveedores.

Este documento responde a cada uno de esos criterios.

---

## Resumen de la recomendación

| Componente | Recomendación | Alternativas evaluadas |
|---|---|---|
| Base de datos | PostgreSQL | SQL Server, MySQL, MongoDB |
| Backend | Python con Django y Django REST Framework | Node.js, .NET, Laravel |
| Frontend | React | Vue, Angular, renderizado en servidor |
| Contenedores | Docker y Docker Compose | Instalación directa en servidor |
| Tareas programadas | Celery con Redis | Cron del sistema operativo |
| Alojamiento | Servidor virtual de proveedor con presencia regional | Nube gestionada, hosting compartido |

---

## 1. Base de datos: PostgreSQL

**Por qué relacional y no documental.** La información del encargo está
fuertemente relacionada entre sí: clientes, representantes, servicios
contratados, oficinas, trámites, documentos, plantillas, firmantes, pagos y
auditoría. El §13 exige que la información quede centralizada y trazable, y que
los permisos impidan acciones no autorizadas. Eso requiere integridad referencial
y transacciones: un documento emitido no puede quedar huérfano de su trámite, y
un pago registrado sin su operación asociada sería un error silencioso.

Una base documental como MongoDB resolvería mejor un problema con estructura
variable y pocas relaciones. No es el caso.

**Por qué PostgreSQL entre las relacionales.**

| Criterio | PostgreSQL | SQL Server | MySQL |
|---|---|---|---|
| Licencia | Libre y sin costo, sin límites de uso | Edición Express gratuita, con límite de 10 GB por base de datos y uso acotado de memoria y núcleos; las ediciones sin esos límites son pagadas | Libre en su edición comunitaria |
| Campos semiestructurados | JSONB nativo, indexable y consultable | Soporte JSON más limitado | Soporte JSON más limitado |
| Integridad referencial | Completa | Completa | Completa |
| Disparadores para auditoría | Completos | Completos | Más limitados |
| Disponibilidad en proveedores chilenos | Amplia | Amplia, con costo adicional | Amplia |
| Comunidad y soporte a largo plazo | Muy amplia | Amplia | Muy amplia |

El factor decisivo es el soporte de **JSONB**. El encargo pide en los §5.2, §8.2
y §8.3 que los formularios y las plantillas se adapten a distintos servicios sin
desarrollo nuevo. Eso significa que parte de la información tiene forma variable
según el servicio. PostgreSQL permite guardarla en campos JSONB e igualmente
indexarla y consultarla, sin renunciar a que el resto del modelo sea relacional
estricto. Es lo que permite que el sistema sea configurable sin dejar de ser
consistente.

**Escalabilidad.** El §4.1 pide aumentar considerablemente la cantidad de
clientes y registros sin reemplazar la arquitectura. Con el volumen declarado, de
200 a 250 clientes mensuales y una meta de 50 operaciones diarias, PostgreSQL
opera muy por debajo de su capacidad. El crecimiento se absorbe con índices y, si
alguna vez fuera necesario, con réplicas de lectura, sin cambiar de motor.

**Costo recurrente.** Cero en licencias. El costo es el del servidor donde corre.

---

## 2. Backend: Django con Django REST Framework

**Qué aporta al encargo específicamente.**

- **Control de accesos y roles (§4.2).** Django trae un sistema de usuarios,
  grupos y permisos maduro. El modelo por roles que recomienda el encargo se
  implementa sobre esa base en lugar de construirse desde cero, que es donde
  suelen aparecer los agujeros de seguridad.
- **Seguridad (§4.3).** Protección integrada contra las vulnerabilidades web más
  comunes: inyección SQL, scripting entre sitios, falsificación de peticiones y
  secuestro de sesión.
- **Auditoría (§4.3).** El acceso a la base permite implementar el registro
  mediante disparadores, más difíciles de eludir que el código de aplicación.
- **Panel de administración.** Django genera una interfaz de administración
  funcional sin desarrollo. Es lo que permite que Easy Office configure nuevos
  servicios, tipos de documento y plantillas sin depender de un programador, que
  es el requisito de escalabilidad del §11.
- **API REST.** Django REST Framework es el estándar del ecosistema para exponer
  la API que consumirán el portal de clientes y el CRM.

**Alternativas.** Node.js exige integrar por separado autenticación, permisos y
capa de administración. .NET es sólido pero su ecosistema encarece el
alojamiento. Laravel es comparable a Django en prestaciones; la diferencia es que
el equipo ya trabaja con Python y Django, lo que reduce el riesgo del proyecto.

---

## 3. Frontend: React

El encargo define dos interfaces distintas sobre el mismo sistema: el CRM interno
y el portal de contratación. React permite construir ambas con componentes
compartidos, y es la biblioteca con mayor disponibilidad de desarrolladores en
el mercado chileno, lo que importa para el mantenimiento posterior.

---

## 4. Contenedores: Docker

Permite que el sistema se levante de forma idéntica en el equipo de un
desarrollador y en el servidor de producción, elimina la dependencia de la
configuración específica de una máquina, y hace que el despliegue sea portable
entre proveedores de alojamiento. Esto último es relevante porque el proveedor
todavía no está definido: contenerizar hoy evita rehacer trabajo después.

---

## 5. Alojamiento y disponibilidad

**Recomendación:** un servidor virtual de un proveedor con presencia o latencia
adecuada para Chile, con la aplicación, la base de datos y el almacenamiento de
documentos contenerizados.

**Por qué no una nube gestionada completa.** Servicios como bases de datos
administradas o almacenamiento de objetos gestionado reducen el trabajo de
operación, pero elevan el costo recurrente y aumentan la dependencia del
proveedor. Para el volumen de Easy Office no se justifican todavía.

**Por qué no hosting compartido.** No permite contenedores, ni tareas programadas,
ni control sobre la base de datos, ni certificados propios.

`[PENDIENTE DE DEFINIR CON EASY OFFICE: proveedor, presupuesto mensual disponible
y quién administrará el servidor después de la entrega.]`

---

## 6. Seguridad y protección de datos

El sistema almacenará RUT, domicilios, antecedentes tributarios y contratos de
clientes reales. Medidas recomendadas:

- Acceso individual con usuario y contraseña, sin cuentas compartidas.
- Permisos por rol, con la restricción del §4.2 de que los ejecutivos no eliminan
  registros.
- HTTPS obligatorio en todo el tráfico.
- Credenciales y claves fuera del código, en variables de entorno.
- Cifrado en reposo de los documentos almacenados.
- Registro de accesos y de acciones relevantes.
- Minimización: solicitar únicamente los campos que cada servicio requiere.
- Datos anonimizados en los entornos de desarrollo y prueba.

**Marco regulatorio.** La Ley 21.719 sobre protección de datos personales entra
en plena vigencia el 1 de diciembre de 2026. Introduce obligaciones sobre el
tratamiento de datos personales, incluidos el registro de actividades de
tratamiento, la respuesta a los derechos de los titulares y la notificación de
incidentes de seguridad. Las medidas anteriores son coherentes con ese marco.

Este documento no constituye asesoría legal ni una certificación de cumplimiento.
Se recomienda que Easy Office revise sus obligaciones con asesoría especializada.

---

## 7. Respaldos y recuperación

| Elemento | Frecuencia recomendada | Retención |
|---|---|---|
| Base de datos completa | Diaria | 30 días |
| Documentos emitidos | Diaria, incremental | Permanente |
| Configuración del sistema | Ante cada cambio | Versionada |

Los respaldos deben almacenarse **fuera del servidor de producción**: un respaldo
en el mismo disco no protege ante la falla que más importa. Y deben probarse
periódicamente restaurándolos en un entorno de prueba, porque un respaldo que
nunca se restauró es una suposición, no una garantía.

---

## 8. Integración mediante API

La plataforma expondrá una API REST propia, que es lo que permite que el portal
de contratación y el CRM compartan la misma información sin redigitación (§9).

Las integraciones con terceros, firma electrónica y pasarela de pago, se
implementan detrás de interfaces definidas por el sistema. Esto significa que
cambiar de proveedor de pago o de plataforma de firma afecta a un solo componente
y no al resto del sistema, que es lo que el §11 pide al mencionar incorporar
nuevas formas de pago e integrar nuevas plataformas de firma.

**Pendiente:** la documentación técnica y la API de la plataforma de firma
electrónica, que el §14 compromete "si está disponible". Es la dependencia
externa más relevante del proyecto y conviene resolverla cuanto antes.

---

## 9. Pasarela de pago

El §5.3 pide recomendar la solución de pago considerando seguridad, comisiones,
compatibilidad con medios de pago nacionales, facilidad de integración y
confirmación automática.

| Criterio | A evaluar |
|---|---|
| Medios de pago nacionales | Cobertura de tarjetas de débito y crédito locales |
| Comisiones | Porcentaje por transacción y costos fijos |
| Confirmación automática | Notificación de resultado hacia el sistema |
| Ambiente de pruebas | Disponibilidad para desarrollar sin cobros reales |
| Requisitos de contratación | Documentación y plazos para habilitar la cuenta |

La recomendación definitiva requiere información que solo Easy Office maneja: el
volumen esperado de transacciones y las condiciones comerciales que pueda
negociar. Para el desarrollo se trabajará contra una interfaz desacoplada, de
modo que la decisión pueda tomarse más adelante sin afectar el avance.

`[PENDIENTE: decisión de Easy Office sobre el proveedor.]`

---

## 10. Costos recurrentes estimados

| Concepto | Costo |
|---|---|
| Licencias de software | Sin costo. Todo el stack recomendado es de código abierto |
| Servidor de aplicación | Según proveedor y plan `[POR DEFINIR]` |
| Dominio | Anual, bajo |
| Certificado HTTPS | Sin costo con certificados automáticos |
| Almacenamiento de respaldos | Bajo, proporcional al volumen |
| Comisiones de la pasarela de pago | Por transacción, según proveedor |
| Firma electrónica | Según el contrato vigente de Easy Office con su proveedor |

La ausencia de licencias es una decisión deliberada: mantiene el costo operativo
del sistema acotado al alojamiento y evita que Easy Office quede sujeta a
renovaciones de licencia para seguir usando su propia plataforma.

---

## 11. Mantenibilidad y dependencia de proveedores

**Mantenibilidad.** El sistema se organiza en módulos con responsabilidades
delimitadas. La configuración de servicios, tipos de documento, plantillas,
estados y roles se administra como datos desde el panel de administración, no
como código. Esto significa que incorporar un servicio o un documento nuevo, que
es lo que el §11 pide, no requiere necesariamente a un desarrollador.

**Dependencia de proveedores.** El stack recomendado es de código abierto y
ampliamente soportado: no hay un proveedor que pueda discontinuar el producto ni
elevar unilateralmente su costo. Las únicas dependencias externas reales son la
plataforma de firma electrónica y la pasarela de pago, ambas aisladas detrás de
interfaces precisamente para que puedan reemplazarse.

**Continuidad.** Python, Django, React y PostgreSQL son tecnologías con gran
disponibilidad de profesionales en Chile. Easy Office podrá contratar
mantenimiento sin quedar atada al equipo original.

---

## 12. Qué se necesita de Easy Office para cerrar esta recomendación

1. Presupuesto mensual disponible para infraestructura.
2. Quién administrará el servidor después de la entrega.
3. Documentación técnica y API de la plataforma de firma electrónica.
4. Decisión sobre la pasarela de pago, o las condiciones comerciales de las
   opciones que estén evaluando.
5. Volumen esperado de transacciones en línea durante el primer año.
