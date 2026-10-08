# Actas numeradas con emisión digital y carga posterior

## Objetivo

Mantener el acta en papel para el trabajo de campo, pero generarla desde el sistema con número controlado y datos conocidos ya impresos. El vacunador completa a mano únicamente los datos producidos durante la visita. Luego un operador recupera el acta por número o código, transcribe los campos manuscritos y confirma el registro.

Este flujo reduce escritura repetida, errores de identificación y carga duplicada sin exigir conectividad en el establecimiento.

## Datos preimpresos

La emisión debería completar automáticamente:

- número de acta;
- campaña y tipo total/parcial;
- UEL responsable;
- vacunador y matrícula;
- marca, serie/lote y vencimiento de la vacuna asignada;
- RENSPA/Ficha;
- productor, tipo y número de documento;
- establecimiento;
- ubicación: provincia, departamento, distrito/localidad;
- tipo de explotación y régimen;
- fecha de emisión;
- código de barras o QR que identifique el formulario, sin incluir datos personales en el código.

El [relevamiento del acta actual](acta-actual-relevada.md) confirma además código de plan, tipo sistemática/estratégica, total/parcial, período primero/segundo, totales/menores, régimen, hectáreas y tipo de rodeo.

Los datos se guardan como una instantánea de lo impreso. Si luego cambia el propietario o el padrón, el sistema conserva qué información figuraba en esa acta física.

## Datos que se completan a mano

- fecha efectiva de vacunación;
- cantidades vacunadas por categoría animal;
- existencias de ovinos, porcinos, caprinos, equinos y otras especies;
- dosis utilizadas;
- diferencias encontradas respecto del padrón;
- observaciones;
- firma, aclaración y DNI del productor o responsable;
- firma, aclaración y DNI del vacunador; la matrícula ya figura preimpresa;
- motivo cuando no se pudo vacunar.

La lista definitiva debe compararse con el acta actual. Los campos que puedan conocerse al imprimir deben evitarse en la sección manuscrita.

Las firmas y los DNI se completan únicamente en papel. La carga posterior no incluye tildes de presencia ni captura digital de esos datos.

Si en campo se utiliza una vacuna diferente de la preimpresa, el vacunador debe corregir marca, serie/lote y vencimiento de forma legible y explicar el cambio en observaciones. La carga posterior registra el dato impreso y el efectivamente utilizado.

## Juego por triplicado

Cada número corresponde a un único juego documental de tres ejemplares con el mismo contenido:

1. `ORIGINAL: FUNDACIÓN`.
2. `DUPLICADO: SENASA`.
3. `TRIPLICADO: PRODUCTOR`.

El destino se imprime claramente en el pie de cada hoja. Los tres ejemplares comparten número de acta, campaña, establecimiento, vacunador y vacuna asignada. El sistema cuenta juegos emitidos y, además, registra qué ejemplares se imprimieron o reimprimieron.

Las hojas se completan apiladas con papel carbónico entre original, duplicado y triplicado. La salida debe mantener la misma geometría en las tres páginas para que casillas, líneas, firmas y tablas coincidan al escribir. La impresión operativa se realizará en A4, escala 100 % y sin ajuste automático.

Una reimpresión conserva el número y el destino del ejemplar, incorpora la leyenda `REIMPRESIÓN` y queda asociada al usuario, fecha y motivo. No crea una cuarta copia ordinaria ni consume otro correlativo.

## Numeración

Cada acta necesita dos identificadores:

1. Una clave interna no visible, estable y sin significado comercial.
2. Un número humano, correlativo y preimpreso.

El acta actual utiliza un correlativo de ocho posiciones con ceros iniciales, por ejemplo `00118953`. También imprime la fecha y el rango completo del lote o talonario. Esta numeración se conservará inicialmente para mantener continuidad documental.

La secuencia todavía debe definir formalmente su ámbito: global, por entidad emisora, por código de plan o por otro criterio administrativo.

Reglas mínimas:

- un número emitido nunca se reutiliza;
- reservar números y confirmar la generación dentro de una única operación;
- registrar quién generó, imprimió, reimprimió, entregó, anuló y cargó el acta;
- una reimpresión conserva el mismo número e indica `REIMPRESIÓN` y su número de copia;
- las actas perdidas, dañadas o no utilizadas se anulan con motivo;
- los saltos de numeración quedan explicados mediante estados, no se eliminan;
- el QR o código de barras identifica el acta, pero la numeración sigue siendo legible.

## Ciclo de vida del formulario

