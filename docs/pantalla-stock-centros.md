# Pantalla de stock por centros

## Objetivo

Consultar el stock físico y disponible de la UEL principal y los cinco subcentros, separado por veterinaria, lote y condición. La pantalla no permite escribir un saldo: todos los valores se calculan desde movimientos confirmados.

El [prototipo navegable](../prototipos/stock-centros.html) muestra la distribución propuesta.

## Alcance

Incluye únicamente el stock actual y sus movimientos relacionados:

- frascos llenos;
- dosis sobrantes;
- existencias sin asignar;
- transferencias preparadas o en tránsito;
- roturas y decomisos registrados en cada centro;
- acceso a recepciones, distribuciones, transferencias, remitos y devoluciones.

Las planillas diarias y las actas generales de campaña pertenecen a otros circuitos.

## Filtros

- centro: todos, San Cristóbal o uno de los cinco subcentros;
- veterinaria, incluyendo `Sin asignar`;
- serie/lote;
- marca;
- condición: lleno o sobrante;
- estado: disponible, reservado o en tránsito;
- vencimiento;
- texto libre.

Los filtros no cambian el saldo: solamente modifican la consulta.

## Resumen superior

Las tarjetas muestran unidades sin mezclarlas:

- frascos llenos disponibles;
- dosis equivalentes de esos frascos;
- dosis sobrantes disponibles;
- frascos sin asignar;
- frascos o dosis en tránsito;
- alertas de vencimiento o diferencias.

No se suma un frasco lleno con una dosis sobrante como si fueran la misma unidad. Cuando se necesita comparar, el sistema muestra también el equivalente en dosis.

## Navegación por centros

Una columna lateral presenta:

- consolidado general;
- depósito principal San Cristóbal;
- los cinco subcentros;
- cantidad de veterinarias activas en cada centro;
- alertas y transferencias pendientes.

Seleccionar un centro actualiza las tarjetas y la grilla sin ocultar el acceso al consolidado.

## Grilla de existencias

Cada fila representa una combinación de:

`centro + veterinaria o Sin asignar + lote + vencimiento + condición`

Columnas:

- centro;
- veterinaria/asignación;
- marca;
- serie/lote;
- vencimiento;
- condición;
- cantidad y unidad;
- equivalente en dosis;
- reservado;
- disponible;
- estado o alerta;
- acción `Ver movimientos`.

En stock `Lleno`, cantidad y disponible se expresan en frascos enteros. En `Sobrante`, se expresan en dosis.

## Cálculo de disponibilidad

`disponible = saldo físico confirmado - cantidad reservada`

Una transferencia `Preparada` reserva stock en el origen. Cuando pasa a `En tránsito`, deja de estar físicamente disponible en el origen y se muestra en la categoría tránsito. Recién al confirmarse como `Recibida` queda disponible en el destino.

Un remito en borrador no reserva stock. Si más adelante se decide incorporar reservas para remitos autorizados, deberá quedar expresamente identificado y sin duplicar la reserva de una transferencia.

## Detalle de movimientos

Al abrir una fila se muestra un historial cronológico con:

- fecha y hora efectiva;
- tipo de movimiento;
- entrada o salida;
- cantidad y unidad;
- saldo posterior;
- documento origen;
- centro de origen y destino;
- usuario responsable, solo con permiso;
- observación o motivo.

Desde cada movimiento se puede abrir su recepción, distribución, transferencia, remito, devolución, rotura, decomiso, ajuste o reversión. Los movimientos confirmados no se editan desde esta pantalla.

## Transferencias pendientes

Un panel separado muestra:

- número de transferencia;
- origen y destino;
- veterinaria;
- lote;
- envases/dosis;
- salida;
- temperatura de entrega;
- estado;
- tiempo pendiente de recepción.

Acciones según permiso: `Abrir`, `Registrar recepción`, `Imprimir acta` e `Informar diferencia`.

Para un usuario de subcentro, el panel muestra únicamente las transferencias dirigidas a sus centros. `Registrar recepción` abre el [control físico de recepción](pantalla-recepcion-transferencia-subcentro.md); el stock se habilita localmente solo después de aceptarlo.

## Alertas

- lote vencido: no disponible para remitos;
- vencimiento próximo;
- transferencia pendiente de recepción;
- saldo bajo cero: error crítico que bloquea nuevas salidas;
- diferencia informada por un subcentro;
- stock sin asignar;
- sobrante pendiente de criterio sanitario por antigüedad.

## Acciones principales

- `Nueva recepción`;
- `Distribuir stock sin asignar`;
- `Nueva transferencia`;
- `Registrar rotura o decomiso`;
- `Exportar consulta`;
- `Ver todos los movimientos`.

La rotura o decomiso exige centro, veterinaria/asignación, lote, cantidad, motivo y autorización. Nunca se utiliza para cerrar diferencias de un remito.

## Permisos

- consultar stock general;
- consultar solamente centros asignados;
- ver movimientos y documentos;
- registrar recepción;
- distribuir o transferir;
- recibir transferencias;
- registrar rotura/decomiso;
- autorizar ajustes o reversiones;
- exportar información.
