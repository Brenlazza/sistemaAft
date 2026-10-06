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

## Columnas observadas inicialmente

Estas observaciones surgieron de la hoja de datos y la vista de Relaciones. La sección Diseño confirmado, más abajo, incorpora luego los tipos y descripciones de Vista Diseño.

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

## Diseño confirmado

Vista Diseño confirma `Ficha` como clave primaria de texto. Su descripción dice que debe ser única para cada establecimiento. En la hoja de datos se presenta como `Renspa`, con valores que incluyen puntos y barra; debe conservarse como texto.

### Identificación, titular y ubicación

| Campo | Tipo en Access | Descripción o evidencia |
|---|---|---|
| Ficha | Texto corto | Clave primaria; debe ser única por establecimiento. Se presenta como Renspa. |
| Propietario | Texto corto | Nombre del propietario o de quien alquila el predio, según la descripción original. |
| Tipo_doc | Texto corto | Valores indicados: blanco, DNI y CUIT; en los datos también se observó CUIL. |
| Nro_doc | Texto corto | Documento o identificación fiscal. Conservar formato original. |
| Clasi | Texto corto | Valores indicados: blanco, RI, MT, EX y CF; significado tributario por confirmar. |
| Domicilio | Texto corto | Domicilio del propietario. |
| Localidad | Texto corto | Localidad asociada al propietario. |
| Telefono | Texto corto | Contacto. |
| Boleto | Número | Probable boleto de marca; significado y uso de cero pendientes. |
| Establecimiento | Texto corto | Nombre del establecimiento. |
| Departamento | Número | Departamento donde se encuentra el predio; relacionado conceptualmente con TABGEO. |
| Distrito | Número | Código de distrito; relacionado conceptualmente con TABGEO. |
| Explotación | Texto corto | Tipo de explotación; la descripción enumera Tambo, Cría y Otros. |
| Explotacion2 | Texto corto | Segunda explotación; finalidad exacta pendiente. |

### Existencias, sanidad y otros datos

| Campo | Tipo en Access | Descripción o evidencia |
|---|---|---|
| Vacas | Número | Cantidad de vacas. |
| Vaquillonas | Número | Cantidad de vaquillonas. |
| Toros | Número | Cantidad de toros. |
| Toritos | Número | Cantidad de toritos. Se observó subtipo Entero largo, valor predeterminado 0, no requerido y no indexado. |
| Terneros | Número | Cantidad de terneros. |
| Terneras | Número | Cantidad de terneras. |
| Novillos | Número | Cantidad de novillos. |
| Novillitos | Número | La descripción dice «Cantidad de reactores + durante el período de saneamiento», lo cual no coincide con el nombre; requiere validación. |
| Bufalos_may | Número | Cantidad de búfalos mayores; la descripción aclara que no se vacunan en parcial. |
| Bufalos_men | Número | Cantidad de búfalos menores. |
| Marca | Texto corto | En los datos se observaron Marca a Fuego, Señal y Ninguna. |
| Estado Sanitario B | Texto corto | Estado sanitario probablemente de brucelosis; pendiente confirmar. |
| Estado Sanitario T | Texto corto | Estado sanitario probablemente de tuberculosis; pendiente confirmar. |
| Regimen | Texto corto | Valores descriptos: Propietario, Pastajero, Arrendatario y Capitalización. |
| Has | Número | Probables hectáreas; confirmar unidad. |
| Caprinos | Número | Cantidad. |
| Ovinos | Número | Cantidad. |
| Porcinos | Número | Cantidad. |
| Equinos | Número | Cantidad. |
| Otros | Número | Cantidad de otras especies. |
| Otros_aclarar | Texto corto | Detalle de otras especies. |
| Provincia | Texto corto | Provincia; confirmar si corresponde al predio. |
| ID_EMPRE | Número | Referencia a EMPRESA; 0 corresponde a «Sin empresa». |
| Fgarrapata | Fecha/Hora | Fecha relacionada con garrapata; significado exacto pendiente. |
| Presencia | Sí/No | Indicador cuya semántica sigue pendiente; no interpretarlo aún como vacunación realizada. |

