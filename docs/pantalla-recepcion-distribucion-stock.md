# Pantallas de recepción y distribución de stock

## Objetivo

Registrar cada llegada de vacuna a San Cristóbal, completar los controles exigidos y, una vez confirmada la recepción, distribuir en una sola operación los frascos entre las veterinarias del depósito principal y las veterinarias de los cinco subcentros.

El circuito se divide visualmente en dos pasos, pero queda vinculado desde la recepción original hasta cada saldo resultante.

El [prototipo navegable](../prototipos/stock-recepcion-distribucion.html) replica la estructura visual de las planillas oficiales para facilitar la familiarización. Incluye vistas A4 del [acta de recepción](../prototipos/acta-recepcion-vacunas-impresion.html) y del [acta para subcentros](../prototipos/acta-subcentro-impresion.html).

## Bandeja de recepciones

La entrada muestra:

- número interno de recepción;
- fecha y hora de llegada;
- campaña;
- comprobante o remito de origen;
- lotes y total de frascos/dosis;
- estado del control;
- estado de distribución;
- documentos impresos.

Filtros: campaña, fecha, lote, marca, comprobante y estado.

Estados sugeridos:

- `Borrador`;
- `Pendiente de control`;
- `Controlada`;
- `Distribución en preparación`;
- `Distribuida`;
- `Observada`;
- `Anulada` mediante reversión.

## Paso 1 — Recepción y control

### Cabecera

- campaña: primera o segunda y año;
- centro receptor fijo: depósito principal San Cristóbal;
- fecha y hora efectiva de llegada;
- oficina SENASA y plan;
- proveedor u origen, cuando corresponda;
- número o números de remito acompañantes;
- observaciones;
- responsable que entrega, opcional;
- responsable nominal que recibe, opcional;
- usuario del sistema, automático y privado.

### Detalle repetible

Una fila por combinación de marca, serie y vencimiento:

| Campo | Regla |
|---|---|
| Vacuna/marca | Selección del catálogo. |
| Serie/lote | Texto obligatorio; conserva letras y ceros iniciales. |
| Total de envases | Frascos enteros positivos. |
| Total de dosis | Automático: envases × 125. |
| Fecha de vencimiento | Obligatoria. |
| Temperatura de recepción | Valor registrado en el control de llegada. |
| Estado de sensores | Conforme, observado o sin sensor, con detalle cuando corresponda. |
| Integridad de conservadoras | Conforme u observada. |

Todas las líneas ingresan inicialmente como stock lleno `Sin asignar` en San Cristóbal.

### Acciones

- `Guardar borrador`;
- `Registrar control`;
- `Vista previa del acta de recepción`;
- `Imprimir acta de recepción`;
- `Confirmar recepción`;
- `Observar recepción`;
- `Anular`, con reversión si ya fue confirmada.

La confirmación es atómica: crea la recepción, sus detalles y los movimientos de entrada. Después no se editan cantidades ni lotes; una corrección genera movimientos compensatorios.

## Paso 2 — Distribución posterior al control

La acción `Distribuir vacuna` se habilita únicamente para una recepción confirmada y controlada. La pantalla muestra arriba los totales recibidos y abajo una grilla de distribución.

### Grilla de distribución

Cada fila contiene:

- serie/lote y vencimiento;
- centro de destino;
- veterinaria que opera en ese centro;
- cantidad de frascos;
- dosis equivalentes;
- observación.

Al seleccionar el centro, la lista de veterinarias se limita a las habilitadas allí:

- San Cristóbal ofrece sus diez veterinarias;
- cada subcentro ofrece una o más veterinarias vinculadas.

El resumen por lote muestra:

- frascos recibidos;
- frascos ya distribuidos;
- frascos incluidos en la distribución actual;
- frascos pendientes de asignación;
- diferencia o exceso.

No se permiten asignaciones superiores al saldo recibido. La distribución puede prepararse como borrador hasta completar todas las líneas; la confirmación operativa se realiza en conjunto después del control de llegada.

### Efecto de la confirmación

- Para una veterinaria de San Cristóbal: asigna la propiedad operativa y conserva la ubicación principal.
- Para una veterinaria de un subcentro: asigna la propiedad y crea la transferencia interna al subcentro.
- Conserva el vínculo con la recepción y línea de lote originales.
- No cambia el total general de vacuna recibida.

### Documentos y acciones

- `Guardar distribución`;
- `Validar totales`;
- `Confirmar distribución`;
- `Imprimir resumen de distribución`;
- `Imprimir acta de entrega y recepción` por cada subcentro;
- `Registrar recepción del subcentro`;
- `Informar diferencia u observación`.

## Recepción en el subcentro

Cada subcentro confirma lo que recibió indicando:

- fecha y hora;
- temperatura de recepción;
- envases y dosis reales;
- responsable receptor;
- observación;
- firma manual en el acta impresa.

Si coincide, la transferencia pasa a `Recibida` y el stock queda disponible para remitos del subcentro. Si no coincide, pasa a `Observada`; se conserva lo enviado y lo recibido sin reemplazar ninguno de los valores.

## Vista de stock posterior

Al finalizar, el usuario puede consultar por centro, veterinaria, lote y condición:

- frascos llenos disponibles;
- dosis sobrantes disponibles;
- cantidades en tránsito;
- cantidades pendientes de asignación;
- vencimientos;
- recepción que originó cada existencia;
- transferencias, remitos y devoluciones posteriores.

## Alcance de esta etapa

Este circuito comprende únicamente:

1. recepción y control de las vacunas que llegan a San Cristóbal;
2. distribución y asignación a veterinarias;
3. transferencia y recepción en subcentros;
4. consulta del stock resultante por centro.

Las actas de inicio y finalización de campaña y las planillas de programación, temperatura diaria y entrega diaria corresponden a otras funciones. Permanecen relevadas, pero se diseñarán posteriormente y no forman parte de estas pantallas.
