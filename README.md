# CRM Easy Office

Plataforma web de gestión y automatización documental para Easy Office.

---

## Descripción

### Qué hace

CRM Easy Office centraliza la gestión de clientes, servicios y documentos de Easy
Office, y automatiza la contratación, generación y firma de los servicios
estandarizables.

La plataforma tiene dos áreas sobre una misma base de información:

- **CRM interno.** Registro y consulta de clientes, gestión de servicios
  contratados con sus vigencias, alertas de vencimiento, indicadores de operación
  y registro de auditoría inalterable.
- **Portal de contratación.** El cliente selecciona un servicio, ingresa sus
  datos, revisa el documento, paga, firma y lo descarga, sin intervención de un
  ejecutivo.

Ambas áreas comparten el mismo dominio, de modo que la información que ingresa el
cliente no se vuelve a digitar internamente.

### A quién está dirigido

| Usuario | Uso |
|---|---|
| Administradores de Easy Office | Configuración del sistema, usuarios, permisos, auditoría e indicadores |
| Ejecutivos de Easy Office | Gestión diaria de clientes, servicios y trámites |
| Clientes de Easy Office | Contratación en línea de servicios estandarizados |

### Qué problema resuelve

Easy Office opera manualmente: el cliente envía sus antecedentes por mensajería,
un ejecutivo los transcribe a una plantilla, genera el documento, lo devuelve como
borrador, espera confirmación y firma en lote. El proceso toma entre tres y cuatro
horas por servicio, y ese tiempo es espera entre pasos, no trabajo efectivo.

Eso limita la operación a tres a cinco clientes diarios sobre una cartera mensual
de 200 a 250. La información de clientes vive en planillas donde todos acceden a
todo y no queda registro de quién modifica qué.

La plataforma ataca las tres consecuencias: la capacidad operativa, la
transcripción manual y sus errores, y la ausencia de trazabilidad.

---

## Tecnologías utilizadas

| Capa | Tecnología |
|---|---|
| Frontend | React |
| Backend | Python · Django · Django REST Framework |
| Base de datos | PostgreSQL |
| Procesamiento asíncrono | Celery · Redis |
| API | REST |
| Contenedores | Docker · Docker Compose |
| Control de versiones | Git · GitHub |
| Alojamiento | Por definir con la contraparte |

La justificación de cada elección, frente a los criterios de base de datos,
alojamiento, seguridad, respaldos, control de accesos, auditoría, API,
escalabilidad, costos recurrentes, mantenibilidad y dependencia de proveedores,
está en `REC-001 Recomendación técnica`.

**Integraciones externas.** Firma electrónica avanzada y pasarela de pago se
consumen a través de interfaces definidas por el sistema, con implementaciones
intercambiables. El proveedor de firma es el que utiliza Easy Office; el de pago
está pendiente de decisión de la contraparte.

---

## Ejecución local

### Requisitos previos

- Docker y Docker Compose
- Git

No se requiere instalar Python, Node ni PostgreSQL en el equipo: todo corre en
contenedores.

### Pasos

```bash
# 1. Clonar el repositorio de backend
git clone https://github.com/<organizacion>/crm-easyoffice-backend.git
cd crm-easyoffice-backend

# 2. Preparar las variables de entorno
cp .envs/.local/.django.example .envs/.local/.django
cp .envs/.local/.postgres.example .envs/.local/.postgres
# editar ambos archivos con los valores locales

# 3. Levantar el entorno
docker compose -f docker-compose.local.yml up --build

# 4. Aplicar las migraciones
docker compose -f docker-compose.local.yml run --rm django python manage.py migrate

# 5. Crear un usuario administrador
docker compose -f docker-compose.local.yml run --rm django python manage.py createsuperuser
```

La aplicación queda disponible en `http://localhost:8000` y el panel de
administración en `http://localhost:8000/admin`.

### Variables de entorno

| Variable | Descripción |
|---|---|
| `DJANGO_SECRET_KEY` | Clave de firma de la aplicación |
| `DJANGO_DEBUG` | Modo depuración. `False` en cualquier entorno distinto del local |
| `DJANGO_ALLOWED_HOSTS` | Dominios autorizados |
| `DATABASE_URL` | Cadena de conexión a PostgreSQL |
| `CELERY_BROKER_URL` | Conexión a Redis |
| `SIGNATURE_PROVIDER` | Implementación de firma a utilizar: `simulado` o el proveedor real |
| `SIGNATURE_API_KEY` | Credencial del proveedor de firma |
| `PAYMENT_PROVIDER` | Implementación de pago a utilizar |
| `PAYMENT_API_KEY` | Credencial del proveedor de pago |
| `DOCUMENT_STORAGE_PATH` | Ubicación del almacenamiento de documentos |

**Ninguna credencial se versiona.** Los archivos de entorno están excluidos del
control de versiones. Los ejemplos incluidos contienen solo nombres de variables y
valores de marcador.

### Frontend

```bash
git clone https://github.com/<organizacion>/crm-easyoffice-frontend.git
cd crm-easyoffice-frontend
cp .env.example .env    # definir la URL base de la API
docker compose up --build
```

> **Estado.** Los repositorios de código están en preparación. Esta sección se
> actualiza con los comandos verificados al completarse el entorno.

---

## Integrantes y roles

**Fernando Esteban Cartagena Acuña**
Coordinación del proyecto, ingeniería de requisitos, arquitectura e integraciones.

