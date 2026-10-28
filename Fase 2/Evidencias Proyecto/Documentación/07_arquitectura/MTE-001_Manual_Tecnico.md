# Manual técnico: instalación, configuración y verificación

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | MTE-001 · Manual técnico |
| **Versión** | 0.1 |
| **Fecha** | 27 de octubre de 2026 |
| **Estado** | **Borrador.** Comandos tomados de los README y archivos de la rama principal (backend `7869d0a`, frontend `f2e4758`). La instalación limpia siguiendo este manual **no se ha ejecutado** todavía |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

El despliegue, los respaldos y la recuperación están en `14_manuales/MOP-001`.
Este manual cubre el ambiente local.

---

## 1. Componentes

El backend es un monolito modular Django 6 con Django REST Framework y
PostgreSQL 17. El frontend es una aplicación React con TypeScript que consume la
API y separa el portal del cliente del backoffice. Celery y Redis ejecutan el
trabajo asíncrono; Gotenberg convierte los documentos Word a PDF. La arquitectura
completa está en `ARQ-001`.

| Área | Código fuente |
|---|---|
| Clientes, empresas, personas y representantes | `crm_easyoffice/clientes/` |
| Inmuebles, sucursales y autorizantes | `crm_easyoffice/inmuebles/` |
| Contratos, servicios contratados, folio y vigencia | `crm_easyoffice/contratos/` |
| Tipos de trámite, solicitudes, estados y firmantes | `crm_easyoffice/tramites/` |
| Plantillas, conversión, archivos y generación | `crm_easyoffice/documentos/` |
| Pagos y firma (adaptadores de proveedores) | `crm_easyoffice/integrations/` |
| PSC de pruebas, sin validez legal (`ARQ-001` AD-16) | `firma_sandbox/` |
| Diferencias entre plantillas | `docs/variation-points.md` |
| Rutas y permisos del frontend | `src/routes/routes.tsx`, `src/features/auth/permisos.ts` |

**Modos de las integraciones.** Pago: simulado, Klap sandbox o Flow sandbox.
Firma: asistida (un ejecutivo sube el PDF firmado) o avanzada contra el PSC de
pruebas. No hay integración con un prestador acreditado: depende de las
credenciales de tufirma.digital (TA-07).

En cada instalación se registran los commits de ambos repositorios, las
versiones de Docker, Compose y Node, la fecha y el sistema operativo. Las
dependencias exactas están en `uv.lock` y `package-lock.json`.

---

## 2. Backend

Requisitos: Git, Docker y Docker Compose. `just` es opcional; abrevia los
comandos de `docker compose`.

```bash
git clone https://github.com/Nicozapata1811/crm-easyoffice-backend.git
cd crm-easyoffice-backend
cp -R .envs.example .envs
docker compose -f docker-compose.local.yml build
docker compose -f docker-compose.local.yml up -d
docker compose -f docker-compose.local.yml ps
```

Copiar `.envs.example` es obligatorio: sin `.envs/` PostgreSQL no arranca. Los
valores de `.local/` sirven tal cual en desarrollo; los de `.production/` son
plantillas con `CHANGEME`. Las migraciones se aplican al iniciar.

`docker-compose.local.yml` levanta once servicios: `django`, `postgres`, `redis`,
`celeryworker`, `celerybeat`, `flower`, `mailpit`, `gotenberg`, `tunnel` (túnel
temporal para los retornos de pago), `firma-sandbox` y `firma-sandbox-worker`.
El README del backend todavía menciona siete.

Datos de demostración, solo con `DEBUG` activo y siempre ficticios:

```bash
docker compose -f docker-compose.local.yml run --rm django python manage.py createsuperuser
docker compose -f docker-compose.local.yml run --rm django python manage.py seed_demo_clientes
docker compose -f docker-compose.local.yml run --rm django python manage.py seed_demo_domicilio
docker compose -f docker-compose.local.yml run --rm django python manage.py seed_demo_bodega
```

Para probar por rol se crean usuarios separados: un Administrador, un Ejecutivo
y dos clientes del portal. El superusuario no sirve como evidencia del acceso de
cada rol.

| Recurso | Dirección |
|---|---|
| API y aplicación | http://localhost:8000 |
| Panel de administración | http://localhost:8000/admin/ |
| Documentación de la API (requiere sesión de administrador) | http://localhost:8000/api/docs/ |
| Correo de prueba (Mailpit) | http://localhost:8025 |
| Monitor de Celery (Flower) | http://localhost:5555 |
| PSC de pruebas: páginas de firma y verificación | http://localhost:8100 |

### Firma avanzada con el PSC de pruebas

```bash
cp .envs.example/.local/.firma .envs/.local/.firma   # reemplazar cada CHANGEME
docker compose -f docker-compose.local.yml up -d
docker compose -f docker-compose.local.yml run --rm django python manage.py seed_demo_firma_avanzada
```

`seed_demo_firma_avanzada` pasa los tipos de trámite a firma avanzada y
`--asistida` los devuelve. Los enlaces y códigos de cada firmante llegan a
Mailpit. El recorrido completo se automatiza con `just e2e-firma`. Los PDF
firmados por este PSC **no tienen validez legal**.

---

## 3. Frontend

