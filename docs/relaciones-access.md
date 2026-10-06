# Relaciones observadas en Access

Fuente: capturas de la ventana Relaciones aportadas por el usuario. La segunda vista ampliada permite leer todos los campos de varias tablas y más cardinalidades, pero no sus tipos ni las opciones de integridad y cascada. Algunas líneas todavía se cruzan: los pares que no son inequívocos permanecen pendientes.

## Campos y claves visibles

Los campos con icono de llave se registran como componentes de la clave primaria mostrada por Access. Deben verificarse en Vista Diseño antes de implementar el esquema.

| Tabla vinculada | Campos con llave visibles | Otros campos visibles |
|---|---|---|
| DISTRI-VETE1 | No se ve llave en el área mostrada | Marca, Id_UEL, Matricula, Cantidad, Fecha Entrega |
| VACUNADOR1 | Matricula | Nombre, Domicilio, Telefono, Tipo |
| VACUNA1 | Ficha; Fecha de Vacunación | Marca, ID_UEL, Acta, Matricula, Canti_Par, Fecha Recep, idVeterinario |
| VACUNABT1 | Ficha; Fecha de Vacunación | Marca, ID_UEL, Acta, Matricula, Canti_Par, Razón |
| TABGEO2 | DPTO; LOCA | POSTA, NOMB, NOMD |
| EMPRESA1 | ID_EMPRE | NOMBRE |
| ESTABLECIMIENTOS1 | Ficha | Propietario, Tipo_doc, Nro_doc, Clasi, Domicilio, Localidad, Telefono, Boleto, Establecimiento, Departamento, Distrito, Explotación, Explotacion2, Vacas, Vaquillonas, Toros, Toritos, Terneros, Terneras, Novillos, Novillitos, Búfalos_may, Bufalos_men, Marca, Estado Sanitario B, Estado Sanitario T, Regimen, Has, Caprinos, Ovinos, Porcinos, Equinos, Otros, Otros_aclarar, Provincia, ID_EMPRE, Fgarrapata, Presencia |
| DISTRUBUCION1 | Marca; ID_UEL; Fecha Entrega | Fecha Vencimiento, Serie, Cantidad |
| UEL1 | ID_UEL | NOMBRE, Presidente, Dpto, Loca, Administrativo, Domicilio, Telefono, Mail, Perso-juri, nro-afip, Equipa, Soft-Base, Soft-Minis, Oficina |

La vista ampliada muestra completos los cuadros de ESTABLECIMIENTOS1 y UEL1 y, aparentemente, los de las demás tablas incluidas. Vista Diseño sigue siendo necesaria para conocer tipos, tamaños, índices, valores predeterminados y reglas de validación.

## Relaciones legibles y relaciones pendientes

| Observación | Interpretación y límite |
|---|---|
| VACUNADOR1 muestra `1` y VACUNA1 muestra `∞` | Relación uno a muchos entre vacunador y registros de VACUNA1; por la disposición de los trazos parece usar Matricula, pendiente de confirmar en Editar relaciones. |
| VACUNADOR1 muestra `1` y VACUNABT1 muestra `∞` | Relación uno a muchos entre vacunador y registros de VACUNABT1; parece usar Matricula, pendiente de confirmación. |
| ESTABLECIMIENTOS1 muestra `1` y VACUNA1 muestra `∞` | Un establecimiento se vincula con múltiples registros de VACUNA1. La línea parte de Ficha en ambos cuadros según la vista; confirmar en Editar relaciones. |
| ESTABLECIMIENTOS1 muestra `1` y VACUNABT1 muestra `∞` | Un establecimiento se vincula con múltiples registros de VACUNABT1. La línea parece vincular Ficha con Ficha; confirmar. |
| EMPRESA1 muestra `1` y ESTABLECIMIENTOS1 muestra `∞` | Una empresa se vincula con múltiples establecimientos. La línea llega a la zona de ID_EMPRE en ESTABLECIMIENTOS1, coherente con ID_EMPRE en EMPRESA1; confirmar el par exacto. |
| UEL1 muestra `1` y DISTRUBUCION1 muestra `∞`, en la conexión a ID_UEL | Una UEL se vincula con múltiples registros de distribución. Verificar el par de campos y las reglas de integridad. |
| TABGEO2 muestra dos símbolos `1` y ESTABLECIMIENTOS1 dos símbolos `∞` | La vista es consistente con una relación por la clave compuesta DPTO + LOCA hacia Departamento + Distrito. Confirmar la correspondencia y el orden exactos en Editar relaciones. |
| Aparecen trazos desde DISTRI-VETE1 y hacia el borde izquierdo | La vista no muestra todos los extremos con claridad. Falta confirmar su relación con vacunador y UEL mediante el diálogo de edición. |

Los símbolos `1` y `∞` expresan las cardinalidades que muestra el diagrama. La captura no documenta las opciones de actualización o eliminación en cascada. Tampoco prueba que se estén mostrando todas las relaciones del archivo original.

## Hallazgos que cambian el relevamiento

1. `ESTABLECIMIENTOS` tiene una clave visible llamada `Ficha`. RENSPA sigue siendo un identificador importante, pero no es la clave que muestra esta captura. Falta conocer el tipo de Ficha y su relación con RENSPA.
2. `VACUNA` y `VACUNABT` contienen Ficha, Fecha de Vacunación, Acta y Matricula. Vista Diseño confirma sus tipos y la clave compuesta; ver [relevamiento de las tablas de vacunación](vacunaciones-access.md). Son registros de vacunación, no simples catálogos de productos. La enfermedad o proceso registrado por cada tabla sigue pendiente.
3. Las claves visibles de VACUNA y VACUNABT combinan Ficha y Fecha de Vacunación. Revisar cómo se registran varias actas o visitas en una misma fecha y si la fecha incluye hora.
4. `TABGEO` muestra una clave compuesta por DPTO y LOCA. No asumir que el código de localidad es único fuera de su departamento.
5. `DISTRUBUCION` incluye Serie y Fecha Vencimiento, además de Marca, UEL y Cantidad. Relevar si Serie representa un lote y qué significa Marca en esta tabla; no confundirla automáticamente con la marca del ganado.
6. `EMPRESA` solo muestra ID_EMPRE y NOMBRE y se relaciona uno a muchos con ESTABLECIMIENTOS. Esto sugiere una agrupación de establecimientos, pero no define todavía si representa una organización, delegación u otro concepto.
7. `Canti_Par` aparece tanto en VACUNA como en VACUNABT. `Fecha Recep` e `idVeterinario` solo se ven en VACUNA, mientras `Razón` solo se ve en VACUNABT. Se necesita el formulario de actas para interpretar estos campos.
8. UEL contiene datos institucionales, contacto, identificación fiscal, equipamiento, software y oficina. Conviene separar estos conceptos en el nuevo modelo si siguen siendo operativos.

## Material siguiente

- Vista Diseño de ESTABLECIMIENTOS para identificar tipos y confirmar si la etiqueta RENSPA de la hoja corresponde al campo Ficha.
- Diálogo Editar relaciones de EMPRESA1–ESTABLECIMIENTOS1: confirmar ID_EMPRE, integridad referencial y opciones de cascada.
- Formulario de ingreso de actas y datos de ejemplo anonimizados para confirmar qué proceso representa VACUNA y cuál VACUNABT.
- Diálogos de las relaciones geográficas para confirmar los pares de campos.

Inspeccionar sin modificar las relaciones del sistema original. No se ha creado todavía un esquema de base de datos nuevo: seguimos recopilando evidencia.
