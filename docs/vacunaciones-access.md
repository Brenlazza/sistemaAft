# Relevamiento de VACUNA y VACUNABT

Fuente: capturas de Vista Diseño de las tablas vinculadas `VACUNA1` y `VACUNABT1`. Los nombres, tipos y propiedades indicados a continuación corresponden a lo visible en Access. Todavía faltan los tamaños y propiedades particulares de cada campo, salvo `Ficha`, y la interpretación funcional de ambas tablas.

## VACUNA1

| Campo | Tipo en Access | Observación |
|---|---|---|
| Ficha | Texto corto | Parte de la clave primaria compuesta. La descripción indica que es el número de ficha que relaciona con Establecimientos. Su título de presentación es `Renspa`. |
| Fecha de Vacunación | Fecha/Hora | Parte de la clave primaria compuesta. |
| Marca | Texto corto | Significado pendiente; no asumir que es la marca del animal ni la marca comercial de una vacuna. |
| ID_UEL | Número | Referencia aparente a UEL; confirmar relación e integridad. |
| Acta | Número | Número de acta. Falta confirmar si debe conservar ceros iniciales y si es único dentro de una campaña, UEL u otro ámbito. |
| Matricula | Texto corto | Referencia aparente a VACUNADOR.Matricula. |
| Canti_Par | Número | Nombre abreviado; significado y unidad pendientes. |
| Fecha Recep | Fecha/Hora | Probable fecha de recepción; confirmar qué se recibe y quién registra el dato. |
| idVeterinario | Número | Referencia aparente a un veterinario. No se observa su relación en el diagrama aportado. |

## VACUNABT1

| Campo | Tipo en Access | Observación |
|---|---|---|
| Ficha | Texto corto | Parte de la clave primaria compuesta. Misma descripción y título `Renspa` que VACUNA1. |
| Fecha de Vacunación | Fecha/Hora | Parte de la clave primaria compuesta. |
| Marca | Texto corto | Significado pendiente. |
| ID_UEL | Número | Referencia aparente a UEL. |
| Acta | Número | Número de acta; alcance de unicidad pendiente. |
| Matricula | Texto corto | Referencia aparente a VACUNADOR.Matricula. |
| Canti_Par | Número | Nombre abreviado; significado y unidad pendientes. |
| Razón | Texto corto | Motivo o razón pendiente de interpretar mediante formularios y datos de ejemplo. |

## Propiedades visibles de Ficha

Las dos tablas muestran las mismas propiedades para `Ficha`:

| Propiedad | Valor visible |
|---|---|
| Tamaño del campo | 20 |
| Título | Renspa |
| Requerido | Sí |
| Permitir longitud cero | No |
| Indexado | No |
| Compresión Unicode | Sí |
| Modo IME | Sin Controles |
| Modo de oraciones IME | Nada |
| Alineación del texto | General |

Formato, máscara de entrada, valor predeterminado, regla de validación y texto de validación aparecen vacíos. Aunque la propiedad visible dice `Indexado: No`, Access muestra `Ficha` y `Fecha de Vacunación` con el icono de llave como clave primaria compuesta. La definición de la clave compuesta debe tratarse como la evidencia principal y verificarse también en la ventana Índices.

## Interpretación provisional

- Ambas tablas registran eventos de vacunación asociados a un establecimiento mediante Ficha/Renspa y a un vacunador mediante Matricula.
- La clave `(Ficha, Fecha de Vacunación)` impide más de un registro de la misma tabla para el mismo establecimiento en el mismo valor de fecha y hora. Debemos confirmar si Access guarda solo fecha o también hora en la operación real.
- VACUNA1 añade recepción e identificador de veterinario; VACUNABT1 añade Razón. Esto demuestra una diferencia funcional, pero todavía no identifica con certeza la enfermedad o el proceso de cada tabla.
- En el nuevo modelo conviene usar una clave interna para el evento y aplicar una regla de duplicados basada en el proceso real. No copiar automáticamente la clave compuesta antigua sin revisar casos de vacunación parcial, correcciones y múltiples actas en el mismo día.
- `Acta` está almacenada como Número. Antes de migrar hay que comprobar si existen ceros iniciales, prefijos o numeraciones que excedan el tipo numérico elegido.

## Formulario Vacunacion

Una captura de `Vacunacion` en Vista Diseño muestra los siguientes grupos y controles:

