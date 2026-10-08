# Flujo de stock entre UEL principal y subcentros

## Estructura operativa

La Fundación funciona como una única unidad ejecutora con varios centros físicos de stock:

- `Principal`: depósito central de San Cristóbal, donde operan actualmente diez veterinarias;
- `Subcentro`: cinco depósitos dependientes ubicados en otras localidades, cada uno con una o más veterinarias;
- eventualmente, otras ubicaciones internas habilitadas.

Los subcentros no se modelan como veterinarias ni como vacunadores. Son ubicaciones físicas dependientes de la UEL principal. Cada uno conserva localidad, domicilio, responsable, estado y centro principal del que depende. Operan con el mismo circuito de remitos, actas, devoluciones y conciliaciones que San Cristóbal, pero solamente sobre el stock disponible en ese subcentro.

## Dos datos que nunca deben mezclarse

Cada existencia conserva simultáneamente:

1. **Asignación o propiedad:** veterinaria a la que corresponde, o `Sin asignar` mientras permanece en el stock general pendiente de distribución.
2. **Ubicación física:** principal San Cristóbal, un subcentro o custodia de un profesional mediante remito.

Asignar vacuna a una veterinaria no significa por sí solo que haya salido de San Cristóbal. De igual modo, una transferencia a un subcentro no cambia la veterinaria propietaria.

## 1. Registrar cada llegada en San Cristóbal

Cada llegada se registra como una recepción independiente, aunque repita marca, serie/lote y vencimiento de una recepción anterior. Esto permite recibir una compra o entrega en varias tandas sin perder fechas ni responsables.

### Cabecera de recepción

- número interno de recepción;
- fecha y hora efectiva de llegada;
- centro receptor, inicialmente San Cristóbal;
- proveedor u origen;
- número de remito, factura o comprobante de origen;
- responsable que entrega, opcional;
- responsable de la Fundación que recibe, opcional;
- observación;
- documento adjunto, cuando exista;
- temperatura de recepción y estado de sensores;
- control de integridad de las conservadoras y números de remito acompañantes.

### Detalle por lote

- laboratorio y marca;
- serie/lote almacenado como texto;
- fecha de vencimiento;
- cantidad de frascos llenos;
- dosis equivalentes calculadas como frascos × 125;
- estado inicial `Sin asignar`.

Una misma recepción puede contener varios lotes. Un mismo lote puede aparecer en muchas recepciones. Confirmar una recepción incrementa el stock lleno sin asignar del centro principal y el movimiento confirmado ya no se edita: cualquier corrección se realiza mediante reversión.

Aunque la distribución prevista se conozca antes de la llegada, no afecta saldos hasta que toda la vacuna haya llegado y se hayan completado los controles de recepción. Puede guardarse como planificación, pero la asignación efectiva se confirma después.

## 2. Asignar existencias a veterinarias

Después de confirmar la recepción y sus controles se realiza una única operación de distribución. Cada línea indica:

- veterinaria;
- serie/lote y vencimiento;
- cantidad entera de frascos;
- centro donde quedarán físicamente disponibles;
- fecha y hora;
- usuario responsable;
- documento o motivo de la asignación.

Para las diez veterinarias que operan en San Cristóbal, la distribución asigna la veterinaria y conserva la ubicación principal. Para una veterinaria de un subcentro, la misma confirmación asigna la veterinaria y genera la transferencia hacia ese subcentro. Internamente siguen siendo dos movimientos vinculados para conservar propiedad y ubicación, aunque el usuario complete una sola pantalla.

No se permite asignar más frascos que el saldo `Sin asignar` disponible del lote. Una reasignación entre veterinarias requiere una operación expresa, motivo y autorización; nunca se cambia el nombre sobre un movimiento anterior.

## 3. Transferir a un subcentro

Cuando los frascos deben quedar físicamente en otra localidad, la distribución genera una transferencia interna. No es el remito mediante el que el veterinario retira para vacunar. La transferencia debe poder imprimir el formato oficial `Acta de entrega y recepción de vacunas — Subcentros Operativos`.

La transferencia indica:

- centro de origen y subcentro de destino;
- veterinaria propietaria;
- serie/lote y vencimiento;
- frascos enviados;
- fecha y hora de salida;
- responsable que prepara y entrega;
- transportista o responsable del traslado, si corresponde;
- fecha y hora de recepción;
- responsable que recibe en el subcentro;
- observaciones o diferencias de recepción.

El acta oficial agrega, por línea, vacuna, serie, total de envases y dosis, temperatura de entrega, envases devueltos con vacuna, dosis devueltas, temperatura de recepción y envases devueltos vacíos. También identifica campaña, plan, subcentro, localidad, vacunador programador y firmas de ambas partes.