La tabla está configurada para mostrar una subhoja `Tabla.VACUNA`, vinculando `ESTABLECIMIENTOS.Ficha` con `VACUNA.Ficha`. Esto explica los signos «+» observados en la hoja de datos y confirma el acceso desde un establecimiento a sus registros de vacunación antiaftosa.

## Consecuencias para el nuevo modelo (propuestas)

- Separar los datos de contacto e identificación del productor de los datos físicos del establecimiento, una vez confirmada la identidad de cada entidad.
- Relevar el vínculo productor–establecimiento, su régimen y su vigencia. La repetición de nombres de predio y variantes de RENSPA justifica investigarlo, pero no demuestra por sí sola que los registros sean duplicados o pertenezcan al mismo predio.
- Proponer una tabla `existencias_animales` con referencia al registro productor–establecimiento que corresponda, fecha de relevamiento, campaña cuando aplique, categoría y cantidad. Las existencias del padrón deben distinguirse de los animales efectivamente vacunados en un acta.
- Mantener códigos y valores originales durante la migración y definir luego su correspondencia con catálogos de geografía, explotación, régimen y estado sanitario.
- Evaluar historial de estados sanitarios con enfermedad, fecha y fuente; falta confirmar qué representan las dos columnas actuales.
- No convertir ceros en datos faltantes ni vacíos en ceros sin confirmar su significado. «Sin Datos» tampoco equivale a «Libre».
- Mantener identificadores oficiales separados de las claves internas del nuevo sistema. No definir una restricción de unicidad de RENSPA hasta revisar los datos y las reglas reales.

## EMPRESA

Fuente: captura de Vista Diseño de `EMPRESA1`. Origen confirmado: `D:\DiscoDpc2\aftosa 31\datos.mdb`, tabla `EMPRESA`.

| Campo | Tipo en Access | Observación |
|---|---|---|
| ID_EMPRE | Número, Entero | Clave primaria, requerido, índice único y valor predeterminado 0. |
| NOMBRE | Texto corto | Denominación de la empresa o agrupación. |

El diagrama muestra una relación uno a muchos desde EMPRESA hacia ESTABLECIMIENTOS, cuyo campo `ID_EMPRE` almacena la referencia. La estructura no explica qué función cumple la empresa: puede ser propietaria, administradora, agrupadora u otra entidad. Se necesita observar los nombres existentes o el formulario que la utiliza.

Una captura posterior de la hoja de datos muestra 247 registros. El registro 0 se denomina `Sin empresa`. Los demás nombres corresponden principalmente a empresas lácteas, cooperativas, queserías, firmas comerciales y algunas personas. Varios incluyen `Cerrada` dentro del propio nombre.

Esto indica que EMPRESA representa una vinculación comercial o industrial del establecimiento —probablemente la empresa láctea o receptora— y no al propietario, que ya figura directamente en ESTABLECIMIENTOS. El significado contractual exacto y si corresponde a comprador, receptor de leche u otra relación todavía debe confirmarse.

### Consecuencias para el nuevo modelo

- No equiparar EMPRESA con productores o propietarios.
- Modelar la empresa receptora o relacionada como una entidad independiente.
- Si la relación puede cambiar, usar una tabla de vínculo establecimiento–empresa con fechas de vigencia en vez de sobrescribir ID_EMPRE.
- Representar «sin empresa» mediante ausencia de relación o un estado explícito, no mediante una entidad ficticia con identificador 0.
- Separar el estado activa/cerrada del nombre. Conservar el texto histórico original durante la migración.
- Revisar nombres duplicados, razones sociales, personas físicas y empresas cerradas antes de aplicar reglas de unicidad.

## Próximo paso

Confirmar con el usuario qué archivo está vigente y qué significan Estado Sanitario B, Estado Sanitario T, Fgarrapata y Presencia. Revisar EPIDEMIA y los formularios o informes sanitarios para resolver esas equivalencias. También debe aclararse la descripción anómala de Novillitos.