| Encabezado o control visible | Campo o interpretación aparente |
|---|---|
| F. de Vacun. | `Fecha de Vacunación` |
| Vacunador | `Matricula` |
| Marca y UEL | Controles para `Marca` e `ID_UEL` |
| Vac. suminist. | El control ubicado debajo corresponde aparentemente a `Canti_Par`; esto sugiere cantidad de vacunas suministradas, pendiente confirmar unidad y nombre completo. |
| Acta | `Acta` |
| Fecha Recepción / Fecha Recep | Controles relacionados con `Fecha Recep`; la disposición exacta debe confirmarse mediante las propiedades de los controles. |
| Veterinarias: | Selector asociado aparentemente a `idVeterinario`. Confirmar el origen de la lista y si realmente representa una veterinaria, un veterinario o ambos. |

El formulario contiene además controles pequeños cuyos textos visibles parecen `idVete`, `Marca`, `ID_U...` y `idVeterinario`. Vista Diseño no muestra por sí sola el origen de registro del formulario ni el origen de fila de los desplegables.

La lista de formularios visible aporta estos objetos relacionados:

- `Stock-vete`
- `Sub_vacu_distri`
- `Sub_vacu_vete`
- `Vacu-distri`
- `Vacunacion`
- `VacunacionBT`
- `Vacunador`
- `diferenciaingresoacta`
- `diferenciaingresoactap`
- `Establecimientos Vacunados Por Dpto`

Los nombres sugieren flujos de distribución, stock y conciliación, pero su función debe comprobarse mediante el origen de registro y la vista de formulario.

### Propiedades confirmadas de Vacunacion

| Propiedad | Valor |
|---|---|
| Origen del registro | VACUNA |
| Tipo Recordset | Dynaset |
| Valores predeterminados | Sí |
| Filtrar al cargar | No |
| Ordenar por al cargar | Sí |
| Entrada de datos | No |
| Permitir agregar | Sí |
| Permitir eliminación | Sí |
| Permitir ediciones | Sí |
| Permitir filtros | Sí |
| Bloqueos del registro | Sin bloquear |

Esto confirma que el formulario está vinculado directamente a VACUNA y permite consultar, agregar, modificar y eliminar registros existentes. La migración deberá reemplazar la eliminación directa por una política que conserve el historial de correcciones y anulaciones de actas.

### Selector de veterinaria

| Control de Access | Origen del control | Origen de la fila | Comportamiento |
|---|---|---|---|
| Cuadro combinado59 | idVeterinario | Consulta `Veterinarias.Veterinaria` y `Veterinarias.idVeterinario` desde la tabla Veterinarias; el criterio final de orden queda truncado en la captura. | Columna dependiente 2, `Limitar a la lista: Sí` y expansión automática activada. |

El operador selecciona la veterinaria por su nombre y el registro de VACUNA guarda `idVeterinario`, la segunda columna. Esto confirma que el campo se relaciona conceptualmente con una entidad Veterinarias. Todavía debe verificarse la relación formal en Access y el diseño de esa tabla.

La lista de controles del formulario muestra un solo cuadro combinado, `Cuadro combinado59`. A diferencia de VacunacionBT, los campos Matricula, Marca e ID_UEL no tienen selectores combinados en este formulario. Se debe investigar si llegan como valores predeterminados, parámetros, filtros o asignaciones realizadas por otro formulario o por código.

## Formulario VacunacionBT

Una captura de `VacunacionBT` en Vista Diseño permite relacionar sus controles con los campos de VACUNABT1:

| Encabezado visible | Control o campo aparente |
|---|---|
| F. de Vacun. | `Fecha de Vacunación` |
| Razón | `Razón`, mediante un control desplegable |
| Marca | `Marca`, mediante un control desplegable |
| Vacunador | `Matricula`, mediante un control desplegable |
| Uel | `ID_UEL`, mediante un control desplegable |
| Cant.Tern.Vacun. | `Canti_Par` |
| Acta | `Acta` |

`Cant.Tern.Vacun.` parece abreviar «cantidad de terneras/terneros vacunados». Dado que el menú principal describe el ingreso de actas de aftosa y brucelosis, la hipótesis más consistente es que `VACUNABT` registra vacunación de brucelosis en terneras y que `BT` puede estar relacionado con ese concepto. Debe confirmarse con el texto completo del formulario, sus consultas o informes; no se adoptará todavía como equivalencia definitiva.

La hoja de propiedades confirma además un cuadro de texto inferior llamado `Razón`, con `Origen del control: Razón`. En el diseño aparece otro control superior con flecha desplegable; falta inspeccionarlo para conocer el catálogo o consulta que proporciona los valores.

### Controles combinados identificados

