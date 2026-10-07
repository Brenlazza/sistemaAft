# Lógica funcional de stock de vacunas

## Objetivo

Mantener trazabilidad completa desde la recepción de una serie/lote hasta su aplicación en un acta, devolución, rotura o decomiso en la UEL y ajuste autorizado. El saldo no se edita manualmente: se calcula a partir de movimientos confirmados.

## Identidad del stock

En la primera versión la vacuna antiaftosa es el único producto administrado. No se necesita todavía un catálogo operativo de productos diferentes. Toda existencia se diferencia por:

- veterinaria propietaria;
- serie/lote textual;
- fecha de vencimiento;
- condición del stock;
- ubicación física o responsable actual.

La presentación es fija: frascos de 125 cc equivalentes operativamente a 125 dosis. Para los frascos cerrados se almacena una cantidad entera de frascos y su equivalente se calcula multiplicando por 125. Dos series o stocks pertenecientes a veterinarias diferentes nunca se mezclan. El número de serie/lote se almacena como texto para conservar letras y ceros iniciales.

## Ubicaciones

El stock puede estar en:

- depósito o UEL;
- poder del vacunador o veterinario que lo retiró.

La veterinaria es propietaria del stock, pero no necesariamente su ubicación física: los frascos comprados llegan a la UEL, donde se almacenan y administran separados por veterinaria. La entrega cambia la custodia al profesional que los retira, sin cambiar la veterinaria propietaria.

La condición del stock puede ser:

- `Lleno`: frasco cerrado de 125 cc;
- `Sobrante`: contenido de un frasco abierto devuelto a la UEL;
- `Decomisado`: no disponible para aplicación;
- `Roto`: pérdida física registrada.

## Tipos de movimiento

| Tipo | Origen | Destino o efecto |
|---|---|---|
| Recepción | Compra de una veterinaria | Incrementa en la UEL el stock lleno perteneciente a esa veterinaria. |
| Retiro con remito | Stock lleno de la veterinaria en la UEL | Entrega frascos al vacunador/veterinario autorizado. |
| Consumo | Stock del vacunador | Descuenta lo aplicado en un acta confirmada. |
| Devolución de sobrante | Vacunador/veterinario | Ingresa en la UEL un sobrante abierto de la misma veterinaria y lo vincula al productor/establecimiento. |
| Decomiso en UEL | Stock físico en la UEL | Pasa la existencia a condición `Decomisado` y descuenta su disponibilidad en la UEL. |
| Rotura en UEL | Stock físico en la UEL | Pasa la existencia a condición `Roto` y descuenta su disponibilidad en la UEL. |
| Ajuste positivo/negativo | Ubicación responsable | Corrige una diferencia inventariada con justificación. |
| Reversión | Movimiento confirmado | Compensa una anulación o rectificación sin borrar historia. |

## Estados del movimiento

| Estado | Comportamiento |
|---|---|
| Borrador | Editable; no afecta saldos. |
| Confirmado | Inmutable; afecta saldos. |
| Anulado | Conserva historia y posee una reversión vinculada cuando afectó saldos. |

Un movimiento confirmado no se modifica ni elimina. Cualquier corrección genera otro movimiento vinculado.

## Recepción en la UEL

Las veterinarias compran la vacuna y son propietarias de las cantidades adquiridas. Los frascos llegan físicamente a la UEL para su almacenamiento y administración. Para confirmar una recepción se requiere:

- veterinaria propietaria;
- laboratorio y marca;
- serie/lote y vencimiento;
- cantidad entera positiva de frascos cerrados de 125 cc;
- fecha efectiva;
- UEL receptora;
- origen o proveedor;
- comprobante cuando corresponda;
- usuario responsable.

La recepción suma los frascos al stock lleno de la veterinaria correspondiente dentro de la UEL. El sistema permite recibir la misma serie en fechas diferentes, manteniendo cada comprobante y movimiento.

## Autorización y retiro mediante remito

