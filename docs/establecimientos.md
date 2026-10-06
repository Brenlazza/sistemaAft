# Relevamiento de ESTABLECIMIENTOS

## Evidencia disponible

Tres capturas adicionales muestran la hoja de datos de `ESTABLECIMIENTOS1`, desplazada horizontalmente, y el administrador de tablas vinculadas. No muestran Vista Diseño: aún no conocemos los tipos declarados, las claves, los índices ni las restricciones.

Actualización: una captura posterior de Relaciones muestra `Ficha` con icono de clave en ESTABLECIMIENTOS1. Ver [claves y relaciones observadas](relaciones-access.md). Los tipos e índices siguen pendientes de Vista Diseño.

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

Los encabezados cortados se conservan con puntos suspensivos. El contenido visible ayuda a interpretar su función, pero no acredita el tipo de datos declarado en Access.

| Encabezado visible | Observación y pendiente |
|---|---|
| Renspa | Identificador con puntos y barra. Conservar como texto con su formato; falta confirmar unicidad y clave. |
| Propietario | Nombre de persona o razón social. |
| Tipo de Doc | Se observan CUIT, CUIL, DNI y celdas vacías. Confirmar catálogo y reglas. |
| Nro Docume… | Identificación documental; se observan formatos con y sin separadores. Conservar el valor original y validar según tipo, sin convertirlo a número. |
| Clasificació… | No se distingue una clasificación informada en las filas visibles. Confirmar finalidad y valores. |
| Domicilio Prop… | Domicilio del propietario, según el encabezado. No confundir con ubicación del predio. |
| Localidad Pr… | Localidad del propietario, pendiente nombre completo. |
| Telefono | Contacto. Tratar como texto en la propuesta de modelo. |
| Boleto de M… | Se observan números y ceros; confirmar si corresponde a boleto de marca y qué significa cero. |
| Nombre del … | Se observan nombres de establecimientos. Confirmar encabezado completo. |
| Departame… | Se observan códigos numéricos; falta catálogo y jurisdicción. |
| Distrito | Se observan códigos numéricos; falta catálogo. |
| Tipo de Expl… | Se observan Cría, Invernada y Tambo. |
| Explo2 | Segunda columna de explotación; se observan Invernada, Tambo y vacíos. Confirmar si representa actividad secundaria. |
| Cani… (primera captura) | Encabezado incompleto, junto a categorías de ganado. No asignar significado hasta verlo completo. |
| Vaq… | Posiblemente vaquillonas; requiere confirmación. |
| Torc… | Posiblemente toros; requiere confirmación del encabezado exacto. |
| Toritos | Categoría visible; las cantidades no se distinguen en el área mostrada. |
| Terneros | Cantidades visibles. |
| Terneras | Cantidades visibles. |
| Novillos | Cantidades visibles. |
| Novillitos | Cantidades visibles. |
| Búfalos may… | Categoría incompleta; confirmar nombre y límite de edad. |
| Búfalos Mer… | Categoría incompleta; confirmar nombre y límite de edad. |
| Marca | Se observan Marca a Fuego, Señal y Ninguna. Confirmar si describe el método de identificación. |
| Estado Sanitario B… | Se observan Sin Datos, En Saneamiento y Libre. Confirmar enfermedad; no dar por hecho brucelosis por la inicial. |
| Estado Sanitar… | Otra columna sanitaria con valores similares; falta identificar enfermedad y diferencia con la anterior. |
| Regimen de … | Se observan Propietario, Arrendatario y Pastajero. Confirmar nombre completo y si se aplica al vínculo productor–predio. |
| Cani… (segunda captura) | Otra columna de encabezado incompleto con cantidades. No asumir superficie, capacidad ni total de rodeo. |
| Caprinc… | Parece referirse a caprinos; confirmar nombre completo. |
| Ovinos | Cantidades visibles. |
| Porc… | Posiblemente porcinos; confirmar. |
| Equinos | Cantidades visibles. |
| Otrc… (primera aparición) | Cantidades visibles; significado pendiente. |
| Otrc… (segunda aparición) | Columna distinta también truncada; significado pendiente. |
| Provincia | Nombres de provincias. Confirmar si corresponde al domicilio del propietario o al establecimiento. |
| Empre… | Se observan ceros; confirmar si es referencia a EMPRESA y si cero es un valor especial. |
| Farrgapata | Encabezado que se lee en la captura, pendiente verificación ortográfica. Se observan fechas y vacíos; confirmar si registra una fecha relacionada con garrapata. |
| Presencia | Casillas marcadas y desmarcadas; confirmar de qué presencia se trata. No interpretar como vacunación realizada. |

Puede haber columnas fuera de las áreas capturadas. Esta lista no certifica que el esquema esté completo. Los signos «+» de las filas indican subhojas expandibles, pero no permiten identificar relaciones ni cardinalidades.

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
