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
| Frontend | React 19 · TypeScript · Material UI · Vite · Vitest |
| Backend | Python 3.14 · Django 6 · Django REST Framework |
| Base de datos | PostgreSQL 17 |
| Procesamiento asíncrono | Celery · Redis |
| Generación documental | docxtpl (plantillas Word) · Gotenberg con LibreOffice (Word a PDF) |
| API | REST, con esquema OpenAPI |
| Contenedores | Docker · Docker Compose |
| Control de versiones | Git · GitHub |
| Alojamiento | Por definir con la contraparte |

La justificación de cada elección, frente a los criterios de base de datos,
alojamiento, seguridad, respaldos, control de accesos, auditoría, API,
escalabilidad, costos recurrentes, mantenibilidad y dependencia de proveedores,
está en `REC-001 Recomendación técnica`.

**Integraciones externas.** Firma electrónica avanzada y pasarela de pago se
consumen a través de interfaces definidas por el sistema, con implementaciones
intercambiables. El proveedor de firma es el que utiliza Easy Office; mientras no
se reciban sus credenciales, la firma es asistida: un ejecutivo sube el PDF
firmado y el sistema registra su huella SHA-256. En pagos están integrados Klap y
Flow, y el cliente elige entre los medios habilitados; la elección de cuáles
ofrecer en producción está pendiente de la contraparte. La conversión de Word a
PDF corre en un contenedor aparte (Gotenberg), alcanzable solo desde el worker.

---

## Ejecución local

### Requisitos previos

- Docker y Docker Compose
- Git
- Node.js 22.12 o superior, solo si el frontend se levanta sin Docker

No se requiere instalar Python ni PostgreSQL en el equipo: el backend corre en
contenedores.

### Backend

```bash
git clone git@github.com:Nicozapata1811/crm-easyoffice-backend.git
cd crm-easyoffice-backend
cp -R .envs.example .envs
docker compose -f docker-compose.local.yml build
docker compose -f docker-compose.local.yml up -d

# usuario administrador
docker compose -f docker-compose.local.yml run --rm django python manage.py createsuperuser

# datos ficticios para desarrollo (solo con DEBUG activo)
docker compose -f docker-compose.local.yml run --rm django python manage.py seed_demo_clientes
docker compose -f docker-compose.local.yml run --rm django python manage.py seed_demo_bodega

# pruebas
docker compose -f docker-compose.local.yml run --rm django pytest
```

Levanta Django, PostgreSQL, Redis, el worker y el planificador de Celery, Flower,
Mailpit, Gotenberg y el túnel para webhooks de pago. Las migraciones se aplican al
iniciar.

| Servicio | URL |
|---|---|
| Aplicación y API | `http://localhost:8000` |
| Panel de administración | `http://localhost:8000/admin/` |
| Documentación de la API | `http://localhost:8000/api/docs/` |
| Correo de prueba (Mailpit) | `http://localhost:8025` |
| Monitor de Celery (Flower) | `http://localhost:5555` |

### Frontend

```bash
git clone git@github.com:Nicozapata1811/crm-easyoffice-frontend.git
cd crm-easyoffice-frontend
npm install
cp .env.example .env
npm run dev          # http://localhost:5173
npm test             # pruebas
```

También puede levantarse con `docker compose -f docker-compose.local.yml up`.

### Variables de entorno

Cada repositorio documenta sus variables en su propio README: `.envs.example/` en
el backend y `.env.example` en el frontend. Las principales del backend son las
de Django y la base de datos, los medios de pago habilitados
(`PAYMENT_PROVIDERS`), las credenciales de Klap y Flow, y la conversión a PDF
(`PDF_CONVERTER`, `GOTENBERG_URL`).

**Ninguna credencial se versiona.** Los archivos de entorno reales están
excluidos del control de versiones. Los ejemplos incluidos contienen solo valores
de desarrollo local o marcadores `CHANGEME`.

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
| Planificación macro | Carta Gantt de 18 semanas con diez hitos (`GNT-001`) |

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
                                       ├─→  Celery + Redis   (documentos, pagos, vencimientos)
                                       │       └─→  Gotenberg (Word a PDF)
                                       ├─→  Almacenamiento de documentos
                                       ├─→  SignatureService  ─→  Proveedor de firma
                                       └─→  PaymentService    ─→  Klap · Flow
```

### Módulos

| Módulo | Responsabilidad |
|---|---|
| `users` · `core` | Usuarios, roles (Administrador y Ejecutivo) y permisos. La auditoría se agrega aquí (HU-30, pendiente) |
| `clientes` | Personas, empresas, representación legal y cuentas de cliente del portal |
| `configuracion` | Datos de Easy Office, marcas y cuentas donde recibe pagos |
| `inmuebles` | Oficinas y bodegas, rol de avalúo y autorizantes |
| `contratos` | Contratos con folio, servicios contratados, vigencias y vencimientos |
| `tramites` | Tipos de trámite configurables, trámites, estados y motivos de pendiente |
| `documentos` | Plantillas versionadas, generación Word y PDF, firmantes, integridad |
| `ventas` | Catálogo de servicios y planes, ventas, órdenes de pago y pagos |
| `panel` | Indicadores del dashboard y exportación a Excel |
| `integrations` | Interfaces e implementaciones de firma, pago (Klap y Flow) y conversión a PDF (Gotenberg) |
| `migracion` | Carga de información desde planillas (a la espera de la planilla anonimizada) |

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

El detalle está en `ARQ-001 Arquitectura` (componentes UML y secuencia del
trámite), `UML-002 Diagrama de clases` y `DIS-001 Documento de diseño`.

---

## Estructura del repositorio

```
CRM---Easy-Office/
├── Fase 1/          Definición del proyecto
├── Fase 2/          Desarrollo
│   └── Evidencias Proyecto/
│       └── Documentación/   Documentación del proyecto (índice en su README.md)
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
| ARQ-001 | Arquitectura: componentes, comunicación, secuencia y despliegue |
| MOD-001 | Modelo de datos |
| UML-001 | Casos de uso |
| UML-002 | Diagrama de clases |
| REC-001 | Recomendación técnica |
| PLA-001 | Plan de pruebas |
| RSK-001 | Registro de riesgos |
| ACTA-001 | Acta de levantamiento |
| ACTA-002 | Reunión de validación con Easy Office, 08-10-2026 |
| CHK-001 | Checklist de validación con la contraparte |
| PLT-000 | Catálogo de plantillas, diccionario de campos y regla del folio (`13_plantillas`) |
| PLT-01 a PLT-05 | Plantillas documentales anonimizadas (`13_plantillas`) |
| GNT-001 | Carta Gantt, hitos y calendario de sprints |

El índice con versiones y orden de lectura está en
`Fase 2/Evidencias Proyecto/Documentación/README.md`.

---

## Protección de datos

Este repositorio es público. **No se versiona ningún dato personal real ni
credencial**, en ningún formato: fixtures, pruebas, capturas, comentarios o
migraciones. Los entornos de desarrollo operan con datos ficticios, y las
plantillas de Easy Office se versionan solo anonimizadas (`PLT-01` a `PLT-05`).

El sistema trata datos personales de clientes de Easy Office. El diseño incorpora
control de acceso por rol, minimización de campos, cifrado de documentos en reposo
y trazabilidad de las acciones, en línea con la Ley 21.719 sobre protección de
datos personales.