La veterinaria propietaria emite manualmente una autorización firmada, con sus datos, para que un vacunador o veterinario retire vacuna de su stock almacenado en la UEL.

El nuevo sistema implementará el circuito de remitos que actualmente se gestiona en Xubio:

1. registrar la veterinaria propietaria y la autorización firmada;
2. identificar al vacunador/veterinario autorizado para retirar;
3. seleccionar uno o más destinos, vinculando en cada caso el cliente/productor canónico, el registro SENASA de la campaña y el establecimiento/RENSPA;
4. seleccionar serie/lote y cantidad de frascos llenos;
5. validar que exista saldo suficiente de esa veterinaria en la UEL;
6. generar e imprimir el remito de retiro;
7. obtener la firma del profesional que recibe y asume responsabilidad;
8. confirmar la entrega y descontar los frascos del stock lleno de la veterinaria en la UEL;
9. asignar la custodia de esos frascos al profesional receptor.

El remito conserva número, fecha, veterinaria, receptor, UEL, serie/lote, cantidad de frascos, destinos y firmas. Una entrega nunca se descuenta del stock de otra veterinaria, aunque coincidan marca y lote.

Un mismo remito puede abastecer a varios productores. Por ejemplo, un frasco de 125 dosis puede respaldar actas por 50, 50 y 25 animales de tres productores diferentes. Cada acta identifica un destino del remito y la conciliación suma las dosis de todas las actas vinculadas.

También puede agregarse un productor a un remito ya entregado. El remito conserva su número, incrementa su versión, se imprime completo nuevamente y el profesional receptor debe volver a firmarlo. La impresión y firma anteriores se conservan para auditoría; no puede cerrarse la conciliación mientras la versión vigente esté pendiente de firma.

### Estados del remito

| Estado | Significado | Efecto sobre stock |
|---|---|---|
| Borrador | Datos todavía editables. | Ninguno. |
| Autorizado | Existe autorización firmada de la veterinaria propietaria. | Ninguno. |
| Entregado | El profesional firmó y retiró los frascos. | Descuenta stock lleno en UEL y cambia la custodia. |
| Anulado | El retiro no se realiza o fue revertido con motivo. | Ninguno o movimiento inverso si ya había sido entregado. |

El remito no puede pasar a `Entregado` sin identificar al receptor y registrar su firma. La autorización y el comprobante de retiro quedan vinculados para futuras consultas.

## Relación con formularios y actas

La serie/lote impresa en el formulario representa la vacuna prevista para la visita. Emitir o imprimir el formulario no descuenta frascos ni dosis. El retiro se controla mediante remito y el consumo efectivo mediante el acta.

Al confirmar el acta digital:

- se usa la serie/lote efectivamente consignada en campo;
- se valida la veterinaria propietaria y que el lote estuviera bajo custodia del vacunador en la fecha de aplicación;
- se crea un consumo vinculado de manera única al acta;
- se impide confirmar dos consumos para la misma aplicación.

Cuando se abre un frasco, deja de formar parte del stock lleno. La aplicación registra la cantidad utilizada y, si queda contenido, su devolución crea un sobrante independiente. Los frascos cerrados no utilizados pueden devolverse manteniendo su condición `Lleno`.

Cada acta parcial tiene su propio consumo. Varias actas del mismo establecimiento y campaña pueden utilizar fechas, series/lotes y cantidades distintas sin mezclarse. A su vez, varias actas de establecimientos o productores diferentes pueden consumir vacuna de un mismo remito cuando figuren entre sus destinos.

## Categorías excepcionales

Si una campaña admite excepcionalmente vacunar categorías bovinas o bubalinas adicionales, el consumo de stock se calcula sobre la cantidad realmente vacunada y autorizada. La excepción no altera las reglas del lote ni permite saldo negativo.

## Cálculo de saldo

Para cada veterinaria propietaria, UEL y serie/lote se mantienen dos saldos separados:

