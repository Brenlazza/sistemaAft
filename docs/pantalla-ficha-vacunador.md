# Ficha operativa del vacunador

## Objetivo

Reunir en una sola pantalla la identificación profesional, los contactos, las habilitaciones, el estado de devolución de actas y los acumulados de vacuna administrada por cada vacunador.

El [prototipo navegable](../prototipos/ficha-vacunador.html) presenta la ficha del Dr. Juan Pérez dentro del alcance del Subcentro Ceres.

## Alcance y privacidad

- El operador de un subcentro ve únicamente los movimientos, remitos y actas de su centro.
- Los coordinadores ven los centros incluidos en su alcance.
- Los administradores generales pueden consultar el consolidado completo.
- Teléfonos, seguimientos y auditoría detallada son privados.
- Los teléfonos aparecen enmascarados hasta que el usuario ejecuta una acción autorizada.

La ficha no se imprime como parte de un acta o remito.

## Encabezado

Muestra:

- nombre completo;
- matrícula o identificación del idóneo;
- tipo de profesional;
- estado de habilitación y vigencia;
- veterinaria o veterinarias con las que operó en el período consultado;
- centros en los que puede operar;
- estado de preimpresión: habilitado o bloqueado por demora;
- cantidad de actas que originan el bloqueo.

La habilitación profesional y el bloqueo documental se presentan por separado. Un profesional puede estar activo, pero no recibir nuevas actas hasta regularizar las demoradas.

## Período de los indicadores

Por defecto, los indicadores corresponden a la campaña activa desde su inicio hasta la fecha actual. La pantalla permite cambiar:

- campaña;
- fecha desde y hasta;
- centro, dentro del alcance del usuario;
- veterinaria propietaria.

Todos los indicadores muestran el período aplicado. No se mezclan valores históricos con la campaña actual sin indicarlo expresamente.

## Indicadores principales

### Animales vacunados hasta la fecha

Suma las categorías bovinas y bubalinas de actas `Confirmadas` y vigentes realizadas por el vacunador dentro del período.

No incluye:

- borradores u observadas;
- actas anuladas;
- formularios devueltos sin utilizar;
- existencias declaradas de otras especies;
- actas fuera del alcance del centro seleccionado.

Una rectificación vigente reemplaza el efecto de la versión anterior sin duplicar cantidades.

### Dosis retiradas

Suma las entregas confirmadas de todos los remitos recibidos por el profesional:

`frascos llenos retirados × 125 + dosis sobrantes retiradas`

La ficha muestra además el desglose entre dosis provenientes de frascos llenos, dosis retiradas desde stock de sobrantes y cantidad de remitos involucrados.

Un remito en borrador no suma. Una anulación o reversión resta mediante su movimiento compensatorio; nunca se modifica el valor histórico original.

### Dosis devueltas

Suma únicamente devoluciones confirmadas vinculadas a los remitos del profesional:

`frascos llenos devueltos × 125 + dosis sobrantes devueltas`

Se muestran por separado frascos llenos y sobrantes abiertos porque regresan a condiciones de stock diferentes.

### Dosis aplicadas

Suma `acta_lotes_aplicados.dosis_aplicadas` de actas confirmadas y vigentes. En una vacunación antiaftosa normal debe coincidir con animales vacunados, pero se mantiene como cálculo independiente para detectar inconsistencias.

### Pendiente de explicar

Se calcula:

`dosis retiradas − dosis aplicadas − dosis devueltas`

El resultado se presenta como control operativo, no como ajuste automático:

- `0`: los movimientos del período están explicados;
- positivo: existen dosis todavía bajo custodia o pendientes de devolución/carga;
- negativo: se devolvieron o imputaron más dosis de las retiradas y debe revisarse la conciliación.

La ficha no fuerza el resultado a cero ni modifica datos para hacerlo coincidir.

## Indicadores documentales

También se muestran:

- actas entregadas pendientes;
- actas por vencer entre los días 5 y 7;
- actas demoradas desde el día 8;
- actas en borrador u observadas;
- actas confirmadas;
- remitos pendientes de conciliación;
- promedio de días de devolución de actas durante el período.

## Desglose de actividad

La pestaña `Actividad` permite agrupar los indicadores por centro, veterinaria propietaria, campaña, mes, remito, productor o establecimiento, respetando permisos.

Cada total permite abrir el conjunto de registros que lo origina. No se muestran cifras sin posibilidad de trazarlas hasta sus actas, remitos, devoluciones y movimientos.

## Actas pendientes

Lista priorizada con número o identificador provisional, remito, productor, establecimiento, fecha de entrega, antigüedad, estado, último seguimiento y acceso a carga o consulta.

La acción `Ver todas` abre la [bandeja de actas](pantalla-bandeja-actas.md) filtrada por ese vacunador.

## Remitos y conciliación

La pestaña `Remitos` muestra:

- dosis retiradas por remito;
- animales y dosis imputadas mediante actas;
- devoluciones llenas y sobrantes;
- diferencia;
- estado de conciliación.

Los remitos con varios productores aparecen una sola vez en el total retirado. Las actas vinculadas aportan cada una su cantidad vacunada sin duplicar la entrega.

## Contactos

La ficha permite administrar, según permisos:

- teléfono móvil y fijo;
- correo;
- contacto preferido;
- vigencia;
- verificación;
- autorización para avisos operativos;
- habilitación para WhatsApp.

El teléfono importado desde Access permanece como `No verificado` hasta su control. Marcar un número para WhatsApp requiere formato internacional y autorización vigente.

## Acciones

- `Ver actas pendientes`;
- `Registrar seguimiento`;
- `Avisar por WhatsApp`, previsto para una etapa posterior;
- `Ver remitos`;
- `Editar contactos`, con permiso;
- `Suspender habilitación`, solo para perfiles administrativos y con motivo;
- `Ver auditoría`, restringido.

Resolver la última acta demorada actualiza automáticamente el estado de preimpresión. No existe una acción manual `Desbloquear`.

## Reglas de cálculo y consistencia

- Los cálculos se realizan en el servidor con el mismo alcance aplicado a la consulta.
- Los indicadores se obtienen de movimientos y actas, no de contadores editables.
- Todas las cantidades se expresan en dosis; los frascos se convierten a 125 dosis sin perder su cantidad original.
- La fecha utilizada es la fecha efectiva del movimiento o acta, no la fecha de carga técnica.
- Las anulaciones y rectificaciones se aplican mediante registros compensatorios o versiones vigentes.
- Los totales de la ficha deben coincidir con los informes de stock y conciliación para el mismo período y alcance.

## Datos de soporte

La pantalla utiliza principalmente:

- `vacunadores` y `contactos_vacunador`;
- `centro_veterinaria` y las veterinarias propietarias registradas en sus remitos;
- `remitos_retiro` y `remitos_retiro_detalle`;
- `devoluciones_remito` y su detalle;
- `formularios_acta`;
- `actas_vacunacion`, categorías y lotes aplicados;
- `seguimientos_acta`;
- `conciliaciones_remito`.

No requiere una tabla de totales permanentes. Si el volumen futuro lo exige, podrán utilizarse resúmenes regenerables, conservando siempre los movimientos originales como fuente de verdad.