**Nicolás Eduardo Zapata Trujillo**
Base de datos, desarrollo backend e integraciones.

**Marcos José Álvarez Muñoz**
Desarrollo frontend, experiencia de usuario y prototipos.

Los roles representan responsabilidades principales, no funciones exclusivas.
Todo el código entra por Pull Request revisado por otro integrante.

---

## Metodología de trabajo

Metodología ágil basada en **Scrum**, adaptada a un equipo de tres integrantes.

| Aspecto | Definición |
|---|---|
| Sprints | Dos semanas |
| Product Owner | Fernando Cartagena, responsable de la relación con la contraparte |
| Scrum Master | Marcos Álvarez |
| Equipo de desarrollo | Los tres integrantes |
| Ceremonias | Sprint Planning, seguimiento asíncrono tres veces por semana, Sprint Review con demostración y retrospectiva al cierre |
| Tablero | GitHub Projects |
| Planificación macro | Carta Gantt de 18 semanas con diez hitos |

### Artefactos

Product Vision · Product Backlog priorizado · Sprint Backlog por iteración ·
Definition of Done · documento de diseño · registro de retrospectivas · plan de
pruebas con evidencias por sprint · manual técnico y de despliegue.

### Trazabilidad

Cada historia de usuario tiene su issue, los commits referencian ese issue y las
pruebas se asocian a la historia que verifican. La cadena completa es:

```
requerimiento → historia de usuario → issue → commit / PR → prueba → evidencia
```

Se mantiene en `TRZ-001 Matriz de trazabilidad`.

---

## Arquitectura de la solución

**Estilo: monolito modular.** Una aplicación, una base de datos, un despliegue, con
módulos de fronteras explícitas por dentro.

```
Portal cliente (React)  ─┐
                         ├─→  API REST (Django + DRF)  ─→  PostgreSQL
Backoffice CRM (React)  ─┘             │
                                       ├─→  Celery + Redis   (documentos, webhooks, vencimientos)
                                       ├─→  Almacenamiento de documentos
                                       ├─→  SignatureService  ─→  Proveedor de firma
                                       └─→  PaymentService    ─→  Pasarela de pago
```

### Módulos

| Módulo | Responsabilidad |
|---|---|
| `core` | Usuarios, roles, permisos, auditoría |
| `clientes` | Personas, empresas, representación legal |
| `servicios` | Servicios contratados, vigencias, alertas de vencimiento |
| `inmuebles` | Oficinas, rol de avalúo, domicilios asignados |
| `tramites` | Tipos de trámite configurables, trámites, máquina de estados |
| `documentos` | Plantillas versionadas, generación, firmantes, integridad |
| `integrations` | Interfaces e implementaciones de firma y pago |
| `migracion` | Carga de información desde planillas |

### Decisiones de diseño

- **Un solo dominio para CRM y portal.** Sincronizar dos bases introduciría el
  problema de redigitación que el proyecto busca eliminar.
- **Configuración como dato.** Servicios, formularios, plantillas, estados y roles
  se administran como registros, no como código.
- **Documentos inmutables.** Cada documento emitido queda asociado a la versión de
  plantilla usada y a una copia congelada de sus datos.
- **Dependencias externas tras interfaces.** Cambiar de proveedor de firma o de
  pago afecta a un solo componente.
- **Auditoría de solo escritura.** No existe funcionalidad de modificación ni
  eliminación del historial.

El detalle está en `ARQ-001 Arquitectura` y `DIS-001 Documento de diseño`.

---

## Estructura del repositorio

```
CRM---Easy-Office/
├── Fase 1/          Definición del proyecto
├── Fase 2/          Desarrollo
│   └── Evidencias Proyecto/
│       └── Documentación/   Documentos técnicos del proyecto
├── Fase 3/          Cierre
└── README.md
```

Los repositorios de código se mantienen separados:

- `crm-easyoffice-backend`
- `crm-easyoffice-frontend`

---

## Documentación

| Código | Documento |
|---|---|
| DIP-001 | Documento de inicio de proyecto |
| PV-001 | Product Vision |
| ALC-001 | Alcance del MVP |
| FUN-001 | Ficha funcional del servicio piloto |
| MRQ-001 | Matriz de requerimientos |
| RN-001 | Reglas de negocio |
| CAT-001 | Catálogo de identificadores |
| TRZ-001 | Matriz de trazabilidad |
| BKL-001 | Product Backlog |
| SPR-001 | Sprint Backlog |
| DOD-001 | Definition of Done |
| RET-001 | Registro de retrospectivas |
| DIS-001 | Documento de diseño |
| ARQ-001 | Arquitectura |
| MOD-001 | Modelo de datos |
| UML-001 | Casos de uso |
| REC-001 | Recomendación técnica |
| PLA-001 | Plan de pruebas |
| RSK-001 | Registro de riesgos |
| ACTA-001 | Acta de levantamiento |

---

## Protección de datos

Este repositorio es público. **No se versiona ningún dato personal real ni
credencial**, en ningún formato: fixtures, pruebas, capturas, comentarios o
migraciones. Los entornos de desarrollo operan con datos anonimizados.

El sistema trata datos personales de clientes de Easy Office. El diseño incorpora
control de acceso por rol, minimización de campos, cifrado de documentos en reposo
y trazabilidad de las acciones, en línea con la Ley 21.719 sobre protección de
datos personales.
