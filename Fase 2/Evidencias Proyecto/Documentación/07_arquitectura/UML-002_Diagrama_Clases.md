# Diagrama de clases

| | |
|---|---|
| **Cliente** | Easy Office |
| **Proyecto** | CRM Easy Office · Plataforma de gestión y automatización documental |
| **Documento** | UML-002 · Diagrama de clases |
| **Versión** | 1.1 |
| **Fecha** | 22 de octubre de 2026 |
| **Estado** | Refleja el código de la rama principal del backend al 22-10-2026 (después del PR #60) |
| **Preparado por** | Fernando Cartagena · Nicolás Zapata · Marcos Álvarez |

---

## 1. Alcance y notación

Este diagrama muestra las clases **implementadas** en el backend
(`crm_easyoffice/*/models.py` e `integrations/`), no el modelo conceptual. El
modelo conceptual, con las entidades previstas que aún no existen (alertas de
vencimiento, migración), está en `MOD-001`. La auditoría (sección 5 bis) está
en una rama sin fusionar.

- Cada clase es un modelo de Django y una tabla de PostgreSQL. Salvo
  `CorrelativoFolio`, `ServicioContratado`, `Firmante`, `ItemVenta`,
  `EventoPago` y `EventoTramite`, todas heredan de `TimestampedModel`
  (`created_at`, `updated_at`), una clase abstracta del módulo `core`. La
  herencia no se dibuja para no cruzar todos los diagramas con la misma flecha.
- Se muestran los atributos de negocio. Los identificadores (`id`) y las claves
  foráneas se omiten como atributos: aparecen como asociaciones.
- Multiplicidad: `1` obligatorio, `0..1` opcional, `*` varios.
- La lógica de negocio vive en funciones de servicio por módulo
  (`services.py`), no en métodos de los modelos; la sección 7 lista las
  principales.
- Los estados son enumeraciones (`TextChoices` de Django).

---

## 2. Vista general

Las clases agrupadas por módulo y sus asociaciones principales.

```mermaid
classDiagram
    direction LR
    User "1" -- "0..1" CuentaCliente : accede al portal
    CuentaCliente "*" --> "1" Cliente
    Cliente "1" --> "0..1" Persona : persona natural
    Cliente "1" --> "0..1" Empresa : persona jurídica
    RepresentanteLegal "*" --> "1" Persona
    RepresentanteLegal "*" --> "1" Empresa

    Contrato "*" --> "1" Cliente
    Contrato "*" --> "1" Marca
    Contrato "1" *-- "1..*" ServicioContratado : líneas
    ServicioContratado "*" --> "1" Servicio
    ServicioContratado "*" --> "0..1" Oficina : inmueble

    Tramite "*" --> "1" Cliente
    Tramite "*" --> "1" TipoTramite
    Tramite "*" --> "0..1" Contrato
    Tramite "*" --> "0..1" Venta
    Tramite "1" *-- "*" Documento
    Documento "*" --> "1" VersionPlantilla

    Venta "*" --> "1" Cliente
    Venta "1" *-- "1..*" ItemVenta
    Venta "1" *-- "*" OrdenPago
    OrdenPago "1" *-- "*" Pago
```

---

## 3. Clientes, configuración e inmuebles

```mermaid
classDiagram
    class User {
        +str email
        +str name
        +bool is_staff
        +bool is_active
    }
    class CuentaCliente {
        +bool activa
    }
    class Cliente {
        +str codigo
        +bool activo
        +bool es_empresa
        +str tipo
        +str nombre
    }
    class Persona {
        +str rut
        +str nombres
        +str apellido_paterno
        +str apellido_materno
        +str nacionalidad
        +EstadoCivil estado_civil
        +str profesion
        +str domicilio
        +str comuna
        +str region
        +str email
        +str telefono
        +bool activo
        +str nombre_completo
    }
    class Empresa {
        +str rut
        +str razon_social
        +str nombre_fantasia
        +str giro
        +str email
        +str telefono
        +bool activa
    }
    class RepresentanteLegal {
        +date vigente_desde
        +date vigente_hasta
        +date fecha_instrumento
        +str sitio_o_notaria
        +bool activo
    }
    class ConfigEmpresa {
        +str razon_social
        +str rut
        +str direccion
        +str comuna
        +str region
        +date fecha_personeria
        +str registro_personeria
        +str email_contacto
        +str email_pagos
        +str telefono
    }
    class Marca {
        +str nombre
        +bool activa
    }
    class CuentaPago {
        +str banco
        +str tipo_cuenta
        +str numero
        +str titular
        +str texto_contrato
        +bool activa
    }
    class Oficina {
        +TipoInmueble tipo
        +str codigo_sucursal
        +str rol_avaluo
        +str nombre
        +str calle
        +str numero
        +str unidad
        +str comuna
        +str region
        +str unidad_sii
        +str municipalidad
        +bool activa
        +str direccion_completa
    }
    class Autorizante {
        +date vigente_desde
        +date vigente_hasta
        +File documento_identidad
        +File documento_identidad_reverso
        +bool activo
    }
    class TipoInmueble {
        <<enumeration>>
        oficina
        bodega
    }

    User "1" -- "0..1" CuentaCliente
    CuentaCliente "*" --> "1" Cliente
    Cliente "0..1" --> "0..1" Persona
    Cliente "0..1" --> "0..1" Empresa
    Persona "1" -- "*" RepresentanteLegal : representa
    Empresa "1" -- "*" RepresentanteLegal : representada por
    ConfigEmpresa "*" --> "0..1" Persona : representante
    Marca "*" --> "0..1" CuentaPago : cuenta por defecto
    Oficina "1" *-- "*" Autorizante : autorizaciones
    Autorizante "*" --> "0..1" Persona
    Autorizante "*" --> "0..1" Empresa
    Autorizante "*" --> "0..1" RepresentanteLegal
    Oficina ..> TipoInmueble
```

**Reglas que el diagrama no alcanza a mostrar**

- Un `Cliente` es persona natural **o** empresa: exactamente uno de los dos
  vínculos está presente.
- `ConfigEmpresa` es un registro único (`unico = true`): los datos de Easy Office
  que imprimen los contratos.
- El autorizante de un inmueble es una persona natural, o una empresa con su
  representante.
- Una bodega solo se ofrece si tiene dirección, rol de avalúo y un autorizante
  vigente con la cédula por ambos lados (`PLT-02`).

---

## 4. Contratos y ventas

```mermaid
classDiagram
    class Contrato {
        +str folio
        +str codigo_sucursal
        +int anio_folio
        +int correlativo
        +date fecha_inicio
        +int duracion_meses
        +date fecha_vencimiento
        +bool renovacion_automatica
        +int dias_aviso_termino
        +str email_aviso
        +str movil_aviso
        +EstadoContrato estado
        +date terminado_en
    }
    class ServicioContratado {
        +int orden
        +str nombre
        +str descripcion
        +Decimal precio
        +bool precio_incluye_iva
    }
    class CorrelativoFolio {
        +int anio
        +int ultimo
    }
    class DuracionContrato {
        +int meses
        +bool activa
    }
    class EstadoContrato {
        <<enumeration>>
        borrador
        vigente
        terminado
        anulado
    }
    class Servicio {
        +str codigo
        +str nombre
        +str descripcion
        +Decimal precio_base
        +bool precio_incluye_iva
        +bool precio_confirmado
        +bool se_vende_en_portal
        +bool visible_en_portal
        +int orden
        +bool activo
    }
    class PlanServicio {
        +str codigo
        +str nombre
        +int duracion_meses
        +Decimal precio
        +bool precio_incluye_iva
        +bool precio_confirmado
        +bool firma_incluida
        +bool se_vende_en_portal
        +int orden
        +bool activo
    }
    class CicloPago {
        +str codigo
        +str nombre
        +int meses
        +int orden
        +bool activo
    }
    class Venta {
        +str folio
        +EstadoVenta estado
        +Decimal total
        +datetime pagada_en
    }
    class ItemVenta {
        +str nombre
        +Decimal precio_unitario
        +int cantidad
        +Decimal subtotal
    }
    class OrdenPago {
        +str proveedor
        +UUID referencia
        +str id_externo
        +Decimal monto
        +EstadoOrden estado
        +str redirect_url
        +str detalle_error
        +datetime sincronizada_en
    }
    class Pago {
        +str tipo
        +str id_externo
        +Decimal monto
        +str medio
        +str marca
        +str tipo_tarjeta
        +str ultimos_digitos
        +int cuotas
        +str billetera
        +datetime pagado_en
        +json respuesta
    }
    class EventoPago {
        +str proveedor
        +str evento
        +str clave_dedup
        +json payload
        +str resultado
        +datetime recibido_en
    }
    class EstadoVenta {
        <<enumeration>>
        pendiente_pago
        pagada
        reembolsada
        anulada
    }
    class EstadoOrden {
        <<enumeration>>
        creando
        pendiente
        pagada
        rechazada
        expirada
        cancelada
        reembolsada
        error
    }

    Contrato "*" --> "1" Cliente
    Contrato "*" --> "1" Marca
    Contrato "*" --> "0..1" RepresentanteLegal : firma por la empresa
    Contrato "*" --> "1" CuentaPago
    Contrato "*" --> "0..1" User : ejecutivo responsable
    Contrato "0..1" --> "0..1" Contrato : contrato anterior
    Contrato "1" *-- "1..*" ServicioContratado
    ServicioContratado "*" --> "1" Servicio
    ServicioContratado "*" --> "1" CicloPago
    ServicioContratado "*" --> "0..1" Oficina
    Contrato ..> CorrelativoFolio : asigna el folio
    Contrato ..> EstadoContrato

    Servicio "1" *-- "*" PlanServicio : planes
    Servicio "*" --> "0..1" CicloPago : ciclo por defecto
    PlanServicio "*" --> "1" CicloPago

    Venta "*" --> "1" Cliente
    Venta "1" *-- "1..*" ItemVenta
    ItemVenta "*" --> "1" Servicio
    Venta "1" *-- "*" OrdenPago
    OrdenPago "1" *-- "*" Pago
    OrdenPago "0..1" -- "*" EventoPago
    Venta ..> EstadoVenta
    OrdenPago ..> EstadoOrden
```

**Reglas que el diagrama no alcanza a mostrar**

- El folio del contrato tiene la forma `{sucursal}-{correlativo}-{año}`. El
  correlativo es común a todas las sucursales y marcas y se reinicia cada año
  (`CorrelativoFolio`, uno por año).
- Nombre, descripción, precio e IVA de cada línea se **copian** del catálogo al
  contratar: un cambio posterior del catálogo no altera contratos hechos.
- El plan elegido no se guarda en la línea del contrato: queda en
  `Tramite.datos` y de él salen la duración del contrato y el precio de la venta.
- El estado de una venta solo cambia en `ventas.services.aplicar_resultado`. Las
  notificaciones repetidas de un proveedor se descartan por `clave_dedup`.

---

## 5. Trámites y documentos

```mermaid
classDiagram
    class TipoTramite {
        +str codigo
        +str nombre
        +json configuracion
        +bool activo
    }
    class MotivoPendiente {
        +str codigo
        +str nombre
        +str texto_cliente
        +bool activo
    }
    class Tramite {
        +str folio
        +EstadoTramite estado
        +str detalle_pendiente
        +Canal canal
        +json datos
    }
    class EventoTramite {
        +str estado_anterior
        +str motivo_anterior
        +str estado_nuevo
        +str motivo_nuevo
        +str detalle
        +datetime fecha
        +delete() prohibido
    }
    class EstadoTramite {
        <<enumeration>>
        iniciado
        pendiente
        completado
    }
    class Canal {
        <<enumeration>>
        portal
        backoffice
    }
    class PlantillaDocumento {
        +str codigo
        +str nombre
        +bool activa
    }
    class VersionPlantilla {
        +int numero
        +File archivo
        +str sha256
        +str notas
        +datetime vigente_desde
        +save() prohibido si fue usada
        +delete() prohibido si fue usada
    }
    class Documento {
        +int version
        +EstadoDocumento estado
        +File archivo_docx
        +str sha256_docx
        +File archivo_pdf
        +str sha256_pdf
        +json datos_snapshot
        +datetime generado_en
        +File archivo_firmado
        +str sha256_firmado
        +datetime firmado_en
    }
    class Firmante {
        +str rol
        +int orden
        +datetime firmado_en
    }
    class EstadoDocumento {
        <<enumeration>>
        generado
        reemplazado
        firmado
    }

    Tramite "*" --> "1" TipoTramite
    Tramite "*" --> "0..1" MotivoPendiente : mientras está pendiente
    Tramite "*" --> "1" Cliente
    Tramite "*" --> "0..1" Contrato
    Tramite "*" --> "0..1" Venta
    Tramite "*" --> "0..1" User : creado por
    Tramite "1" *-- "*" EventoTramite : historial
    EventoTramite "*" --> "0..1" User
    Tramite ..> EstadoTramite
    Tramite ..> Canal

    PlantillaDocumento "1" *-- "1..*" VersionPlantilla
    Tramite "1" *-- "*" Documento
    Documento "*" --> "1" VersionPlantilla
    Documento "*" --> "0..1" User : firmado por
    Documento "1" *-- "*" Firmante
    Firmante "*" --> "1" Persona
    Documento ..> EstadoDocumento
```

**Reglas que el diagrama no alcanza a mostrar**

- `TipoTramite.configuracion` define el servicio por configuración y no por
  código. Claves: `servicio`, `marca`, `plantilla`, `requiere_pago` (booleano o
  lista de canales), `requiere_firma`, `inicio_max_dias`, `planes`,
  `duracion_por_defecto`, `canales`, `tipos_cliente` y `lineas` (cada una con
  `servicio`, `inmueble`, `requisitos`, `inmueble_exclusivo` e
  `incluido_en_plan`). Hay dos tipos cargados: `domicilio-tributario` (`PLT-01`)
  y `bodega` (`PLT-02`).
- El motivo de pendiente (`revision_borrador`, `pago`, `firma`,
  `falta_informacion`) solo existe mientras el trámite está en estado
  `pendiente`.
- `EventoTramite` no se puede eliminar, y una `VersionPlantilla` ya usada no se
  puede modificar ni eliminar: `save()` y `delete()` lanzan un error. Los usuarios
  inician sesión con su correo (`User` no tiene `username`).
- Cada `Documento` guarda la versión de plantilla, una copia congelada de los
  datos con que se generó (`datos_snapshot`) y la huella SHA-256 del Word, del PDF
  y del PDF firmado. Una nueva generación deja la anterior como `reemplazado`.

---

## 5 bis. Auditoría

En la rama `feature/HU-30-auditoria`, sin fusionar al 22-10-2026.

```mermaid
classDiagram
    class RegistroAuditoria {
        +datetime fecha_hora
        +OrigenAuditoria origen
        +AccionAuditoria accion
        +str entidad
        +int registro_id
        +json valor_anterior
        +json valor_nuevo
        +inet direccion_ip
        +save() prohibido si ya existe
        +delete() prohibido
    }
    class OrigenAuditoria {
        <<enumeration>>
        api
        portal
        admin
        sistema
    }
    class AccionAuditoria {
        <<enumeration>>
        crear
        modificar
        eliminar
        inicio_sesion
        cierre_sesion
        inicio_sesion_fallido
        descarga_documento
        acceso_documento_identidad
    }
    RegistroAuditoria "*" --> "0..1" User : usuario
    RegistroAuditoria ..> OrigenAuditoria
    RegistroAuditoria ..> AccionAuditoria
```

- Las filas las escriben disparadores de PostgreSQL en las tablas auditadas
  (`crear`, `modificar`, `eliminar`) y la función `core.auditoria.registrar_evento`
  para los accesos (ingresos, descargas, cédula). La lista de tablas auditadas
  está en un solo lugar, `core/auditoria/registro.py`.
- `entidad` y `registro_id` identifican el registro afectado sin una clave
  foránea, porque puede pertenecer a cualquier tabla y seguir en el historial
  después de eliminado.
- El usuario se protege (`PROTECT`): quien aparece en el registro no se elimina,
  se desactiva.
- La base rechaza `UPDATE`, `DELETE` y `TRUNCATE` sobre esta tabla.

---

## 6. Interfaces de integración

Las dependencias externas se consumen detrás de una interfaz abstracta con
implementaciones intercambiables por configuración (`AD-04`, `AD-14`).

```mermaid
classDiagram
    class PaymentProvider {
        <<interface>>
        +create_payment(PaymentRequest) PaymentResult
        +get_status(external_id) PaymentResult
        +confirm(external_id) PaymentResult
        +refund(external_id, amount) PaymentResult
        +parse_webhook(payload, headers) PaymentResult
    }
    class KlapPaymentProvider
    class FlowPaymentProvider
    class SimulatedPaymentProvider
    PaymentProvider <|.. KlapPaymentProvider
    PaymentProvider <|.. FlowPaymentProvider
    PaymentProvider <|.. SimulatedPaymentProvider

    class SignatureProvider {
        <<interface>>
        +create_request(SignatureRequest) SignatureResult
        +get_status(external_id) SignatureResult
        +download_signed(external_id) bytes
        +cancel(external_id) SignatureResult
        +parse_webhook(payload, headers) SignatureResult
    }
    class SimulatedSignatureProvider
    SignatureProvider <|.. SimulatedSignatureProvider

    class PdfConverter {
        <<interface>>
        +docx_a_pdf(docx, nombre) bytes
    }
    class GotenbergPdfConverter
    class SimulatedPdfConverter
    PdfConverter <|.. GotenbergPdfConverter
    PdfConverter <|.. SimulatedPdfConverter

    class PaymentRequest {
        +str reference
        +Decimal amount
        +str currency
        +str return_url
        +str notification_url
        +Payer payer
        +PaymentItem[] items
    }
    class PaymentResult {
        +str external_id
        +str reference
        +PaymentStatus status
        +Decimal amount
        +str redirect_url
        +str authorization_code
    }
    PaymentProvider ..> PaymentRequest
    PaymentProvider ..> PaymentResult
    OrdenPago ..> PaymentProvider : proveedor
```

`SignatureProvider` solo tiene la implementación simulada: la del proveedor de
Easy Office espera sus credenciales (TA-07). Mientras tanto la firma es asistida:
un ejecutivo sube el PDF firmado (`documentos.services.registrar_firmado`).

---

## 7. Principales funciones de servicio

| Módulo | Función | Qué hace |
|---|---|---|
| `tramites` | `iniciar_tramite` | Valida la elección contra `TipoTramite.configuracion`, guarda los datos del contratante, crea el contrato en borrador y el trámite, y pide el documento |
| `tramites` | `confirmar_borrador` | El cliente aprueba el borrador; si el tipo requiere pago, crea la venta |
| `tramites` | `registrar_pago` | Recibe la venta pagada y deja el trámite esperando la firma |
| `tramites` | `completar` | Cierra el trámite y activa el contrato |
| `documentos` | `generar` | Llena la plantilla Word, la convierte a PDF, calcula las huellas y congela los datos |
| `documentos` | `registrar_firmado` | Guarda el PDF firmado con su huella y completa el trámite |
| `contratos` | `crear_contrato`, `activar_contrato` | Asigna el folio, copia las líneas del catálogo y calcula el vencimiento |
| `ventas` | `crear_venta`, `iniciar_pago`, `aplicar_resultado` | Precio desde el servidor, orden de pago en el proveedor y único punto donde cambia el estado de la venta |

Los módulos se avisan por señales para no depender en ambos sentidos: `ventas`
emite `venta_pagada` y `tramites` la escucha; `tramites` emite
`documento_requerido` y `documentos` la escucha. La secuencia completa está en
`ARQ-001 §5`.

---

## 8. Diferencias con el modelo conceptual (`MOD-001`)

| Tema | `MOD-001` | Código |
|---|---|---|
| Auditoría | `REGISTRO_AUDITORIA` | `RegistroAuditoria` en `core`, con el campo `origen` agregado (HU-30, sin fusionar) |
| Alertas de vencimiento | `ALERTA_VENCIMIENTO` | No implementada (HU-42, HU-53) |
| Plan de la línea del contrato | `PLAN_SERVICIO` asociado a `SERVICIO_CONTRATADO` | El plan elegido queda en `Tramite.datos`; la línea no lo referencia |
| Nombre del inmueble | `OFICINA` | La clase se llama `Oficina` y representa oficinas y bodegas (`tipo`) |

---

## Control de cambios

| Versión | Fecha | Descripción | Autor |
|---|---|---|---|
| 1.0 | 22-10-2026 | Emisión inicial, desde el código de la rama principal del backend al 22-10-2026 | Equipo de proyecto |
| 1.1 | 22-10-2026 | Sección 5 bis: `RegistroAuditoria` (HU-30, sin fusionar); §8 actualizada | Equipo de proyecto |
