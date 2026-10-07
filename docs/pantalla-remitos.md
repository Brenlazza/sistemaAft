# Pantalla de remitos de vacuna

## Objetivo

Registrar en una sola operación la autorización de una veterinaria, la entrega de vacuna a un profesional, uno o más productores/establecimientos de destino y la posterior conciliación con actas y devoluciones.

La pantalla representa un remito principal. No se crea un remito separado por productor cuando la vacuna proviene del mismo retiro.

Existe un [prototipo navegable de la pantalla](../prototipos/remito-vacunas.html) para revisar la distribución, la incorporación de destinos y el cálculo automático de frascos y dosis. Su acción `Vista previa / imprimir` abre el [modelo imprimible A4](../prototipos/remito-vacunas-impresion.html), basado en el remito de referencia utilizado actualmente.

## Vistas del módulo

### Bandeja de remitos

La entrada al módulo muestra una grilla con:

- número de remito;
- fecha de retiro;
- estado;
- campaña y UEL;
- veterinaria propietaria;
- profesional receptor;
- cantidad de destinos;
- frascos y dosis retiradas;
- dosis conciliadas;
- diferencia;
- alertas de firma, documentación o vencimiento.

Filtros: texto libre, número, campaña, UEL, veterinaria, profesional, productor/RENSPA, lote, fecha y estado. La búsqueda por productor debe encontrar el remito aunque tenga varios destinos.

Acciones principales: `Nuevo remito`, `Abrir`, `Imprimir`, `Registrar devolución` y `Conciliar`, según estado y permisos.

## Pantalla de alta y detalle

### Cabecera fija

Siempre visible:

- número de remito, asignado por el sistema;
- estado;
- campaña;
- UEL de salida;
- fecha prevista o efectiva del retiro;
- usuario y fecha técnica, solo en auditoría privada.

El número se muestra como `Pendiente` mientras el registro aún no haya sido guardado.

### 1. Veterinaria y autorización

Campos:

- veterinaria propietaria, obligatoria;
- saldo total disponible de esa veterinaria en la UEL;
- número o referencia de autorización;
- fecha de autorización;
- documento adjunto, opcional hasta definir si será obligatorio;
- observación de la autorización.

Al elegir la veterinaria, los lotes disponibles se restringen a existencias llenas de esa propietaria y UEL.

### 2. Profesional receptor

Campos:

- vacunador/veterinario que retira;
- matrícula;
- tipo: veterinario o idóneo;
- documento;
- estado de habilitación;
- teléfono, solo como dato de consulta;
- firma de recepción y fecha/hora de entrega.

No puede confirmarse la entrega si el profesional está inhabilitado o no se registra su firma.

### 3. Destinos del remito

Grilla repetible con una fila por destino:

| Campo | Comportamiento |
|---|---|
| Cliente/productor | Se busca en el maestro canónico utilizado para facturación. |
| RENSPA | Se elige entre los vínculos SENASA resueltos para la campaña. |
| Establecimiento | Se completa desde el vínculo seleccionado. |
| Estado SENASA | Debe ser `Vinculado`; un caso pendiente u observado bloquea la entrega. |
| Dosis previstas | Entero no negativo; sirve para calcular una sugerencia, pero no consume stock. |
| Observación | Opcional, propia del destino. |

Acciones por fila: `Ver productor`, `Cambiar`, `Quitar`. Acción general: `Agregar productor/establecimiento`.

Reglas:

- debe existir al menos un destino;
- se admiten varios productores y establecimientos;
- no se repite el mismo vínculo de campaña y establecimiento dentro del remito;
- el total previsto es la suma de las filas;
- la cantidad sugerida de frascos es el redondeo hacia arriba del total previsto dividido por 125;
- cambiar un destino no modifica el maestro ni el padrón;
- para resolver un vínculo SENASA pendiente se abre el módulo de conciliación correspondiente.

### 4. Vacuna a entregar

Grilla con una fila por serie/lote:

| Campo | Comportamiento |
|---|---|
| Marca | Informativa, proveniente del lote. |
| Serie/lote | Solo lotes llenos, vigentes y pertenecientes a la veterinaria seleccionada. |
| Vencimiento | Informativo, con alerta de proximidad. |
| Stock disponible | Frascos llenos disponibles en la UEL. |
| Frascos a retirar | Entero positivo y no superior al saldo. |
| Dosis teóricas | Calculadas como frascos × 125. |

Se permite más de un lote en un remito. El total de dosis retiradas es la suma de todos sus detalles.

La pantalla compara dosis previstas y dosis teóricas. Una diferencia en esta etapa genera una advertencia, no reemplaza la conciliación real posterior. Si se retiran menos dosis que las previstas o una cantidad superior a la sugerida, se solicita un motivo.

### 5. Resumen antes de entregar

Tarjetas visibles:

- destinos seleccionados;
- dosis previstas;
- frascos a retirar;
- dosis teóricas retiradas;
- diferencia entre previsión y retiro;
- alertas pendientes.

Acciones:

- `Guardar borrador`;
- `Registrar autorización`;
- `Vista previa / imprimir`;
- `Confirmar entrega`;
- `Anular`, con motivo y permiso.

`Confirmar entrega` abre una revisión final y requiere:

