# Relevamiento de ESTABLECIMIENTOS

## Evidencia disponible

Tres capturas adicionales muestran la hoja de datos de `ESTABLECIMIENTOS1`, desplazada horizontalmente, y el administrador de tablas vinculadas. No muestran Vista Diseño: aún no conocemos los tipos declarados, las claves, los índices ni las restricciones.

Actualización: una vista ampliada de Relaciones muestra `Ficha` con icono de clave y permite leer la lista completa de campos de ESTABLECIMIENTOS1. Ver [claves y relaciones observadas](relaciones-access.md). Los tipos e índices siguen pendientes de Vista Diseño.

El contador de Access muestra 2077 registros. Es el volumen indicado en esa vista, no un recuento independiente del archivo original. No se transcriben datos personales de las filas.

## Origen de datos confirmado

El administrador muestra dos orígenes Access:

- `C:\Aftosa31\datos.mdb`, cuyo grupo está contraído.
- `D:\DiscoDpc2\aftosa 31\datos.mdb`, cuyo grupo está expandido.

En el segundo origen se observan las siguientes correspondencias entre el nombre local del vínculo y la tabla del archivo de origen:

| Vínculo local | Tabla de origen |
|---|---|
| DISTRI-VETE1 | DISTRI-VETE |
| DISTRUBUCION1 | DISTRUBUCION |
| EMPRESA1 | EMPRESA |
| EPIDEMIA | EPIDEMIA |
| ESTABLECIMIENTOS1 | ESTABLECIMIENTOS |
| LABORATORIO1 | LABORATORIO |
| TABGEO2 | TABGEO |
| UEL1 | UEL |
| VACUNA1 | VACUNA |
| VACUNABT1 | VACUNABT |
| VACUNADOR1 | VACUNADOR |

El sufijo local no demuestra una versión de campaña ni una tabla distinta en el origen. No fusionar los dos archivos ni asumir que contienen los mismos datos. Falta identificar cuál es la fuente operativa vigente y comprobar su esquema y relaciones. Las rutas son referencias del equipo original, no rutas disponibles en el entorno en la nube.

## Columnas observadas

La vista ampliada de Relaciones resolvió los nombres internos que antes aparecían cortados en la hoja de datos. El encabezado mostrado al usuario puede ser una propiedad Caption distinta del nombre interno. El contenido visible ayuda a interpretar la función, pero no acredita el tipo de datos declarado en Access.

