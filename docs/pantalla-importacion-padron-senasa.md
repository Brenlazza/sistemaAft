# Asistente de importación del padrón SENASA

## Objetivo

Incorporar el archivo recibido al inicio de una campaña sin sobrescribir padrones anteriores, conservando el archivo original, cada fila y las decisiones de validación o vinculación.

El [prototipo navegable](../prototipos/importacion-padron-senasa.html) representa el proceso guiado.

## Principio central

La importación se ejecuta primero en un área temporal. Subir o prevalidar un archivo no modifica clientes, establecimientos, remitos, actas ni el padrón operativo.

La incorporación ocurre únicamente al confirmar el resumen final. Incluso entonces:

- se crean la importación y sus filas originales;
- se confirman únicamente coincidencias exactas permitidas por la política;
- los casos ambiguos permanecen pendientes;
- no se crean clientes automáticamente solo por semejanza de nombre;
- nunca se sobrescribe una importación anterior.

## Formatos admitidos

- XLSX como formato preferido;
- CSV cuando el origen lo requiera;
- XLS legado, con lectura controlada y registro de cualquier conversión técnica.

Cada archivo conserva nombre original, tamaño, formato, hash, fecha de recepción, campaña, usuario y copia inmutable. Un hash ya registrado para la misma campaña genera una advertencia de posible duplicado antes de continuar.

## Paso 1 — Archivo y contexto

Campos:

- campaña;
- archivo;
- fecha de recepción;
- fuente o remitente;
- finalidad: padrón inicial, reemplazo formal o complemento;
- observación;
- hojas del libro que contienen registros y hojas utilizadas solo como control.

Un reemplazo formal no elimina el anterior. Crea una nueva versión y exige motivo. Un complemento agrega filas a revisar; no altera silenciosamente la fotografía original.

## Paso 2 — Correspondencia de columnas

El usuario relaciona encabezados del archivo con campos internos.

Campo mínimo para generar una fila identificable del formato real recibido:

- titular o nombre del productor.

RENSPA, CUIT y nombre de establecimiento son opcionales desde el punto de vista operativo. Cuando faltan, se conserva `No informado` y se utiliza un identificador interno; nunca se impide vacunar por esa ausencia.

El nombre del establecimiento no está presente en el listado recibido. Por eso no es obligatorio para esta importación: el predio se identifica primero por RENSPA. Si el RENSPA aún no existe en el maestro, se genera una incidencia para crear o vincular un establecimiento provisional, sin inventar un nombre. El nombre podrá completarse después desde una fuente autorizada.

Campos opcionales cuando estén presentes:

- CUIT, CUIL, DNI y tipo de documento;
- provincia, departamento, distrito o localidad;
- domicilio y contacto;
- régimen;
- explotación;
- condición total/parcial;
- cantidades por categoría animal;
- identificadores o estados provistos por SENASA.

## Estructura comprobada del listado 1/2026

El archivo `LISTADO GENERAL EXISTENCIA BOVINA PRIMERA CAMPAÑA 2026.xls` contiene tres hojas:

| Hoja | Uso | Registros útiles |
|---|---|---:|
| `UP TOTAL A VACUNAR` | UP del ámbito principal | 3.440 |
| `UP- CONVENIO ENTE SANITARIOS` | UP de otros distritos con convenio | 57 |
| `TOTALES` | Control general; no genera filas del padrón | 0 |

El total esperado es de **3.497 UP**, todas con RENSPA único dentro del libro. El importador procesa las dos primeras hojas en una sola versión, pero conserva en cada fila la hoja, el tipo de origen y el número de fila.

La correspondencia comprobada es:

| Encabezado de origen | Campo interno |
|---|---|
| `RENSPA` | RENSPA original y normalizado |
| `TITULAR` | Nombre original del productor/cliente |
| `CUIT` | CUIT original y normalizado, siempre tratado como texto |
| `OFICINA LOCAL` | Oficina SENASA de origen, cuando esté informada |
| `PARTIDO` | Partido/departamento informado |
| `SUPERFICIE` | Superficie informada; no se presume unidad si el archivo no la declara |
| `De Riesgo` | Indicador de riesgo recibido |
| `FECHA ANTEULTIMA AFTOSA` | Fecha de vacunación anteúltima, cuando exista |
| `FECHA ULTIMA AFTOSA` / `ULT, AFTOSA` | Fecha de última vacunación |
| `VACAS`, `VAQUILLONAS`/`VAQUI`, `NOVILLOS`/`NOV`, `NOVILLITOS`/`NOVTOS`, `TERNERAS`/`TRAS`, `TERNEROS`/`TEROS`, `TORITOS`, `TOROS`/`toro`, `BUEYES` | Existencia recibida por categoría |
| `TOTAL DE BOVINOS` | Total declarado por SENASA, conservado como dato de control |

Las abreviaturas de la hoja de convenios se resuelven mediante un mapeo específico de esa hoja. No se exige que todas las hojas compartan los mismos encabezados.

Las filas vacías y las filas cuyo RENSPA contiene `TOTAL` o `TOTALES` se excluyen como estructura del libro, no como errores del padrón. Los valores de esas filas y de la hoja `TOTALES` se conservan como controles de conciliación.