- `stock lleno`: cantidad entera de frascos cerrados de 125 cc;
- `stock sobrante`: remanentes de frascos abiertos devueltos.

El saldo de frascos llenos se calcula como:

`frascos llenos = frascos recibidos - frascos retirados - frascos rotos/decomisados ± ajustes`

Los borradores no forman parte del saldo. Los movimientos anulados se neutralizan mediante reversiones, manteniendo la trazabilidad.

El sistema debe mostrar por separado:

- saldo físico esperado;
- frascos llenos disponibles por veterinaria y lote;
- sobrantes separados por veterinaria, lote y productor/establecimiento de origen;
- cantidades decomisadas;
- cantidades rotas.

La cantidad comprometida no es un descuento contable hasta que exista un movimiento confirmado.

## Reglas de disponibilidad

- no permitir una cantidad de frascos llenos fraccionaria, cero o negativa;
- no permitir saldo negativo;
- no entregar existencias decomisadas o rotas;
- no aplicar un lote vencido en la fecha de vacunación;
- advertir vencimientos próximos;
- respetar la presentación fija de 125 cc;
- confirmar movimiento y actualización de saldo en una única transacción;
- impedir duplicaciones por reenvío de la misma operación.

## Devolución de sobrantes y conciliación

Cuando el profesional retira un frasco lleno y no utiliza todo su contenido, devuelve el frasco abierto a la UEL. El ingreso se registra como `Sobrante`, separado del stock lleno, conservando:

- veterinaria propietaria;
- vacunador/veterinario que devuelve;
- serie/lote y vencimiento;
- cantidad remanente declarada;
- productor y establecimiento donde se utilizó;
- acta relacionada;
- fecha de devolución;
- responsable que recibe en la UEL.

El sobrante nunca vuelve a contarse como frasco lleno y se mide en dosis remanentes. Un frasco contiene 125 dosis: si se aplican 100, la devolución registra 25 dosis de sobrante. La cantidad aplicada y la cantidad realmente remanente se almacenan por separado.

La conciliación detallada entre remito, vacunación y devolución se define en [flujo de remitos y conciliación](flujo-remitos-conciliacion.md). La diferencia esperada entre dosis retiradas, suma de dosis vacunadas en todas las actas vinculadas y devoluciones es cero; cualquier desvío queda observado y auditado.

Al finalizar una etapa o campaña, cada vacunador informa por serie/lote:

- frascos llenos retirados mediante remitos;
- cantidad aplicada en todas las actas confirmadas vinculadas a esos remitos;
- sobrantes devueltos;
- frascos llenos sin utilizar;
- diferencia.

Toda diferencia requiere motivo. Los administradores generales pueden autorizar el ajuste; el coordinador no lo realiza salvo permiso adicional explícito.

Las vacunas rotas o decomisadas se registran únicamente para descontar el stock físico de la UEL. Son movimientos separados, con motivo y autorización, y no pueden utilizarse para cuadrar la responsabilidad de un profesional sobre un remito ya entregado.

## Anulación o rectificación de un acta

Si un acta confirmada se anula, el consumo original no se borra. Se genera una reversión por la misma serie/lote y cantidad, vinculada al acta y al movimiento original.

Si una rectificación cambia cantidades o lote:

1. se revierte la parte incorrecta;
2. se valida disponibilidad de la nueva serie/lote;
3. se registra el nuevo consumo;
4. se conserva el vínculo entre todos los movimientos.

## Privacidad y auditoría

Los usuarios, fechas técnicas, valores anteriores y motivos internos quedan almacenados para auditoría privada. No se imprimen en el acta ni se muestran en pantallas comunes de coordinación.

## Decisiones pendientes

1. Regla sanitaria y plazo de reutilización o descarte de un sobrante abierto.
2. Si se admite alguna tolerancia distinta de cero durante la conciliación.
3. Anticipación utilizada para considerar un lote próximo a vencer.
4. Si la autorización manual de la veterinaria se adjunta escaneada o se conserva solo en papel.
