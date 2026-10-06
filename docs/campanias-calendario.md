# Relevamiento de campañas y calendario

## Tabla semanas

Fuente: capturas de Vista Diseño y hoja de datos de la tabla local `semanas`.

| Campo | Tipo en Access | Observación |
|---|---|---|
| año | Texto corto | En la captura aparece tamaño 50; no requerido ni indexado. |
| mes | Texto corto | Nombre del mes. |
| semana | Texto corto | Número o etiqueta de semana dentro del mes. |
| fechad | Fecha/Hora | Fecha inicial. La tabla se ordena por este campo. |
| fecgah | Fecha/Hora | Fecha final; el nombre parece contener un error tipográfico. |

No se observa clave primaria. En la hoja de datos, los períodos se dividen dentro de cada mes y pueden incluir semanas parciales al inicio o final del mes. No deben reemplazarse automáticamente por semanas ISO.

## Interpretación

`semanas` funciona como calendario de períodos para consultas o informes. No representa por sí sola una campaña: no contiene tipo de campaña, estado, enfermedad, población objetivo ni relación con actas.

## Propuesta

Crear `campañas` como entidad principal y, solo cuando el negocio lo requiera, `periodos_campaña` con:

- campaña;
- número o etiqueta;
- fecha desde y hasta;
- tipo de período;
- orden;
- estado de cierre.

Los períodos deben validarse para evitar superposiciones dentro de la misma campaña. Año y mes se derivarán de las fechas para informes, salvo que exista una regla administrativa que requiera conservar etiquetas específicas.
