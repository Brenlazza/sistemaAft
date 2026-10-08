# Pantalla de carga y confirmación del acta

## Objetivo

Transcribir el original que vuelve del campo, validar lo escrito y confirmar la vacunación sin perder la relación con el formulario físico, el remito, el productor, el establecimiento, el profesional y la vacuna retirada.

El [prototipo navegable](../prototipos/carga-acta-vacunacion.html) muestra una carga originada en un remito del Subcentro Ceres. La misma pantalla se utiliza en la principal y en los demás subcentros, respetando el alcance del usuario.

## Entrada a la pantalla

Puede abrirse desde:

- la bandeja de actas pendientes;
- un remito entregado;
- la conciliación del remito;
- la búsqueda por número de formulario, productor o RENSPA.

Si existe un preborrador, el operador siempre completa ese registro. No crea otra acta con el mismo número.

## Encabezado bloqueado

Se muestran como solo lectura los datos preimpresos:

- número del formulario y estado físico;
- campaña, período y plan;
- centro que emitió el remito;
- número y versión vigente del remito;
- veterinaria propietaria;
- profesional receptor y vacunador previsto;
- cliente/productor, documento y RENSPA;
- establecimiento, domicilio y ubicación;
- tipo de rodeo, régimen y hectáreas;
- marca ganadera, cuando esté digitalizada;
- vacuna, lote y vencimiento previstos.

Una corrección observada en el papel no sobrescribe la instantánea. Se registra como dato efectivo y conserva ambos valores.

## Recepción del documento físico

Antes de cargar se registra:

- estado: devuelto completo, devuelto sin utilizar, ilegible, incompleto o faltante;
- fecha de recepción;
- legibilidad;
- presencia del original;
- imagen o PDF del original, cuando se adjunte;
- observación documental.

`Devuelto sin utilizar` resuelve el formulario sin crear vacunación ni consumo. Un documento ilegible o incompleto puede guardarse, pero queda `Observado` hasta su resolución.

## Datos efectivos de la visita

- lugar y fecha de vacunación;
- vacunación total o parcial;
- tipo de intervención;
- resultado: vacunación realizada o motivo de no vacunación;
- vacunador efectivo, cuando difiera del previsto;
- observaciones manuscritas;
- correcciones visibles del papel.

Una fecha fuera de campaña requiere permiso y motivo. Un acta parcial es independiente y no bloquea futuras actas para el mismo establecimiento dentro de la campaña.

## Bovinos y bubalinos vacunados

Se transcriben cantidades enteras por categoría:

- vacas;
- toros;
- toritos;
- novillos/bueyes;
- novillitos;
- vaquillonas;
- terneras;
- terneros;
- categorías bubalinas configuradas, cuando correspondan.

El total es calculado y no editable. El sistema compara las categorías con las reglas de la campaña. Se permiten casos excepcionales, pero exigen marcar la excepción, indicar motivo y contar con autorización al confirmar.

## Vacuna realmente utilizada

Se registra una línea por lote utilizado:

- marca;
- serie/lote;
- vencimiento;
- dosis aplicadas;
- coincidencia o diferencia respecto del lote preimpreso;
- motivo del cambio, cuando corresponda.

La suma de dosis aplicadas por lote debe coincidir con el total de animales vacunados. Todos los lotes deben pertenecer al remito y a la veterinaria propietaria, salvo una excepción expresamente autorizada.

La pantalla muestra para cada lote:

- dosis retiradas en el remito;
- dosis ya imputadas por otras actas confirmadas;
- dosis disponibles bajo custodia para imputar;
- dosis cargadas en el acta actual.

No puede confirmarse una cantidad superior a lo todavía explicable por el remito.

## Existencia de otras especies

Las existencias de ovinos, porcinos, caprinos, equinos y otras especies se cargan manualmente tal como fueron escritas. Sus totales se calculan por grupo, pero no se suman a las dosis de vacuna antiaftosa aplicadas.

Una cantidad en estas existencias no significa que esa especie haya sido vacunada; representa la declaración incluida en el acta.

## Programas adicionales

Cuando el papel contiene datos de brucelosis o carbunclo se habilitan sus secciones:

- resultado y cantidades;
- marca, serie/lote y vencimiento;
- identificación requerida;
- observaciones.

Cada programa conserva sus propias cantidades y lotes. No se mezcla su consumo con la vacuna antiaftosa.

## Firmas y DNI en papel

No se muestran tildes ni controles digitales para las firmas. El vacunador y el propietario o responsable firman físicamente los tres ejemplares y completan sus DNI a mano en el papel.

La pantalla no transcribe firmas ni DNI, no almacena sus trazos y no exige marcarlos para confirmar la carga. El original físico conserva esa constancia documental.

## Validaciones antes de confirmar

- formulario válido, devuelto y no utilizado por otra acta;
- remito entregado y con firma vigente;
- productor/establecimiento incluido entre sus destinos;
- centro y veterinaria dentro del alcance del usuario;
- fecha, lugar y resultado completos;
- cantidades enteras y no negativas;
- total por categorías igual al total por lotes aplicados;
- lotes pertenecientes al retiro y no vencidos en la fecha efectiva;
- dosis suficientes pendientes de imputación;
- documento físico recibido y legible;
- motivo y autorización para toda excepción.

## Guardado y revisión

- `Guardar borrador`: conserva una carga incompleta sin afectar cobertura ni conciliación.
- `Marcar observada`: registra inconsistencias y la deriva a revisión.
- `Validar`: muestra errores y advertencias sin confirmar.
- `Confirmar acta`: aplica todos los efectos en una única transacción.

Repetir por error una confirmación no puede producir un segundo consumo ni duplicar la cobertura.

## Efectos de la confirmación

1. El acta pasa a `Confirmada` y queda bloqueada para edición directa.
2. El formulario físico pasa a `Cargado`.
3. Las dosis se imputan como aplicadas dentro de la custodia del remito y lote.
4. Se actualiza la cobertura del establecimiento y la campaña.
5. Se recalculan dosis vacunadas y diferencias de la conciliación.
6. Se registra la actividad privada de auditoría.

El remito ya descontó la vacuna del stock del centro al momento de la entrega. Por eso confirmar el acta no vuelve a descontar el depósito: transforma parte de lo retirado en uso aplicado. El retorno de frascos o sobrantes se registra por la devolución separada.

## Actas parciales

Después de confirmar una parcial puede generarse otro formulario para el mismo establecimiento. Cada acta conserva número, fecha, categorías, lote y dosis propios. La cobertura se calcula sumando solamente actas confirmadas y vigentes.

La pantalla muestra las actas anteriores de la misma campaña para advertir posibles duplicaciones sin impedir una visita parcial legítima.

## Permisos y alcance

El operador del subcentro solo puede cargar formularios vinculados a remitos de su centro. Los administradores pueden consultar todos los centros. Confirmar, aceptar excepciones, rectificar y anular son permisos independientes.

Los datos técnicos de usuario, fecha detallada e historial se conservan de forma privada y no aparecen en el acta impresa.
