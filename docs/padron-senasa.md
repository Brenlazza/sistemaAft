# Relevamiento del padrón SENASA

## Tabla senasa

Fuente: captura de Vista Diseño de la tabla local `senasa`. Su hoja de propiedades no muestra una ruta de tabla vinculada.

| Campo | Tipo en Access | Observación |
|---|---|---|
| renspa | Texto corto, tamaño 20 | No requerido, permite longitud cero, no indexado y sin compresión Unicode. |
| Nombre Productor | Texto corto | Nombre del productor. |
| Establecimiento | Texto corto | Nombre del establecimiento. |

No se observa clave primaria. La tabla tampoco registra fecha de importación, campaña, fuente, estado, ubicación, documento del productor ni otros atributos oficiales.

## Lectura funcional

Parece una tabla local de consulta o importación simplificada del padrón SENASA. El menú anterior ofrece «Consulta Padrón del Senasa», pero esta estructura no permite demostrar cuándo se obtuvo el padrón ni comparar formalmente versiones.

## Propuesta para el nuevo modelo

- Registrar cada importación con fuente, fecha, archivo, campaña y responsable.
- Conservar las filas originales en un área de importación sin sobrescribir cargas anteriores.
- Validar y normalizar RENSPA como texto, manteniendo también el valor original.
- Mantener un maestro canónico de clientes/productores, alimentado por importaciones o altas manuales, que sea el utilizado para facturación y operación.
- Relacionar las filas SENASA con clientes/productores y establecimientos cuando exista evidencia suficiente, conservando los casos pendientes sin bloquear su uso operativo.
- Registrar coincidencias, diferencias, altas, bajas y casos ambiguos.
- Crear una instantánea de padrón por campaña para calcular correctamente pendientes y establecimientos no vacunados.
- Evitar usar nombres como única forma de correspondencia.

La tabla propuesta `padron_campaña` debe representar qué establecimiento y productor se esperaba atender en esa campaña, manteniendo la evidencia de la importación que originó el dato y su vínculo directo con el cliente/productor canónico.

Una misma identidad canónica puede tener varios establecimientos, RENSPA o filas históricas de SENASA. Una fila pendiente u observada se conserva como referencia y genera seguimiento, pero no bloquea remitos, actas, conciliaciones ni facturación.

La definición completa se encuentra en [relación entre clientes/productores y padrón SENASA](clientes-productores-senasa.md).

La revisión diaria se realiza desde el [padrón operativo de productores y establecimientos](pantalla-productores-establecimientos.md), con acceso a la [ficha histórica del cliente/productor](pantalla-ficha-productor.md).

La carga de cada archivo se realiza mediante el [asistente de importación SENASA](pantalla-importacion-padron-senasa.md), que previsualiza y valida los datos antes de crear una nueva versión.

## Listado real recibido para la primera campaña 2026

Se relevó el archivo legado XLS `LISTADO GENERAL EXISTENCIA BOVINA PRIMERA CAMPAÑA 2026.xls` sin modificarlo. Contiene:

- 3.440 UP en la hoja principal;
- 57 UP de otros distritos con convenio;
- 3.497 RENSPA únicos en total;
- una tercera hoja de totales utilizada para conciliación, no como origen de establecimientos.

El archivo identifica al predio por RENSPA y al productor mediante `TITULAR` y `CUIT`, pero no trae nombre de establecimiento. Por ello, el nuevo importador no puede exigir ese nombre ni deducirlo. La vinculación se realiza por RENSPA y los casos nuevos quedan pendientes de completar o relacionar con el maestro.

Además de datos de identidad, el listado incluye existencias por categorías bovinas, total declarado, superficie, partido, indicador de riesgo y fechas de vacunaciones anteriores. La hoja principal agrega oficina local y fecha anteúltima; la hoja de convenios utiliza abreviaturas distintas para varias categorías. El mapeo debe ser por hoja y debe conservar los encabezados originales.

La prevalidación encontró alertas de calidad: 6 diferencias entre la suma de categorías y el total declarado, 4 filas con cantidades negativas, 11 CUIT ausentes, 95 fechas de última vacunación y 152 fechas de vacunación anteúltima faltantes. Todas las filas se conservan y pueden utilizarse; ningún valor debe corregirse automáticamente ni convertirse en cero.

Las existencias informadas por SENASA son una estimación de inicio. Nacimientos, muertes, movimientos o animales no inscriptos pueden hacer que la vacunación real sea mayor o menor. Esa variación se informa, pero no se considera por sí sola un error ni causa bloqueo.

## Pendiente

- Confirmar si SENASA entregará el mismo formato en campañas posteriores.
- Definir la frecuencia de complementos o reemplazos dentro de una campaña.
- Reglas reales de comparación con ESTABLECIMIENTOS.Ficha.
- Consultas utilizadas por «Consulta Padrón» y «Búsqueda de Diferencias».
