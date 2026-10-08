# Pantalla de movimientos de stock

## Objetivo

Explicar y auditar el saldo de cada existencia desde su recepción hasta su uso, devolución, transferencia, rotura o decomiso. La pantalla es de consulta: un movimiento confirmado no se modifica ni se elimina.

El [prototipo navegable](../prototipos/stock-movimientos.html) presenta la estructura propuesta.

## Contexto de consulta

La pantalla puede abrirse desde el stock consolidado con una combinación ya seleccionada:

`centro + veterinaria o Sin asignar + lote + condición`

También puede abrirse sin contexto para consultar todos los movimientos autorizados al usuario.

## Filtros

- período desde/hasta;
- centro;
- veterinaria o `Sin asignar`;
- marca y serie/lote;
- condición `Lleno` o `Sobrante`;
- tipo de movimiento;
- documento relacionado;
- texto libre.

## Tipos de movimiento

- recepción en depósito principal;
- asignación o reasignación a veterinaria;
- transferencia enviada;
- recepción de transferencia;
- retiro mediante remito, discriminado entre llenos y sobrantes;
- devolución de sobrantes;
- rotura o decomiso;
- ajuste autorizado;
- reversión de un movimiento anterior.

Una distribución al subcentro genera movimientos vinculados: asignación de propiedad, salida física del origen y recepción física en destino. Se muestran relacionados, pero no se fusionan en un único asiento.

## Grilla

Cada fila muestra:

- fecha y hora efectiva;
- número interno;
- tipo;
- centro;
- veterinaria/asignación;
- lote;
- condición;
- entrada;
- salida;
- saldo posterior dentro de la misma unidad;
- documento origen;
- estado;
- acción `Ver detalle`.

Los frascos y las dosis no se suman en una misma columna de saldo. Al cambiar entre `Lleno` y `Sobrante`, la unidad queda visible en el encabezado y en cada fila.

## Detalle del movimiento

Al seleccionar una fila se abre un panel con:

- fecha de registro y fecha efectiva;
- usuario responsable, según permiso;
- origen y destino;
- cantidad y unidad;
- saldo anterior y posterior;
- documento y versión que lo originó;
- movimiento relacionado;
- motivo u observación;
- autorización, si corresponde.

Desde el panel se puede abrir el documento original o imprimir una constancia. Los datos privados de auditoría no se incorporan a remitos, actas ni impresiones operativas.

## Correcciones

Un error no se corrige editando el asiento. Un usuario autorizado crea una reversión que referencia el movimiento equivocado y, si corresponde, registra después el movimiento correcto. Ambos permanecen visibles.

Toda reversión exige motivo. Los ajustes de inventario requieren autorización y nunca se usan para ocultar diferencias entre dosis retiradas, vacunadas y devueltas.

## Exportación

La consulta filtrada puede exportarse para informes. La exportación conserva unidades separadas y registra quién la generó, fecha, filtros aplicados y alcance de centros permitido.

