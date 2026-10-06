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
- Relacionar el padrón importado con productores y establecimientos mediante un proceso de conciliación trazable.
- Registrar coincidencias, diferencias, altas, bajas y casos ambiguos.
- Crear una instantánea de padrón por campaña para calcular correctamente pendientes y establecimientos no vacunados.
- Evitar usar nombres como única forma de correspondencia.

La tabla propuesta `padron_campaña` debe representar qué establecimiento y productor se esperaba atender en esa campaña, manteniendo la evidencia de la importación que originó el dato.

## Pendiente

- Formato y procedimiento actual de importación desde SENASA.
- Cantidad de registros y frecuencia de actualización.
- Reglas reales de comparación con ESTABLECIMIENTOS.Ficha.
- Consultas utilizadas por «Consulta Padrón» y «Búsqueda de Diferencias».
