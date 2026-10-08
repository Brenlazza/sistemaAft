# Preborradores de actas generados desde un remito

## Regla funcional confirmada

Una vez definidos y confirmados los destinos del remito, el sistema genera un preborrador de acta por cada productor/establecimiento. El profesional recibe, al retirar la vacuna:

- el remito impreso para firmar;
- la vacuna entregada;
- un juego de acta preimpreso por cada destino confirmado.

Cada juego mantiene el formato triplicado acordado:

- original para la Fundación;
- duplicado para SENASA;
- triplicado para el productor.

El profesional completa manualmente en campo las cantidades realmente vacunadas, firmas, fecha efectiva y demás datos variables. Luego devuelve el original para su carga, junto con los frascos llenos o sobrantes que correspondan.

## Naturaleza del preborrador

En la interfaz se denomina `Preborrador de acta`. Técnicamente representa un formulario físico preimpreso y vinculado, no una vacunación confirmada.

Por lo tanto, generarlo o imprimirlo:

- no consume dosis;
- no actualiza cobertura;
- no indica que el establecimiento fue vacunado;
- no suma cantidades en la conciliación;
- no crea una factura;
- sí reserva y controla el formulario físico cuando se implemente su numeración.

La vacunación produce efectos solamente cuando el acta regresa, se transcribe y se confirma.

## Generación automática

El sistema crea un preborrador por cada fila de `remito_destinos`. Para generarlos se requiere:

- remito guardado y preparado;
- veterinaria propietaria definida;
- profesional receptor habilitado;
- lote o lotes previstos;
- destinos con vínculo cliente/productor–SENASA resuelto;
- campaña y UEL activas.

Si un remito tiene tres destinos, se generan tres juegos de acta diferentes. No se genera un acta conjunta para varios productores.

## Datos preimpresos

Cada preborrador toma una instantánea de:

- campaña, período y año;
- UEL;
- número del remito de origen;
- cliente/productor;
- documento o CUIT disponible;
- RENSPA;
- establecimiento, domicilio y ubicación;
- régimen, hectáreas y tipo de rodeo cuando estén disponibles;
- marca del ganado cuando esté digitalizada;
- profesional y matrícula;
- veterinaria propietaria;
- vacuna, marca, serie/lote y vencimiento previstos;
- fecha de impresión;
- número del formulario cuando se defina su mecanismo.

El número de remito puede imprimirse en una zona administrativa o código de referencia para facilitar la carga posterior.

## Datos que quedan vacíos para el campo

- lugar y fecha efectiva de vacunación;
- animales vacunados por categoría;
- total realmente vacunado;
- existencias de otras especies;
- resultados o motivos sanitarios;
- observaciones;
- firmas, aclaraciones y DNI del vacunador y del propietario o responsable;
- cualquier corrección del lote realmente utilizado.

Las dosis previstas cargadas en el remito no deben copiarse en los casilleros de animales vacunados. Son una estimación logística, no un hecho sanitario.

Las firmas y los DNI quedan en blanco para ser completados manualmente en los tres ejemplares físicos. No se controlan mediante tildes en la carga digital.

## Entrega al profesional

La pantalla muestra una lista de preborradores antes de imprimir:

| Destino | Estado inicial | Acción |
|---|---|---|
| Productor/establecimiento vinculado | Pendiente de generar | Generar juego |
| Juego generado | Preimpreso | Vista previa / imprimir |
| Juego entregado al profesional | Entregado | Esperar devolución |

Se permite imprimir todos los juegos juntos mediante `Imprimir remito y actas` o abrir un destino individual.

Al registrar la entrega de la vacuna, los preborradores impresos pasan a `Entregados` junto con el remito. El profesional firma el remito y se lleva los formularios.

## Regreso y carga

Cuando vuelve un acta:

1. se busca por número de formulario, remito, productor o RENSPA;
2. se abre el preborrador asociado;
3. se transcriben los datos manuscritos;
4. se registra el lote realmente utilizado;
5. se cargan las devoluciones o sobrantes;
6. se valida y confirma el acta;
7. recién entonces se actualizan consumo, cobertura y conciliación.

La transcripción y sus controles se detallan en la [pantalla de carga y confirmación del acta](pantalla-carga-confirmacion-acta.md).

El operador no crea otra acta si ya existe un preborrador para ese formulario: completa el registro asociado.

## Productor agregado posteriormente

Cuando se agrega otro productor a un remito ya emitido:

1. se genera la nueva versión del remito y se imprime para nueva firma;
2. se crea un preborrador solamente para el nuevo destino;
3. se imprime su juego triplicado;
4. los preborradores de los destinos anteriores permanecen válidos y no se duplican;
5. todos continúan vinculados al mismo número de remito.

Si cambian datos que afectan un preborrador ya impreso —profesional, lote previsto, productor o establecimiento— se requiere anular o reimprimir ese juego con trazabilidad. Agregar únicamente otro destino no obliga a reimprimir las actas de los destinos anteriores.

## Formularios no utilizados

Si el profesional no vacuna a uno de los destinos:

- devuelve el juego sin utilizar;
- el formulario queda `Devuelto sin utilizar` o `Anulado`, según el motivo;
- no se registra consumo ni cobertura;
- el número del formulario no se reutiliza si ya fue impreso.

## Controles de integridad

- un preborrador pertenece a un solo destino del remito;
- un destino puede requerir más de un acta parcial, pero cada juego debe generarse expresamente;
- no generar automáticamente actas adicionales solo porque sobren dosis;
- no duplicar un juego por repetir una solicitud de impresión;
- una reimpresión conserva el mismo formulario y registra versión y motivo;
- un acta confirmada conserva el vínculo con el remito y destino que originaron su preborrador.