- veterinaria y autorización;
- profesional habilitado;
- al menos un destino SENASA vinculado;
- al menos un lote con stock suficiente;
- fecha efectiva;
- firma del receptor.

La confirmación genera el movimiento de salida y cambia el estado a `Entregado` en una única transacción.

## Pantalla después de la entrega

Veterinaria, profesional, UEL, lotes y cantidades retiradas quedan bloqueados. Se habilitan las pestañas siguientes.

### Actas vinculadas

Grilla:

- número y fecha del acta;
- productor, RENSPA y establecimiento;
- lote utilizado;
- dosis vacunadas;
- estado del acta;
- alerta por destino o lote inconsistente.

Solo las actas `Confirmadas` y vigentes suman a la conciliación. Desde esta grilla se puede abrir el acta, pero no modificarla silenciosamente.

### Devoluciones

Permite registrar:

- frascos llenos sin abrir, por lote;
- dosis sobrantes de frascos abiertos, vinculadas al acta y productor donde se originaron;
- fecha y responsable que recibe en la UEL;
- observaciones.

No ofrece opciones de rotura ni decomiso. Esas bajas se cargan desde el módulo de stock UEL.

### Conciliación

Resumen calculado:

| Concepto | Cálculo |
|---|---|
| Dosis retiradas | Suma de frascos entregados × 125. |
| Dosis vacunadas | Suma de actas confirmadas y vigentes. |
| Devolución llena | Frascos llenos devueltos × 125. |
| Sobrantes | Suma de dosis abiertas devueltas. |
| Diferencia | Retiradas − vacunadas − devolución llena − sobrantes. |

Si la diferencia es cero, el remito queda `Cuadrado`. Un usuario autorizado revisa firmas y documentación y ejecuta `Cerrar conciliación`, pasando a `Conciliado`.

Si la diferencia no es cero:

- se muestra en rojo;
- el estado queda `Observado`;
- se exige motivo y detalle;
- no se ofrece editar el resultado calculado;
- una resolución autorizada genera un movimiento o registro adicional auditable.

## Incorporación posterior de destinos

Después de entregar el remito puede agregarse un productor no previsto. La pantalla ofrecerá `Agregar destino posterior` antes del cierre de la conciliación y exigirá:

- campaña compatible;
- vínculo cliente/productor–SENASA resuelto;
- motivo obligatorio;
- usuario con permiso;
- constancia de que el destino no figuraba en la impresión anterior.

Al confirmar la incorporación:

1. se agrega el nuevo destino al mismo remito;
2. se incrementa el número de versión del documento;
3. se conserva la versión anterior y su firma para auditoría;
4. el remito vuelve a estado `Pendiente de nueva firma`;
5. se imprime nuevamente el documento completo, incluyendo todos los destinos;
6. la nueva impresión muestra `REIMPRESIÓN MODIFICADA` y su número de versión;
7. el profesional receptor vuelve a firmar;
8. al registrar la nueva firma, el remito recupera su estado operativo anterior.

La nueva impresión reemplaza a la anterior como versión vigente, pero la versión previa nunca se borra. Las actas ya vinculadas continúan asociadas al mismo remito.

## Estados y edición

| Estado | Edición permitida |
|---|---|
| Borrador | Todos los datos operativos. |
| Autorizado | Destinos y lotes, mientras no exista entrega. |
| Entregado | Solo actas, devoluciones, observaciones y ampliaciones autorizadas. |
| Pendiente de nueva firma | Se modificaron destinos; permite imprimir y registrar la nueva firma, pero no cerrar la conciliación. |
| Cuadrado | Revisión documental; no altera cantidades originales. |
| Observado | Carga de explicación y resolución autorizada. |
| Conciliado | Solo consulta; cualquier corrección usa un procedimiento auditado. |
| Anulado | Solo consulta. |

## Documento impreso

El documento se genera en tamaño A4 vertical y debe conservar una presentación simple semejante al remito actual: identificación institucional y del comprobante en el encabezado, datos de las partes, detalle tabular, observaciones y firmas al pie.

El remito debe mostrar:

- número, campaña, UEL y fecha;
- número de versión y, cuando corresponda, leyenda `REIMPRESIÓN MODIFICADA`;
- veterinaria propietaria y referencia de autorización;
- profesional receptor y matrícula;
- todos los destinos previstos, continuando en una hoja anexa si no entran;
- marca, lote, vencimiento, frascos y dosis teóricas;
- total general;
- firma y aclaración del receptor;
- firma del responsable de entrega en la UEL.

Los datos privados de auditoría no se imprimen.

Cuando la cantidad de destinos o lotes no entre en una página, el sistema genera páginas adicionales con el mismo número de remito, encabezados de tabla repetidos y numeración `Página X de Y`. No debe reducir la tipografía hasta volver ilegible el documento.

Cada impresión debe registrar de manera privada versión, fecha, usuario, motivo y contenido emitido. Al imprimir una modificación, todas sus páginas pertenecen a la misma versión.

## Permisos específicos

- consultar remitos;
- crear o modificar borradores;
- registrar autorización;
- confirmar entrega;
- imprimir o reimprimir;
- registrar devolución;
- agregar destino posterior;
- revisar diferencia;
- cerrar conciliación;
- anular.

Los coordinadores solo reciben las acciones y campañas/UEL asignadas. Los ajustes y cierres observados requieren permiso adicional.