La ausencia de una columna opcional no inventa valores. Los encabezados no reconocidos se pueden ignorar o conservar como datos adicionales de origen.

El mapeo puede guardarse como perfil reutilizable, con nombre y vigencia, pero siempre se vuelve a validar contra el archivo actual.

## Paso 3 — Prevalidación

La pantalla muestra:

- filas leídas;
- filas válidas estructuralmente;
- alertas de calidad que permiten continuar;
- RENSPA vacíos o con formato inusual;
- documentos inválidos o ausentes;
- duplicados dentro del mismo archivo;
- posibles duplicados con importaciones anteriores;
- cantidades negativas, no numéricas o inconsistentes;
- titular vacío, que requiere una referencia operativa para distinguir la fila;
- localidades o categorías no reconocidas.

Para este libro también se comprueba:

- que las 3.440 UP principales más las 57 de convenio coincidan con las 3.497 UP declaradas;
- que la suma por categoría coincida con los totales de cada hoja y con la hoja `TOTALES`;
- que la suma de categorías de cada fila coincida con `TOTAL DE BOVINOS`;
- que no existan cantidades negativas;
- que los RENSPA no se repitan entre hojas;
- que los CUIT y las fechas faltantes queden como advertencias, sin inventar datos.

La prevalidación del archivo real detectó 6 filas cuya suma por categorías difiere del total declarado, 4 filas con al menos una cantidad negativa, 11 CUIT ausentes, 94 fechas de última vacunación ausentes en la hoja principal, 1 en la hoja de convenios y 152 fechas de vacunación anteúltima ausentes en la hoja principal. Estas cifras son evidencia del archivo recibido; la pantalla debe recalcularlas en cada ejecución. Son alertas informativas: las 3.497 UP se conservan y pueden utilizarse.

Las cantidades SENASA son estimativas. Una diferencia posterior entre lo informado y lo efectivamente vacunado —por nacimientos, muertes, movimientos u otras causas reales— no es un error de carga ni una causa de bloqueo. La cantidad efectiva proviene del acta.

Cada incidencia conserva número de fila, columna original, valor recibido, severidad y mensaje. Se puede descargar un informe para corregir el archivo.

Los valores originales se muestran junto a la interpretación propuesta. Normalizar no significa reemplazar el original.

## Paso 4 — Coincidencias propuestas

Clasificación:

| Resultado | Tratamiento |
|---|---|
| Exacta | Documento y/o RENSPA coinciden sin contradicciones; puede proponerse para confirmación automática. |
| Probable | Existen indicios suficientes, pero requiere confirmación humana. |
| Ambigua | Hay más de un candidato o datos contradictorios; queda pendiente. |
| Nueva | No existe candidato confiable; no crea un cliente por sí sola. |
| Observada | Presenta un dato incompleto o contradictorio que requiere seguimiento, sin impedir su uso operativo. |

Los porcentajes de similitud son ayudas visuales y no constituyen prueba de identidad.

## Paso 5 — Confirmación

Antes de confirmar se informa:

- archivo, campaña y finalidad;
- total de filas;
- exactas que quedarán vinculadas;
- pendientes de revisión;
- observadas;
- alertas de calidad pendientes;
- posibles nuevos clientes y establecimientos;
- diferencias con la versión previa.

Los resultados de coincidencia con clientes y establecimientos se muestran únicamente después de comparar contra el maestro vigente. El prototipo no debe presentar cantidades simuladas como si fueran resultados del archivo real.

La confirmación exige usuario autorizado y una aceptación explícita. La operación es transaccional: si falla, no deja una importación parcialmente incorporada.

## Resultado

Al finalizar se muestra:

- identificador y versión de importación;
- hash del archivo;
- filas incorporadas, vinculadas, pendientes y con alertas;
- informe de incidencias;
- acceso al [padrón operativo](pantalla-productores-establecimientos.md) filtrado por esta importación;
- historial de quién confirmó y cuándo.

## Reglas de actualización

- Cada importación permanece disponible para reproducir informes históricos.
- Solo una versión puede marcarse como referencia operativa principal de una campaña, sin borrar las demás.
- Cambiar la versión principal requiere permiso, motivo y auditoría.
- Las actas ya confirmadas conservan la importación y el vínculo utilizados en ese momento.
- Una actualización no reasigna automáticamente actas, remitos ni facturas.
- Una fila corregida se vincula mediante una decisión nueva; la original no se edita.

## Permisos

Permisos independientes para cargar, prevalidar, guardar perfiles de mapeo, confirmar importaciones, cambiar la versión principal, resolver incidencias y descargar archivos o informes.

Un usuario de subcentro puede consultar el padrón dentro de su alcance, pero la importación y activación de versiones corresponde a usuarios administrativos autorizados.

## Seguridad y privacidad

- Los archivos contienen datos personales y se almacenan con acceso restringido.
- La descarga del original queda auditada.
- Las vistas previas enmascaran documentos salvo permiso específico.
- Los archivos rechazados no quedan en directorios temporales accesibles; se conservan o eliminan conforme a la política documental aprobada.
- El sistema valida tipo, tamaño e integridad antes de procesar el contenido.