Estados sugeridos:

| Estado | Efecto |
|---|---|
| Borrador | No afecta saldos. |
| Preparada | Reserva la cantidad en el origen para evitar otra entrega. |
| En tránsito | Descuenta disponibilidad del origen y muestra custodia en traslado. |
| Recibida | Incrementa disponibilidad física del subcentro. |
| Observada | El destino informa diferencia, daño o documentación pendiente. |
| Anulada | Revierte mediante movimientos vinculados cuando ya hubo efectos. |

La transferencia no cambia el stock total de la Fundación ni la veterinaria propietaria: solamente cambia la ubicación y la custodia. Si asignación y traslado se realizan al mismo tiempo, la interfaz puede ofrecer una acción única, pero internamente registra ambos movimientos.

### Validación del subcentro

Cada subcentro opera con cuentas individuales limitadas a su propio centro. Una transferencia `En tránsito` no ingresa automáticamente a su saldo disponible. El usuario receptor compara físicamente marca, lote, vencimiento, envases, dosis, temperatura, integridad y documentación con lo declarado por la principal.

Si todo coincide, la aceptación registra los controles y crea el movimiento de entrada local en una única transacción. Desde ese momento el stock puede utilizarse en remitos del subcentro. Si existe una diferencia, la transferencia queda `Observada` y no se habilita para retiros hasta su resolución. Una recepción con diferencia requiere autorización superior e ingresa solamente la cantidad efectivamente comprobada, conservando por separado lo enviado, lo recibido y la diferencia.

La definición completa está en [Recepción de transferencias en subcentros](pantalla-recepcion-transferencia-subcentro.md).

## 4. Retirar para vacunación

El remito de retiro se emite desde el centro que entrega físicamente la vacuna:

- una veterinaria de San Cristóbal retira del saldo disponible en el centro principal;
- una veterinaria abastecida en otro lugar retira del saldo disponible en ese subcentro;
- el usuario del subcentro solamente puede seleccionar veterinarias activamente asociadas a su centro;
- no se permite retirar en un centro existencias que figuran físicamente en otro;
- el remito puede combinar frascos llenos y dosis sobrantes de la misma veterinaria disponibles en ese centro.

Desde este punto continúa el circuito ya definido: profesional receptor, productores, preborradores de actas, vacunación, devolución y conciliación.

## 5. Devoluciones y bajas

Las devoluciones ingresan normalmente al centro que emitió el remito. Conservan veterinaria, lote y condición:

- frascos sin abrir regresan a `Lleno`;
- remanentes de frascos abiertos ingresan a `Sobrante` en dosis;
- una transferencia posterior puede moverlos a otro centro sin cambiar su origen ni propietario;
- roturas y decomisos se registran en el centro donde suceden y descuentan su stock físico.

## Saldo resultante

El saldo se consulta por la combinación:

`centro + veterinaria o Sin asignar + lote + vencimiento + condición`

El sistema debe mostrar, como mínimo:

- total general recibido en San Cristóbal;
- stock sin asignar;
- stock asignado por veterinaria;
- stock físico por centro;
- cantidades preparadas o en tránsito;
- frascos llenos y dosis sobrantes;
- roturas, decomisos y ajustes;
- trazabilidad desde la recepción original hasta el remito y el acta.

## Ejemplo

1. El 10 de octubre a las 08:30 llegan 100 frascos del lote `AF-26091` a San Cristóbal.
2. El 12 de octubre llegan otros 40 frascos del mismo lote. Son dos recepciones, pero el saldo agregado es 140.
3. Después del control se distribuyen 50 frascos a Veterinaria A de San Cristóbal y 30 a Veterinaria B del subcentro Ceres; quedan 60 sin asignar.
4. La confirmación conserva los 50 de Veterinaria A en el depósito principal y genera la transferencia de los 30 de Veterinaria B hacia Ceres.
5. Los remitos de Ceres solamente pueden utilizar los 30 frascos confirmados allí. Los remitos de San Cristóbal pueden utilizar los 50 que permanecen en el centro principal.

Antes del paso 5, un usuario habilitado de Ceres debe controlar y aceptar la transferencia. Mientras figure `En tránsito` u `Observada`, los 30 frascos no están disponibles para ningún remito de Ceres.

## Controles esenciales

- no permitir saldos negativos ni cantidades fraccionarias de frascos llenos;
- conservar cada tanda de recepción aunque coincida el lote;
- no mezclar lotes, vencimientos, veterinarias ni ubicaciones;
- impedir retiros sobre transferencias todavía no recibidas;
- advertir lotes vencidos o próximos a vencer en todos los centros;
- confirmar cabecera, detalles y movimientos de saldo en una única transacción;
- conservar documentos, usuarios y fechas para auditoría privada.
