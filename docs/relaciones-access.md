# Relaciones observadas en Access

Fuente: captura de la ventana Relaciones aportada por el usuario. Esta vista permite leer campos y símbolos de clave, pero no sus tipos, las opciones de cascada ni todas las columnas. Algunas líneas se cruzan o terminan en campos fuera del área visible: no se reconstruyen claves foráneas por suposición.

## Campos y claves visibles

Los campos con icono de llave se registran como componentes de la clave primaria mostrada por Access. Deben verificarse en Vista Diseño antes de implementar el esquema.

| Tabla vinculada | Campos con llave visibles | Otros campos visibles |
|---|---|---|
| DISTRI-VETE1 | No se ve llave en el área mostrada | Marca, Id_UEL, Matricula, Cantidad, Fecha Entrega |
| VACUNADOR1 | Matricula | Nombre, Domicilio, Telefono, Tipo |
| VACUNA1 | Ficha; Fecha de Vacuna | Marca, ID_UEL, Acta, Matricula |
| VACUNABT1 | Ficha; Fecha de Vacuna | Marca, ID_UEL, Acta, Matricula |
| TABGEO2 | DPTO; LOCA | POSTA, NOMB, NOMD |
| EMPRESA1 | ID_EMPRE | NOMBRE |
| ESTABLECIMIENTOS1 | Ficha | Propietario, Tipo_doc, Nro_doc, Clasi, Domicilio |
| DISTRUBUCION1 | Marca; ID_UEL; Fecha Entrega | Fecha Vencimiento, Serie, Cantidad |
| UEL1 | ID_UEL | NOMBRE, Presidente, Dpto, Loca, Administrativo |

En VACUNA1, VACUNABT1, ESTABLECIMIENTOS1 y UEL1 hay más campos fuera del área visible. No se afirma que los demás listados sean esquemas completos sin inspeccionar sus diseños.

## Relaciones legibles y relaciones pendientes

| Observación | Interpretación y límite |
|---|---|
| VACUNADOR1 muestra `1` y VACUNA1 muestra `∞` en la conexión que llega a Matricula | Relación uno a muchos entre vacunador y registros de VACUNA1. Verificar el par de campos en Editar relaciones. |
| VACUNADOR1 muestra `1` y VACUNABT1 muestra `∞` en la conexión que llega a Matricula | Relación uno a muchos entre vacunador y registros de VACUNABT1. Verificar el par de campos. |
| EMPRESA1 y ESTABLECIMIENTOS1 muestran `1` en ambos extremos de una conexión | Relación uno a uno mostrada por Access. El campo destino de ESTABLECIMIENTOS1 no queda legible; no asumir que Ficha es igual a ID_EMPRE ni identificar EMPRESA con productor todavía. |
| UEL1 muestra `1` y DISTRUBUCION1 muestra `∞`, en la conexión a ID_UEL | Una UEL se vincula con múltiples registros de distribución. Verificar el par de campos y las reglas de integridad. |
| Aparecen conexiones entre TABGEO2 y registros de vacunación, y hacia el área de ESTABLECIMIENTOS1 | Hay relaciones geográficas que deben inspeccionarse individualmente. Los trazos cercanos y los campos ocultos impiden establecer todos los pares o si usan DPTO y LOCA como clave compuesta. |
| Aparecen trazos desde DISTRI-VETE1 y hacia el borde izquierdo | La vista no muestra todos los extremos con claridad. Falta confirmar su relación con vacunador y UEL mediante el diálogo de edición. |

Los símbolos `1` y `∞` expresan las cardinalidades que muestra el diagrama. La captura no documenta las opciones de actualización o eliminación en cascada. Tampoco prueba que se estén mostrando todas las relaciones del archivo original.

## Hallazgos que cambian el relevamiento

1. `ESTABLECIMIENTOS` tiene una clave visible llamada `Ficha`. RENSPA sigue siendo un identificador importante, pero no es la clave que muestra esta captura. Falta conocer el tipo de Ficha y su relación con RENSPA.
2. `VACUNA` y `VACUNABT` contienen Ficha, Fecha de Vacuna, Acta y Matricula. Esto sugiere registros de vacunación, no simples catálogos de productos. No reutilizar esos nombres con una equivalencia funcional no comprobada. La enfermedad registrada por cada tabla sigue pendiente.
3. Las claves visibles de VACUNA y VACUNABT combinan Ficha y Fecha de Vacuna. Revisar cómo se registran varias actas o visitas en una misma fecha y si la fecha incluye hora.
4. `TABGEO` muestra una clave compuesta por DPTO y LOCA. No asumir que el código de localidad es único fuera de su departamento.
5. `DISTRUBUCION` incluye Serie y Fecha Vencimiento, además de Marca, UEL y Cantidad. Relevar si Serie representa un lote y qué significa Marca en esta tabla; no confundirla automáticamente con la marca del ganado.
6. `EMPRESA` solo muestra ID_EMPRE y NOMBRE. La relación uno a uno requiere explicación antes de decidir si modela propietario, entidad administrativa u otro concepto.

## Material siguiente

- Vista Diseño de ESTABLECIMIENTOS para identificar Ficha, RENSPA y los campos truncados de las capturas anteriores.
- Diálogo Editar relaciones de EMPRESA1–ESTABLECIMIENTOS1: tablas, campos y opciones de integridad.
- Vista Diseño de VACUNA y VACUNABT y formulario de ingreso de actas para confirmar qué datos almacenan y qué enfermedad corresponde a cada una.
- Diálogos de las relaciones geográficas para confirmar los pares de campos.

Inspeccionar sin modificar las relaciones del sistema original. No se ha creado todavía un esquema de base de datos nuevo: seguimos recopilando evidencia.