Requisitos: Node.js 22.12 o superior, o 20.19 o superior, y el backend
funcionando.

```bash
git clone https://github.com/Nicozapata1811/crm-easyoffice-frontend.git
cd crm-easyoffice-frontend
npm ci
cp .env.example .env
npm run dev
```

`npm ci` instala exactamente lo del lockfile. Si falla, se registra el error; no
se regenera el lockfile para ocultarlo.

La aplicación abre en http://localhost:5173. `VITE_API_BASE_URL` vale `/api` y
`VITE_BACKEND_ORIGIN` apunta el proxy de Vite al backend. Las variables `VITE_`
quedan dentro del código que recibe el navegador: no llevan secretos.

| Entrada | Ruta |
|---|---|
| Catálogo (inicio del portal) | `/` |
| Ingreso de clientes | `/ingresar` |
| Ingreso del personal | `/backoffice/ingresar` |

---

## 4. Configuración por ambiente

| Grupo | Variables o archivos | Regla |
|---|---|---|
| Base de datos | `.envs/.local/.postgres`, `.envs/.production/.postgres` (`POSTGRES_*`) | Usuario y clave propios de cada ambiente |
| Aplicación | `.envs/…/.django`, `DJANGO_ALLOWED_HOSTS`, orígenes y cookies | Documentar nombres y dominio, nunca los valores secretos |
| Sesión | `DJANGO_SESSION_INACTIVITY_SECONDS` | Por defecto 1.800 segundos; probar el vencimiento |
| Tareas | `REDIS_URL`, servicios `celeryworker` y `celerybeat` | Verificar que las tareas periódicas corren |
| PDF | `PDF_CONVERTER`, `GOTENBERG_URL`, `PDF_CONVERSION_TIMEOUT_SECONDS` | Cada evidencia indica si usó el conversor simulado o Gotenberg |
| Pago | `PAYMENT_PROVIDER`, `PAYMENT_PROVIDERS`, `KLAP_*`, `FLOW_*`, `PAYMENT_WEBHOOK_BASE_URL`, `PORTAL_BASE_URL` | Solo claves de sandbox; registrar el modo, no las claves |
| Firma | `.envs/.local/.firma` | Secretos compartidos con el PSC de pruebas; nunca en producción |
| Archivos privados | `DJANGO_PRIVATE_MEDIA_ROOT` | Fuera de la ruta pública; se respaldan y su acceso se controla |
| Folio | `CONTRATO_FOLIO_ANCHO` | Por defecto 4 dígitos |

---

## 5. Verificación

Backend:

```bash
docker compose -f docker-compose.local.yml run --rm django python manage.py check
docker compose -f docker-compose.local.yml run --rm django python manage.py makemigrations --check
docker compose -f docker-compose.local.yml run --rm django pytest
just firma-pytest
just e2e-firma
```

Frontend:

```bash
npm test
npm run typecheck
npm run lint
npm run build
```

**Cifras de referencia, no ejecutadas para este manual:** `PLA-001` v1.11
registra 685 pruebas del backend en la rama principal y 62 del PSC de pruebas.
Las 696 del backend corresponden a la rama de auditoría (PR #62), sin fusionar.
Cada ejecución se registra con comando, salida, código de retorno y commit
(`12_evidencias/README.md`).

Después de las pruebas: ingresar con cada rol, consultar clientes, crear una
solicitud sintética, esperar el PDF y recorrer el pago y la firma en el modo
elegido. La instalación queda aceptada cuando un integrante distinto del autor
la repite siguiendo solo este manual y anota cada desviación.

---

## 6. Diagnóstico

| Síntoma | Qué revisar |
|---|---|
| PostgreSQL no arranca | Que exista `.envs/`, las variables requeridas y el log del servicio |
| El portal no conecta | Backend activo, `VITE_BACKEND_ORIGIN`, orígenes y cookies |
| Sesión o CSRF rechazados | Cookie de sesión, token CSRF y origen. No se resuelve desactivando la protección |
| El PDF no aparece | `celeryworker`, Redis y Gotenberg; estado del documento y error de la tarea |
| El pago no avanza | Modo configurado, URL del túnel y estado de la venta en el backend |
| La firma no avanza | `firma-sandbox` y `firma-sandbox-worker`, Mailpit y el estado de cada firmante en el detalle del trámite |
| Acceso denegado | Rol y pertenencia al cliente; un 403 puede ser lo esperado |

```bash
docker compose -f docker-compose.local.yml logs --tail=100 django celeryworker gotenberg firma-sandbox
```

Antes de adjuntar un log se quitan claves, cookies, tokens y datos personales.

---

## 7. Pendientes para cerrar este manual

- [ ] Instalación limpia por un integrante distinto del autor, con versiones y
      desviaciones.
- [ ] Generación de `PLT-01` y `PLT-02` con datos sintéticos.
- [ ] Resultados de las pruebas y del recorrido por rol, en `12_evidencias/`.
- [ ] Corregir en el README del backend el conteo de servicios y de pruebas.

---

## Historial de versiones

| Versión | Fecha | Cambios |
|---|---|---|
| 0.1 | 27-10-2026 | Versión inicial: componentes, instalación del backend y del frontend, firma con el PSC de pruebas, configuración, verificación y diagnóstico |
