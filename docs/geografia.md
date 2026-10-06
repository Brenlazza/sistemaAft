# Relevamiento geográfico

## TABGEO

Fuente: captura de Vista Diseño de `TABGEO2`. Origen confirmado: `D:\DiscoDpc2\aftosa 31\datos.mdb`, tabla `TABGEO`.

| Campo | Tipo en Access | Clave | Descripción original |
|---|---|---|---|
| DPTO | Número, Entero | Sí | Código de departamento. |
| LOCA | Número | Sí | Código de Distrito. Subtipo pendiente. |
| POSTA | Texto corto | No | Código postal. |
| NOMB | Texto corto | No | Nombre del departamento o localidad. |
| NOMD | Texto corto | No | Nombre departamento. |

La clave primaria es compuesta: `(DPTO, LOCA)`. Para DPTO se observan además título `Cod.Departamento`, valor predeterminado 0 y condición Requerido.

## Uso observado

El diagrama de relaciones muestra dos vínculos desde TABGEO hacia ESTABLECIMIENTOS, compatibles con:

- `TABGEO.DPTO` → `ESTABLECIMIENTOS.Departamento`
- `TABGEO.LOCA` → `ESTABLECIMIENTOS.Distrito`

Los pares exactos deben confirmarse en Editar relaciones, pero los nombres y descripciones respaldan esta correspondencia. UEL también contiene Dpto y Loca, aunque no se observó claramente una restricción formal hacia TABGEO.

La tabla repite `NOMD` para cada distrito/localidad del mismo departamento. `NOMB` es ambiguo porque su descripción admite tanto departamento como localidad, y `LOCA` se describe como distrito.

## Propuesta para el nuevo modelo

- Crear catálogos separados de provincias, departamentos y localidades/distritos con claves internas.
- Conservar DPTO y LOCA como códigos históricos para migración e interoperabilidad.
- Definir explícitamente si LOCA representa distrito, localidad o ambos según los registros reales.
- Evitar repetir el nombre del departamento en cada localidad.
- Guardar el código postal como texto para preservar ceros iniciales y formatos futuros.
- Relacionar formalmente establecimientos y UEL con la ubicación correspondiente.

## Pendiente

- Vista de datos para comprobar la semántica de LOCA, NOMB y NOMD.
- Tamaños de POSTA, NOMB y NOMD.
- Confirmación de los pares exactos de la relación con ESTABLECIMIENTOS.
- Provincia asociada a los códigos y tratamiento de registros fuera de Santa Fe.
