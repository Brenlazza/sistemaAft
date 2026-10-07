# Lógica funcional de actas de vacunación

## Objetivo

Definir el comportamiento del sistema desde la asignación del número hasta la confirmación de la vacunación y el consumo de stock. Esta lógica mantiene el procedimiento en papel: un juego de tres ejemplares, completado una sola vez mediante papel carbónico y cargado posteriormente en el sistema.

## Dos entidades diferentes

El sistema debe distinguir:

1. **Formulario numerado:** documento físico emitido, aunque todavía no se haya realizado la visita.
2. **Acta de vacunación:** información sanitaria transcrita del formulario después de la visita.

Esta separación permite registrar formularios impresos, entregados, dañados, extraviados o anulados sin inventar una vacunación.

## Formulario numerado

Cada formulario posee:

- identificador interno;
- número visible de ocho posiciones;
- campaña y código de plan;
- UEL emisora;
- establecimiento/RENSPA asignado;
- vacunador asignado;
- vacuna y serie/lote previstos;
- instantánea de los datos preimpresos;
- lote de impresión;
- estado actual;
- fechas y usuarios de cada operación.

Las fechas, usuarios y demás metadatos de auditoría son privados. Se conservan para consultas futuras, investigaciones y trazabilidad, pero no se imprimen en el acta ni se muestran en las pantallas operativas comunes. Su consulta queda restringida a los dos usuarios administradores con acceso completo o a un permiso específico de auditoría.

El número identifica al juego completo:

- original para la Fundación;
- duplicado para SENASA;
- triplicado para el productor.

Los tres ejemplares llevan el mismo número. No se crean tres formularios ni se consumen tres números.

## Estados del formulario

| Estado | Significado | Próximas acciones permitidas |
|---|---|---|
| Reservado | Número asignado, todavía sin documento definitivo. | Imprimir o liberar la reserva si nunca fue emitida. |
| Impreso | Juego generado con su instantánea de datos. | Entregar, reimprimir o anular. |
| Entregado | Juego recibido por el vacunador. | Registrar devolución, carga, extravío o anulación. |
| Devuelto | Papel completado o no utilizado regresó a la Fundación. | Cargar, observar o anular. |
| Cargado | Existe un acta digital vinculada. | Revisar el acta o consultar. |
| Anulado | Formulario inutilizado con motivo. | Consultar; no reutilizar. |
| Extraviado | No fue recuperado y se bloqueó su uso. | Consultar o resolver mediante procedimiento autorizado. |

Una reimpresión no cambia el número ni crea un formulario nuevo. Registra fecha, usuario, ejemplar reimpreso y motivo, e incorpora una leyenda visible de reimpresión.

## Emisión del juego

La emisión debe ejecutarse como una única operación:

1. Seleccionar campaña activa, UEL, vacunador y establecimiento.
2. Verificar que el vacunador esté habilitado.
3. Verificar que el establecimiento esté habilitado para recibir un acta en la campaña.
4. Verificar que la vacuna prevista pertenezca al stock disponible del vacunador o a una entrega confirmada.
5. Reservar el siguiente número disponible.
6. Guardar una instantánea de todos los datos que serán impresos.
7. Generar las tres páginas con geometría idéntica y destinos diferentes.
8. Registrar el lote de impresión y pasar el formulario a `Impreso`.

Si falla cualquier paso, no debe quedar un número parcialmente emitido. Si el documento ya fue generado, el número nunca vuelve a utilizarse.

## Datos preimpresos y datos de campo

Se preimprimen los datos conocidos:

- campaña, período y año;
- número de acta y rango de impresión;
- propietario, RENSPA y establecimiento;
- domicilio, ubicación, régimen, hectáreas y tipo de rodeo;
- marca del ganado cuando esté digitalizada;
- vacunador, matrícula y DNI;
- vacuna, serie/lote y vencimiento previstos;

Se completan en campo:

- lugar y fecha efectiva;
- cantidades vacunadas por categoría;
- existencias de ovinos, porcinos, caprinos, equinos y otras especies;
- brucelosis y carbunclo cuando correspondan;
- diferencias respecto de los datos preimpresos;
- observaciones;
- firmas y aclaraciones.

Cuando se utiliza otra serie/lote, el vacunador corrige el papel y explica el cambio. La carga conserva el valor preimpreso y registra por separado el valor efectivamente utilizado.

## Carga posterior

1. Buscar el formulario por número o escanear su código.
2. Mostrar bloqueada la instantánea de datos preimpresos.
3. Transcribir fecha, cantidades, correcciones, observaciones y firmas requeridas.
4. Seleccionar el remito utilizado, verificar que el productor/establecimiento sea uno de sus destinos y registrar el lote realmente aplicado.
5. Comparar totales escritos con la suma de categorías.
6. Guardar como borrador o enviar a revisión.
7. Confirmar solamente cuando todas las validaciones obligatorias se cumplan.

La operación debe ser idempotente: repetir accidentalmente el envío no puede crear dos actas ni consumir dos veces el stock.

## Estados del acta digital

| Estado | Significado |
|---|---|
| Borrador | Carga incompleta o todavía editable. |
| Observada | Requiere aclaración por inconsistencia, ilegibilidad o dato faltante. |
| Confirmada | Validada; afecta cobertura y stock. |
| Rectificada | Fue reemplazada o complementada mediante una corrección auditada. |
| Anulada | Perdió efecto mediante una operación autorizada y con motivo. |