| Control de Access | Origen del control | Propiedades visibles |
|---|---|---|
| Cuadro combinado55 | Matricula | Origen de fila tipo Tabla/Consulta: selecciona `VACUNADOR.Nombre` y `VACUNADOR.Matricula` desde VACUNADOR y aplica un orden cuyo final no se alcanza a leer en la captura. Tiene 2 columnas, la columna dependiente es 2 y `Limitar a la lista` está activado. Anchos aproximados: 2,09 cm y 4,577 cm; 16 filas visibles. |
| Cuadro combinado57 | ID_UEL | Origen de fila tipo Tabla/Consulta: `SELECT UEL.ID_UEL, UEL.NOMBRE FROM UEL;`. La columna dependiente es 1 y `Limitar a la lista` está activado. |
| Cuadro combinado59 | Marca | Origen tipo Lista de valores. La lista visible incluye un valor vacío, `Providean`, `Nort`, `Bruselfin`, `C.Diag.Vet.`, `Agrofarma`, `Bayer`, `Biogénesis` y `Merial`; el final del origen no entra completo en la captura, por lo que pueden existir más valores. Columna dependiente 1 y `Limitar a la lista: Sí`. |
| Cuadro combinado63 | Razón | Origen tipo Lista de valores: `Se Vacunó`, `Sin Existencia de Terneras`, `Criterio Profesional` y `Sin Datos`. Valor predeterminado: `Se Vacunó`. Columna dependiente 1 y `Limitar a la lista: Sí`. |

El selector de vacunador muestra nombre y matrícula, y almacena `Matricula` porque la columna dependiente es la segunda. El selector de UEL obtiene código y nombre y almacena `ID_UEL`, su primera columna. Los cuatro selectores limitan los valores a sus respectivas fuentes y permiten búsqueda incremental.

La lista de `Razón` confirma que VACUNABT no contiene únicamente aplicaciones efectivas: también conserva establecimientos sin existencia de terneras, casos resueltos por criterio profesional y registros sin datos. Por eso `Razón` debe modelarse como el resultado o motivo del relevamiento, separado de la cantidad vacunada. El valor predeterminado `Se Vacunó` facilita la carga, pero exige validación para evitar registrar vacunación por omisión cuando la cantidad sea cero o falte.

La lista de `Marca` contiene valores embebidos en el formulario y no depende de la tabla LABORATORIO. En el nuevo sistema conviene migrarlos a un catálogo administrable de productos, marcas o laboratorios, después de confirmar qué representa cada nombre y depurar variantes históricas.

La comparación de formularios indica provisionalmente:

- `Vacunacion`: registra fecha, vacunador, marca, UEL, vacunas suministradas, acta, recepción y veterinaria.
- `VacunacionBT`: registra fecha, razón, marca, vacunador, UEL, cantidad de terneras/terneros vacunados y acta.
- Los desplegables probablemente aplican catálogos o consultas auxiliares. Sus propiedades `Origen del control` y `Origen de la fila` permitirán identificar las tablas y reglas exactas.

### Propiedades confirmadas de VacunacionBT

La hoja de propiedades del formulario muestra:

| Propiedad | Valor |
|---|---|
| Origen del registro | VACUNABT |
| Vista predeterminada | Formularios continuos |
| Tipo Recordset | Dynaset |
| Entrada de datos | No |
| Permitir agregar | Sí |
| Permitir Vista Formulario | Sí |
| Permitir Vista Hoja de datos | Sí |
| Permitir Vista Presentación | Sí |
| Selectores de registro | Sí |
| Botones de navegación | Sí |

Esto confirma que el formulario está vinculado directamente a `VACUNABT`, permite recorrer registros existentes y agregar nuevos. No se observa una consulta intermedia en `Origen del registro`. Aún falta comprobar las propiedades de edición y eliminación que puedan encontrarse más abajo en la hoja, además de cualquier validación aplicada mediante eventos o código.

## Información pendiente

1. Confirmar la expansión exacta de `BT`; el formulario ya demuestra que el proceso registra vacunación de terneras y excepciones como falta de existencia y criterio profesional.
2. Confirmar si `Marca` representa producto, fabricante o laboratorio, y obtener la lista completa. Determinar el significado exacto de `Fecha Recep`. En VACUNA, `Canti_Par` parece corresponder a vacunas suministradas; en VACUNABT se presenta como cantidad de terneras vacunadas.
3. Tamaño y subtipo numérico de cada campo mediante la selección individual en Vista Diseño.
4. Ya se confirmó que los formularios usan directamente VACUNA y VACUNABT. Falta `Origen del control` y `Origen de la fila` de los campos principales de Vacunacion.
5. Ventana Índices para confirmar la clave primaria y cualquier índice adicional.
6. Diseño y clave de la tabla Veterinarias y relación formal con `VACUNA.idVeterinario`; el selector ya confirma su uso conceptual.
