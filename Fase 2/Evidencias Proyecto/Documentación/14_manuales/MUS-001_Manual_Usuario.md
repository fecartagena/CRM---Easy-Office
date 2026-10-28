# Manual de usuario: portal del cliente y backoffice

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | MUS-001 · Manual de usuario |
| **Versión** | 0.1 |
| **Fecha** | 27 de octubre de 2026 |
| **Estado** | **Borrador.** Basado en las rutas y permisos del frontend en la rama principal (`f2e4758`). El recorrido con cada rol y las capturas están pendientes |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## 1. Acceso

La plataforma permite al cliente contratar servicios, revisar y firmar sus
documentos y seguir su avance, y al personal de Easy Office gestionar la
información centralizada. La dirección del ambiente de entrega se anota cuando
exista; en desarrollo es `http://localhost:5173`.

| Usuario | Dónde ingresa | Qué puede hacer |
|---|---|---|
| Cliente | `/ingresar` (cuenta obligatoria, RN-41) | Contratar, revisar el borrador, pagar y seguir sus trámites y compras |
| Ejecutivo | `/backoffice/ingresar` | Clientes, trámites, ventas y panel, según sus permisos (RN-32) |
| Administrador | `/backoffice/ingresar` | Todo lo anterior, más la administración (RN-31) |

Este manual describe solo lo que está en la rama principal. La auditoría de
cambios (PR #62) y la importación desde la planilla (`MIG-001`) todavía no están
disponibles.

---

## 2. Portal del cliente

### Registro e ingreso

1. Abrir el catálogo de servicios en la página de inicio.
2. Crear una cuenta en **Registro** (`/registro`) o ingresar con una existente.
3. Las solicitudes, compras y pagos quedan asociados a esa cuenta.

### Contratar domicilio tributario

1. Elegir el servicio y el plan en el catálogo.
2. Completar los datos de la empresa, del representante legal, del contacto, la
   sucursal y la fecha de inicio.
3. Corregir lo que el formulario marque. La dirección y el rol de la sucursal
   salen de la configuración de Easy Office; el cliente no los escribe.
4. Esperar la generación y abrir la **vista previa**. Revisar razón social,
   representante, dirección, período, servicios y montos.
5. Si hay un error, corregir el dato: se genera una versión nueva del borrador.
   No crear una solicitud duplicada para cambiar un dato.
6. **Aprobar el borrador** y pagar (RN-40). En una demostración se usan datos y
   medios de prueba, y el informe indica si fue simulado o sandbox.
7. Seguir el avance en **Mis trámites** (`/mis-tramites`) y las compras en **Mis
   compras** (`/mis-compras`). Un pago pendiente o rechazado se puede reintentar
   desde Mis compras. El pago vale cuando la plataforma lo confirma, no por la
   página de retorno.

Hoy el flujo de domicilio en línea está pensado para empresas (TA-23).

### Bodega

El portal usa un formulario común por tipo de trámite (`/tramites/:tipo/nuevo`).
Para bodega: elegir el plan y los inmuebles, completar el formulario, revisar el
borrador y pagar. Los precios de bodega del catálogo de prueba son provisionales
(`DEC-001` DE-08).

### Firma

Depende del nivel configurado para el tipo de trámite:

- **Asistida** (por defecto): Easy Office gestiona la firma y sube el PDF firmado.
  El cliente ve el trámite en *Pendiente · firma* hasta que eso ocurre.
- **Avanzada**: cada firmante recibe un enlace y un código por correo y firma
  desde su teléfono o computador. **En la demostración se firma contra el PSC de
  pruebas, sin validez legal.**

### Cómo leer el avance

| Estado | Significado |
|---|---|
| Iniciado | La solicitud se registró y está en proceso |
| Pendiente | Falta una etapa. El motivo indica cuál: revisión del borrador, pago, firma o falta de información |
| Completado | Terminó el flujo y el contrato quedó vigente. No significa que se haya hecho un trámite ante el SII |

El estado del trámite, el del pago y la vigencia del contrato son cosas distintas.

---

## 3. Backoffice

La página inicial de `/backoffice` depende de los permisos. El panel operativo lo
ven el Administrador y el Ejecutivo (permiso `core.view_dashboard`, D2-06).

| Pantalla | Ruta | Uso |
|---|---|---|
| Inicio y panel | `/backoffice` | Indicadores calculados desde la base de datos |
| Clientes | `/backoffice/clientes` | Buscar, crear y editar según permisos |
| Ficha del cliente | `/backoffice/clientes/:clienteId` | Datos del cliente y sus relaciones |
| Trámites | `/backoffice/tramites` | Listado de solicitudes |
| Detalle del trámite | `/backoffice/tramites/:tramiteId` | Etapa, documento, firmantes y acciones permitidas |
| Nuevo trámite | `/backoffice/tramites/nuevo/:tipo` | Iniciar un tipo habilitado para el canal interno |
| Ventas | `/backoffice/ventas` | Ventas y pagos con su detalle |
| Tipos de trámite | `/backoffice/tipos-tramite` | Tipos configurados |

### Clientes

Buscar primero por RUT para no duplicar. Al crear o editar, distinguir la empresa
de su representante legal. Lo que cada rol puede hacer lo decide el servidor; este
manual no concede permisos.

### Documentos y firma

- **Asistida:** abrir el trámite, revisar el documento y subir el PDF firmado.
  Eso guarda el archivo en el expediente y completa el trámite. No es una firma
  automática con un proveedor.
- **Avanzada:** el detalle muestra el estado de cada firmante y el identificador
  de la solicitud en el proveedor, con las acciones **reenviar enlace**,
  **cancelar** y **reintentar**. Si un firmante rechaza o la solicitud vence, el
  trámite queda en *falta de información* con el motivo.

### Bodega en el backoffice

Hoy el backoffice gestiona la bodega sin crear la venta en línea del portal: se
trata como pago externo supuesto. Esas solicitudes no cuentan como ingreso cobrado
por la plataforma, y no se debe crear una venta a mano para que aparezcan en el
panel.

---

## 4. Situaciones frecuentes

| Situación | Qué hacer |
|---|---|
| Sesión vencida | Ingresar de nuevo y comprobar si la operación quedó guardada |
| Campo inválido | Revisar el dato y el mensaje; nunca usar datos ajenos para pasar la validación |
| El documento no aparece | Revisar el estado de generación; si persiste, anotar folio y fecha y avisar al responsable técnico |
| Pago pendiente o rechazado | Revisar el detalle en Mis compras y reintentar desde ahí |
| No llegó el enlace de firma | El ejecutivo puede reenviarlo desde el detalle del trámite |
| Acceso denegado | Verificar cuenta y rol con el administrador; no compartir credenciales |
| Error en un documento ya firmado | Pedir revisión interna y conservar el documento; no borrar la historia |

El contacto de soporte y su horario se completan cuando se acuerden.

---

## 5. Pendientes para cerrar este manual

- [ ] Recorrer el portal con una cuenta de cliente y datos sintéticos.
- [ ] Recorrer el backoffice como Administrador y como Ejecutivo, con cuentas
      distintas.
- [ ] Agregar capturas: catálogo, formulario, borrador, pago, Mis trámites,
      detalle interno y firma.
- [ ] Copiar las etiquetas y mensajes exactamente como aparecen.
- [ ] Registrar fecha, commit y revisor.

---

## Historial de versiones

| Versión | Fecha | Cambios |
|---|---|---|
| 0.1 | 27-10-2026 | Versión inicial: acceso, portal (domicilio, bodega, firma y estados), backoffice y situaciones frecuentes |