El estado del acta no reemplaza al estado del formulario. Por ejemplo, un formulario puede estar `Cargado` y su acta digital permanecer `Observada`.

## Validaciones al confirmar

### Identificación

- formulario existente y no anulado ni extraviado;
- campaña activa o permiso especial para carga tardía;
- número todavía no asociado a otra acta;
- establecimiento y vacunador coincidentes con lo emitido, salvo corrección autorizada;
- fecha efectiva dentro de la campaña o excepción documentada.
- remito entregado al mismo profesional receptor;
- productor/establecimiento del acta incluido como destino del remito;

### Cantidades

- valores enteros y no negativos;
- total bovino igual a la suma de sus categorías;
- categorías bovinas y bubalinas previstas por el tipo de campaña;
- posibilidad de cargar excepcionalmente categorías o cantidades no previstas por la campaña, con motivo obligatorio y autorización del usuario habilitado;
- cero acompañado por un resultado o motivo cuando no hubo vacunación;
- diferencias relevantes con el padrón acompañadas por observación.

### Vacuna y stock

- producto aplicable al programa sanitario;
- serie/lote existente;
- lote no vencido en la fecha efectiva;
- lote disponible en el stock del vacunador;
- veterinaria propietaria y lote coincidentes con el remito, salvo corrección autorizada;
- dosis suficientes;
- consumo calculado una sola vez.

### Integridad documental

- lugar y fecha completos;
- firma del vacunador;
- firma del propietario o responsable, o motivo documentado de ausencia;
- correcciones legibles y justificadas;
- adjunto del acta cuando la política lo haga obligatorio.

## Efectos de la confirmación

La confirmación debe realizarse en una sola transacción:

1. Bloquear el acta para evitar confirmaciones simultáneas.
2. Crear el movimiento de consumo por lote.
3. Recalcular la suma de dosis vacunadas de todas las actas confirmadas y vigentes vinculadas al remito.
4. Actualizar el cumplimiento del establecimiento en la campaña.
5. Cambiar el acta a `Confirmada`.
6. Cambiar el formulario a `Cargado`.
7. Registrar usuario, fecha y resultado de las validaciones.

Si falla el consumo de stock o cualquier validación, no se confirma ninguna parte de la operación.

La vacuna preimpresa es una asignación prevista; imprimir el acta no consume stock. El consumo ocurre únicamente al confirmar los datos efectivamente aplicados.

## Varias visitas y vacunación parcial

Las actas parciales existen porque un establecimiento puede recibir más de una vacunación dentro de la misma campaña. Cada visita genera un acta independiente, con número, fecha efectiva, vacuna, serie/lote y consumo de stock propios.

Cada acta corresponde a un solo productor/establecimiento. Un remito puede abastecer varias actas del mismo o de distintos productores, siempre que cada uno figure entre sus destinos y el profesional receptor sea el correspondiente.

El sistema no aplica una unicidad simple por campaña y RENSPA. Controla posibles duplicaciones mediante fecha, tipo de intervención, categorías, lotes y estado, pero permite expresamente varias actas parciales para el mismo establecimiento.

El cumplimiento se calcula agregando únicamente actas confirmadas y vigentes. Una vacunación parcial mantiene el establecimiento pendiente por las categorías o cantidades restantes, según las reglas de la campaña.

## Correcciones y anulaciones

Antes de confirmar, el borrador puede editarse y conserva historial básico de cambios.

Después de confirmar no se sobrescriben silenciosamente datos ni movimientos:

- una rectificación registra motivo, usuario, fecha y valores anteriores/nuevos;
- el ajuste de cantidades genera la diferencia correspondiente en stock;
- una anulación revierte el consumo mediante un movimiento inverso vinculado;
- el número del formulario continúa ocupado;
- la cobertura se recalcula sin contar el acta anulada o reemplazada.

La decisión entre rectificación directa o anulación más acta reemplazante debe validarse con el procedimiento oficial.

## Tablas mínimas involucradas

- `series_acta`;
- `lotes_impresion_acta`;
- `formularios_acta`;
- `impresiones_acta`;
- `eventos_formulario_acta`;
- `actas_vacunacion`;
- `aplicaciones_acta`;
- `aplicaciones_detalle`;
- `rectificaciones_acta`;
- `remitos_retiro` y `remito_destinos`;
- `movimientos_stock` y `movimientos_stock_detalle`;
- `auditoria_cambios`.

## Decisiones operativas todavía necesarias

1. Unidad real de consumo: dosis, centímetros cúbicos, frascos u otra.
2. Conversión entre presentación del frasco y dosis disponibles.
3. Tolerancia permitida entre dosis consumidas y animales vacunados.
4. Procedimiento oficial para tachaduras y correcciones en papel.
5. Quién puede confirmar, rectificar y anular un acta.
6. Si el escaneo del original será obligatorio o solamente recomendado.

## Temas postergados para una etapa futura

- ámbito exacto de la numeración: global, código de plan, UEL o lote autorizado;
- autoridad que entrega los rangos;
- continuidad o reinicio de la secuencia entre campañas;
- tratamiento de rangos históricos ya utilizados.

Estas definiciones no bloquean el diseño funcional actual. Hasta resolverlas, el número se trata como un identificador visible único que nunca se reutiliza.