| Estado | Significado |
|---|---|
| Reservada | Número asignado, documento todavía no emitido. |
| Impresa | Documento generado al menos una vez. |
| Entregada | Formulario entregado al vacunador. |
| Completada en campo | El papel regresó con información manuscrita. |
| Cargada | Los datos manuscritos fueron transcritos. |
| Confirmada | La carga fue validada y produce cumplimiento y consumo de stock. |
| Observada | Presenta inconsistencias pendientes. |
| Anulada | No puede utilizarse; conserva número y motivo. |
| Extraviada | No regresó; conserva trazabilidad y bloquea su reutilización. |

No todos los estados necesitan una pantalla independiente, pero sus eventos deben quedar auditados.

## Emisión

1. El operador selecciona campaña, UEL, vacunador y uno o varios establecimientos.
2. El sistema valida que los establecimientos pertenezcan al padrón y no tengan un acta incompatible ya confirmada.
3. Reserva un número para cada acta.
4. Guarda la instantánea de los datos preimpresos.
5. Genera un PDF no editable para impresión.
6. Genera para cada número el original, duplicado y triplicado.
7. Registra lote de impresión, cantidad de juegos, cantidad de hojas y responsable.

La generación masiva debe producir un índice o carátula con números, establecimientos y vacunador para controlar la entrega.

## Carga posterior

1. El operador escanea el código o ingresa el número de acta.
2. El sistema recupera los datos preimpresos, que aparecen bloqueados.
3. Se transcriben únicamente los campos manuscritos.
4. Opcionalmente se adjunta una fotografía o escaneo del papel.
5. Se ejecutan validaciones y se muestran diferencias con padrón y stock.
6. Otro usuario puede revisar y confirmar cuando se requiera doble control.

La pantalla debe permitir indicar que el papel es ilegible, está incompleto o necesita corrección sin inventar datos.

## Validaciones iniciales

- fecha dentro de la campaña o excepción autorizada;
- vacunador habilitado y asignado;
- acta no confirmada ni anulada previamente;
- cantidades enteras y no negativas;
- suma por categorías coherente con el total, si el acta lo utiliza;
- lote existente en el stock del vacunador;
- dosis aplicadas no mayores que las disponibles;
- lote no vencido en la fecha de vacunación;
- motivo obligatorio si no hubo vacunación;
- observación obligatoria para diferencias relevantes con el padrón;
- confirmación explícita antes de afectar stock y cobertura.

## Tablas necesarias

### series_acta

- identificador;
- campaña y UEL cuando formen parte del ámbito;
- prefijo y siguiente número;
- rango permitido;
- estado y vigencia.

### lotes_impresion_acta

- serie o entidad responsable;
- rango desde y hasta;
- cantidad de formularios;
- fecha de impresión;
- tipo y destino de cada ejemplar;
- cantidad de juegos y cantidad total de hojas;
- usuario responsable;
- estado del lote.

### formularios_acta

- identificador interno;
- serie y número;
- campaña, establecimiento, UEL y vacunador;
- instantánea de datos preimpresos;
- estado;
- fecha y usuario de emisión;
- cantidad de impresiones;
- estado de entrega o recepción de cada ejemplar cuando corresponda;
- motivo de anulación o extravío.

### impresiones_acta

- formulario;
- número de copia;
- fecha, usuario y motivo;
- huella del documento generado para poder demostrar qué versión se imprimió.

### actas_vacunacion

Contiene la información transcrita y validada de la intervención. Se vincula uno a uno con el formulario físico cuando se utilizó papel, pero permite en el futuro un origen completamente digital.

### adjuntos_acta

Fotografía o escaneo, tipo, fecha, usuario y referencia a almacenamiento. Su implementación debe definir conservación, acceso y protección de datos personales.

## Plantilla visual

El PDF del acta vigente es la referencia para posiciones, dimensiones, orden, rótulos y espacios manuscritos. La versión digital debe mantener esa geometría para que los tres ejemplares coincidan al utilizar papel carbónico. Los datos preimpresos ocupan las líneas y celdas existentes, sin agregar filas que desplacen el formulario.

El encabezado incorpora el logo institucional arriba a la derecha, alineado con el título y sin modificar la posición vertical del primer bloque del formulario.

Existe un prototipo A4 imprimible en [prototipos/acta-vacunacion.html](../prototipos/acta-vacunacion.html), reconstruido con la estructura del acta aportada. Antes de producción se debe realizar una prueba física a escala 100 % y superponer las tres hojas para calibrar las tolerancias de la impresora utilizada.

## Pendiente de validar con el acta actual

1. Campos y textos legales obligatorios.
2. Categorías animales impresas en campañas totales y parciales.
3. Firmas requeridas y uso de sello.
4. Tratamiento de tachaduras, correcciones y actas incompletas.
5. Necesidad de imprimir actas sin establecimiento asignado para contingencias.

## Tema postergado

El ámbito, administración y continuidad de la numeración se resolverán en una etapa futura. Esta definición no bloquea el diseño actual del formulario ni su lógica de carga.
