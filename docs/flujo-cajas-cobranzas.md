# Flujo futuro de cajas y cobranzas

## Objetivo

Registrar los cobros de facturas de manera individual por usuario y centro, con apertura, movimientos, comprobantes, cierre y rendición. No todos los usuarios pueden facturar y no todo usuario que factura necesariamente opera una caja.

El [prototipo navegable](../prototipos/caja-cobranzas.html) muestra una sesión de caja y el cobro de facturas emitidas.

## Separación de permisos

Se definen acciones independientes:

- `facturacion.preparar`: crear y revisar liquidaciones;
- `facturacion.emitir`: emitir facturas o enviarlas al servicio fiscal;
- `facturacion.anular`: gestionar notas de crédito o anulaciones;
- `caja.abrir`: iniciar una sesión de caja asignada;
- `caja.cobrar`: registrar pagos sobre facturas emitidas;
- `caja.revertir`: anular un cobro mediante movimiento inverso;
- `caja.cerrar`: contar valores y cerrar la sesión propia;
- `caja.supervisar`: controlar diferencias, aprobar rendiciones y reabrir según procedimiento;
- `caja.consultar_global`: consultar cajas de otros usuarios o centros.

Ejemplos:

- un operador puede cobrar facturas ya emitidas sin poder crearlas;
- un administrativo puede preparar liquidaciones sin emitir ni cobrar;
- un responsable de facturación puede emitir sin manejar efectivo;
- un administrador puede supervisar todas las cajas sin registrar cobros cotidianos.

## Configuración de cajas

Cada caja pertenece a un centro o sede y conserva:

- código y nombre;
- centro y ubicación física;
- moneda;
- medios de pago habilitados;
- usuarios autorizados y vigencia;
- cuenta contable o destino de fondos;
- estado activo/inactivo.

Los subcentros pueden tener cajas propias. Un usuario solo ve y opera las cajas expresamente asignadas dentro de su alcance.

## Apertura

Antes de cobrar, el usuario abre una sesión registrando:

- caja;
- fecha y hora;
- turno o referencia;
- saldo inicial en efectivo;
- desglose opcional de billetes;
- observación.

Reglas iniciales:

- un usuario no puede tener dos sesiones abiertas al mismo tiempo;
- una caja física no admite dos sesiones simultáneas salvo configuración expresa de puestos separados;
- el saldo inicial debe coincidir con el cierre/rendición anterior o registrar una diferencia autorizada;
- abrir caja no concede permiso para emitir facturas.

## Registro de un cobro

1. Buscar cliente, factura, acta, establecimiento o RENSPA.
2. Mostrar facturas emitidas, saldo original, pagos previos y saldo pendiente.
3. Seleccionar una o varias facturas del mismo cliente.
4. Ingresar importe y uno o varios medios de pago.
5. Distribuir el cobro entre facturas; se admiten pagos parciales.
6. Confirmar en una única transacción.
7. Generar recibo interno y movimiento de caja.

El usuario con permiso `caja.cobrar` no puede modificar cantidades de animales, tarifas, facturas ni datos fiscales desde esta pantalla.

## Medios de pago

El modelo contempla inicialmente:

- efectivo;
- transferencia;
- tarjeta u otro medio electrónico;
- cheque;
- combinación de medios.

Cada parte del pago conserva importe, referencia, entidad, fecha efectiva y comprobante adjunto cuando corresponda.

Solo el efectivo incrementa el dinero esperado en el arqueo físico. Transferencias, tarjetas y cheques aparecen en la rendición, pero se concilian por su cuenta o medio correspondiente.

## Estados

### Sesión de caja

- `Abierta`;
- `En cierre`;
- `Cerrada`;
- `Rendida`;
- `Observada`.

### Cobro

- `Confirmado`;
- `Revertido`;
- `Observado`.

### Factura respecto del cobro

- `Pendiente`;
- `Parcialmente cobrada`;
- `Cobrada`;
- `Con saldo a favor` cuando exista un crédito tratado por el procedimiento autorizado.

## Inmutabilidad y correcciones

Un cobro confirmado no se edita ni elimina. Si fue incorrecto:

- se registra una reversión vinculada con motivo y permiso;
- se restaura el saldo de las facturas;
- se genera el movimiento contrario en la caja y en cada medio de pago;
- luego se registra el cobro correcto.

La reversión de un cobro no anula la factura. La anulación de una factura sigue su propio circuito fiscal y debe considerar los cobros ya imputados.

## Cierre y arqueo

Al cerrar, el sistema calcula:

`efectivo esperado = saldo inicial + cobros en efectivo + ingresos manuales autorizados - egresos y reversiones`

El usuario declara el efectivo contado. El sistema muestra:

- saldo inicial;
- cobros por cada medio;
- reversiones;
- ingresos/egresos autorizados;
- efectivo esperado;
- efectivo contado;
- diferencia;
- facturas y recibos incluidos.

Una diferencia no se corrige modificando movimientos. Se registra con explicación y queda pendiente de revisión del supervisor.

## Rendición

Después del cierre, un usuario autorizado controla:

- efectivo entregado;
- comprobantes de transferencia y tarjeta;
- cheques recibidos;
- diferencias declaradas;
- recibos anulados o revertidos;
- correspondencia con facturas y clientes.

La rendición cambia la sesión a `Rendida`. Reabrir una caja cerrada o rendida requiere permiso superior, motivo y auditoría.

## Reportes futuros

- caja diaria por usuario y centro;
- cobros por medio de pago;
- facturas pendientes, parciales y cobradas;
- rendiciones y diferencias;
- movimientos revertidos;
- recaudación por campaña, centro, veterinario o idóneo;
- deuda por cliente y antigüedad;
- trazabilidad desde factura y cobro hasta actas y animales vacunados.

## Implementación por etapas

1. Registrar facturas externas y cobros básicos por usuario.
2. Incorporar apertura, cierre, arqueo y rendición.
3. Integrar la emisión de facturas dentro del sistema.
4. Integrar medios electrónicos, bancos y servicio fiscal cuando sea conveniente.

El diseño permite comenzar con cajas aunque la factura todavía se emita en el sistema actual, utilizando su número como referencia externa.
