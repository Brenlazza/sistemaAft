# Relación entre clientes/productores y padrón SENASA

El maestro comercial puede alimentarse mediante la [importación del listado de clientes de facturación](importacion-clientes-facturacion.md). Esa fuente se conserva separada del padrón SENASA y se vincula por evidencia normalizada.

## Regla funcional confirmada

Una misma persona o entidad cumple dos funciones dentro del sistema:

- **cliente/productor:** maestro comercial utilizado para facturación y para el resto de la operación;
- **productor SENASA:** registro recibido al comienzo de una campaña que informa quién debe vacunar y en qué establecimiento o RENSPA.

No deben tratarse como dos identidades independientes cuando existe una correspondencia comprobable. Sin embargo, el listado SENASA es una referencia estimativa y no una autorización: un productor puede vacunarse aunque no figure en el listado, no tenga RENSPA o todavía no esté inscripto formalmente.

El maestro de clientes/productores puede alimentarse mediante importaciones y también permite crear un cliente nuevo cuando no exista. La importación SENASA se conserva por separado porque tiene fuente, campaña y valores históricos propios.

## Entidades propuestas

### `clientes_productores`

Es la identidad canónica para la operación y la facturación. Conserva, entre otros datos:

- identificador interno estable;
- nombre o razón social;
- CUIT, DNI u otro documento, cuando exista;
- domicilio y datos de contacto;
- condición comercial y datos necesarios para facturar;
- origen del alta: importación o creación manual;
- estado activo/inactivo;
- historial auditable de cambios.

Una corrección de nombre o domicilio no debe crear automáticamente otro cliente.

### `padron_senasa_importado`

Conserva cada fila tal como fue recibida, asociada con su archivo, fecha y campaña. Incluye el RENSPA, productor y establecimiento originales, aunque contengan diferencias de escritura respecto del maestro.

### `vinculos_padron_cliente`

Relaciona una fila del padrón SENASA con un cliente/productor canónico. Conserva:

- importación y fila de origen;
- campaña;
- cliente/productor vinculado;
- establecimiento/RENSPA normalizado;
- método de vinculación: automático o manual;
- estado: `Pendiente`, `Vinculado` u `Observado`;
- nivel de confianza o motivo de revisión;
- usuario y fecha, disponibles solo para auditoría privada.

## Cardinalidad y obligatoriedad

- Un cliente/productor puede estar relacionado con varios RENSPA, establecimientos o filas históricas de SENASA.
- Una fila SENASA puede permanecer sin vínculo. Si se vincula, solo puede apuntar a un cliente/productor activo a la vez.
- Una fila pendiente u observada se conserva y puede utilizarse como referencia sin impedir remitos, vacunación o carga de actas.
- Si no existe una coincidencia válida, el operador puede utilizar o crear un cliente/productor interno y completar el vínculo más adelante.
- Los valores originales de SENASA nunca se sobrescriben al vincular o normalizar.
- Un productor nuevo puede tener identificador interno sin CUIT, DNI ni RENSPA. Esos datos quedan como no informados y podrán completarse después.

## Proceso de conciliación

1. Importar el archivo SENASA y conservar todas sus filas originales.
2. Buscar coincidencias por identificadores confiables, especialmente CUIT/DNI y RENSPA cuando estén disponibles.
3. Usar nombres normalizados únicamente como ayuda; no confirmar automáticamente una coincidencia ambigua solo por similitud de nombre.
4. Presentar al operador los casos pendientes, duplicados o contradictorios.
5. Vincular con un cliente/productor existente o crear uno nuevo cuando resulte útil para evitar duplicados.
6. Incorporar el registro al padrón operativo con el estado informativo correspondiente, sin bloquear la vacunación.

## Uso en remitos, actas y facturación

Al seleccionar un destino para un remito, el sistema puede utilizar el vínculo completo cuando existe:

- cliente/productor canónico;
- registro del padrón de la campaña;
- establecimiento y RENSPA.

El cliente/productor canónico se utiliza para facturación. El registro SENASA y el establecimiento se utilizan para controlar obligación, cobertura y cumplimiento sanitario. El acta conserva ambos identificadores para que la relación histórica siga siendo reproducible.

Si el productor no figura en SENASA o carece de RENSPA, el remito y el acta utilizan su identificador interno y dejan esos campos como `No informado`. La ausencia no impide la operación ni debe completarse con un valor ficticio.

Un remito puede incluir uno o más destinos vinculados. Cada acta corresponde a un único productor y establecimiento, pero varias actas de distintos productores pueden consumir vacuna del mismo remito.

## Controles contra duplicados

- advertir coincidencias de CUIT, DNI, RENSPA o combinaciones equivalentes;
- permitir unificación solo mediante una operación autorizada y auditable;
- impedir que una misma fila SENASA quede vinculada simultáneamente con dos clientes/productores activos;
- conservar alias y valores históricos para reconocer futuras importaciones;
- no eliminar físicamente un cliente que tenga facturas, actas, remitos o vínculos históricos.

## Informes mínimos

- filas SENASA vinculadas, pendientes y observadas;
- clientes nuevos creados durante la conciliación;
- posibles clientes duplicados;
- productores del padrón sin establecimiento válido;
- establecimientos y obligaciones SENASA por cliente/productor;
- trazabilidad desde la factura y el acta hasta la fila SENASA de origen.

La operación propuesta se desarrolla en el [padrón de productores y establecimientos](pantalla-productores-establecimientos.md) y en la [ficha del cliente/productor](pantalla-ficha-productor.md).