| Encabezado visible | Observación y pendiente |
|---|---|
| Ficha (mostrado como Renspa en la hoja) | Clave visible en Relaciones. Los valores se muestran con puntos y barra. Confirmar en Vista Diseño si `Renspa` es la etiqueta de presentación de Ficha. Conservar como texto con su formato durante la migración. |
| Propietario | Nombre de persona o razón social. |
| Tipo de Doc | Se observan CUIT, CUIL, DNI y celdas vacías. Confirmar catálogo y reglas. |
| Nro_doc | Identificación documental; se observan formatos con y sin separadores. Conservar el valor original y validar según tipo, sin convertirlo a número. |
| Clasi | No se distingue una clasificación informada en las filas visibles. Confirmar finalidad y valores. |
| Domicilio | La hoja lo presenta como domicilio del propietario. No confundir con ubicación del predio. |
| Localidad | La hoja parece presentarlo como localidad del propietario; confirmar. |
| Telefono | Contacto. Tratar como texto en la propuesta de modelo. |
| Boleto | Se observan números y ceros; confirmar si corresponde a boleto de marca y qué significa cero. |
| Establecimiento | Nombre del establecimiento. |
| Departamento | Se observan códigos numéricos; se relacionaría con DPTO de TABGEO. Confirmar. |
| Distrito | Se observan códigos numéricos; falta catálogo. |
| Explotación | Se observan Cría, Invernada y Tambo. |
| Explotacion2 | Segunda columna de explotación; se observan Invernada, Tambo y vacíos. Confirmar si representa actividad secundaria. |
| Vacas | Cantidades visibles. |
| Vaquillonas | Cantidades visibles. |
| Toros | Cantidades visibles. |
| Toritos | Categoría visible; las cantidades no se distinguen en el área mostrada. |
| Terneros | Cantidades visibles. |
| Terneras | Cantidades visibles. |
| Novillos | Cantidades visibles. |
| Novillitos | Cantidades visibles. |
| Búfalos_may | Categoría cuyo sufijo parece indicar mayores; confirmar significado y límite de edad. |
| Bufalos_men | Categoría cuyo sufijo parece indicar menores; confirmar significado y límite de edad. |
| Marca | Se observan Marca a Fuego, Señal y Ninguna. Confirmar si describe el método de identificación. |
| Estado Sanitario B | Se observan Sin Datos, En Saneamiento y Libre. Probablemente refiere a brucelosis, pendiente confirmación. |
| Estado Sanitario T | Se observan valores similares. Probablemente refiere a tuberculosis, pendiente confirmación. |
| Regimen | Se observan Propietario, Arrendatario y Pastajero. Confirmar si se aplica al vínculo productor–predio. |
| Has | Cantidad; probablemente hectáreas por abreviatura, pendiente confirmación. |
| Caprinos | Cantidades visibles. |
| Ovinos | Cantidades visibles. |
| Porcinos | Cantidades visibles. |
| Equinos | Cantidades visibles. |
| Otros | Cantidades visibles; confirmar qué especies incluye. |
| Otros_aclarar | Texto o código para especificar Otros; confirmar tipo. |
| Provincia | Nombres de provincias. Confirmar si corresponde al domicilio del propietario o al establecimiento. |
| ID_EMPRE | Referencia aparente a EMPRESA.ID_EMPRE en una relación uno a muchos. Se observan ceros; confirmar si cero es un valor especial o un registro válido. |
| Fgarrapata | Se observan fechas y vacíos; confirmar si registra una fecha relacionada con garrapata. |
| Presencia | Casillas marcadas y desmarcadas; confirmar de qué presencia se trata. No interpretar como vacunación realizada. |

La vista de Relaciones muestra el cuadro completo, aunque Vista Diseño debe confirmar que no existan particularidades no representadas. Los signos «+» de las filas indican subhojas expandibles, pero no identifican por sí solos qué relación muestra cada una.

## Consecuencias para el nuevo modelo (propuestas)

- Separar los datos de contacto e identificación del productor de los datos físicos del establecimiento, una vez confirmada la identidad de cada entidad.
- Relevar el vínculo productor–establecimiento, su régimen y su vigencia. La repetición de nombres de predio y variantes de RENSPA justifica investigarlo, pero no demuestra por sí sola que los registros sean duplicados o pertenezcan al mismo predio.
- Proponer una tabla `existencias_animales` con referencia al registro productor–establecimiento que corresponda, fecha de relevamiento, campaña cuando aplique, categoría y cantidad. Las existencias del padrón deben distinguirse de los animales efectivamente vacunados en un acta.
- Mantener códigos y valores originales durante la migración y definir luego su correspondencia con catálogos de geografía, explotación, régimen y estado sanitario.
- Evaluar historial de estados sanitarios con enfermedad, fecha y fuente; falta confirmar qué representan las dos columnas actuales.
- No convertir ceros en datos faltantes ni vacíos en ceros sin confirmar su significado. «Sin Datos» tampoco equivale a «Libre».
- Mantener identificadores oficiales separados de las claves internas del nuevo sistema. No definir una restricción de unicidad de RENSPA hasta revisar los datos y las reglas reales.

## Próximo paso

Abrir la tabla `ESTABLECIMIENTOS` en Vista Diseño dentro del archivo de origen que se utilice realmente. Relevar nombres completos, tipos, tamaños, clave primaria, índices y propiedades. Si el vínculo impide ver o modificar el diseño, inspeccionar el archivo de origen en lugar de cambiar la vinculación.

Confirmar con el usuario qué archivo está vigente y qué significan las dos columnas sanitarias, las columnas de cantidades truncadas y Presencia. Después revisar EMPRESA y la ventana Relaciones para establecer el vínculo real entre productor, establecimiento y RENSPA.
