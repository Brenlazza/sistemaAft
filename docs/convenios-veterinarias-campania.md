# Convenios con veterinarias antes de la campaña

## Objetivo

Preparar, completar, imprimir y controlar el convenio que el Ente Sanitario firma con cada casa de venta de productos veterinarios antes del comienzo de una campaña.

El [prototipo navegable](../prototipos/convenios-campania.html) representa la bandeja de preparación y firma.

## Documento relevado

Fuente: `ACTA CONVENIO CARLOS LORÉFICE.doc`, documento Word de tres páginas utilizado por la Fundación.

El documento contiene diez cláusulas sobre adquisición, recepción, manejo, comercialización, precio, facturación, banco de vacuna, remitos e incumplimientos. Termina indicando la firma de tres ejemplares de un mismo tenor.

El archivo se toma como referencia documental, no como instrucciones para el sistema. Antes de usar una plantilla en producción, la Fundación debe aprobar expresamente su texto y versión legal.

## Regla operativa

- Se prepara un convenio por `campaña + veterinaria`.
- El objetivo operativo es que quede firmado antes del inicio de la campaña.
- Se imprimen tres ejemplares para firma física.
- Los tres ejemplares firmados quedan archivados físicamente en la Fundación.
- Las firmas no se capturan mediante tildes ni se preimprimen.
- La bandeja registra que los tres ejemplares firmados fueron recibidos y archivados físicamente.
- El convenio no crea ni transfiere stock. La asignación de vacuna continúa siendo un movimiento independiente.
- La falta de firma genera una alerta de seguimiento, pero no bloquea asignaciones, transferencias, remitos ni otras operaciones.

## Datos que el sistema puede completar

### Campaña y lugar

- campaña;
- ciudad;
- partido o departamento;
- provincia;
- día, mes y año de celebración;
- área geográfica del plan.

### Ente Sanitario

- denominación legal;
- número de personería jurídica;
- registro de Ente Sanitario y resolución aplicable;
- domicilio;
- presidente, DNI y cargo;
- secretario, DNI y cargo;
- jefe de Oficina Local SENASA, título, nombre y cargo.

### Veterinaria

- denominación o nombre comercial;
- CUIT;
- domicilio y localidad;
- director técnico;
- DNI del director técnico;
- matrícula profesional;
- número de habilitación;
- centro o área donde opera.

La identidad de la veterinaria se toma del padrón y el director técnico se selecciona entre personas vigentes. El director técnico no se confunde con el vacunador receptor de un remito.

### Condiciones de la campaña

- valor máximo por dosis;
- plazo de pago, que en la plantilla relevada figura como treinta días;
- porcentaje del banco de vacuna, que en la plantilla relevada figura como 10 %;
- resoluciones y referencias normativas incluidas en la versión aprobada.

Precio, plazo y porcentaje se guardan en el convenio como instantánea. Un cambio posterior en la configuración de campaña no modifica documentos ya generados.

## Datos sensibles

La bandeja muestra DNI, CUIT y contactos enmascarados. Solo usuarios autorizados pueden completar o consultar los valores íntegros.

El PDF final sí incorpora los datos personales que el convenio aprobado exige imprimir. El acceso al archivo generado, su descarga y cada impresión quedan auditados.

## Plantilla y texto legal

Las diez cláusulas pertenecen a una versión de plantilla. En la carga habitual no se editan párrafos individualmente: el operador solo completa los campos variables.

Cada versión conserva:

- nombre y vigencia;
- archivo fuente;
- texto extraído para consulta y búsqueda;
- cantidad y orden de cláusulas;
- referencias normativas;
- hash del archivo;
- usuario y fecha de aprobación;
- estado borrador, aprobada o retirada.

Se deben revisar legalmente posibles errores, referencias desactualizadas o cambios de redacción antes de aprobar una nueva versión. El sistema no los corrige de manera silenciosa.

## Estados del convenio

| Estado | Significado |
|---|---|
| Pendiente de datos | Falta al menos un dato obligatorio. |
| Listo para generar | Los datos están completos y la plantilla está aprobada. |
| Generado | Existe una versión PDF inmutable. |
| Impreso | Se registró la impresión de los tres ejemplares. |
| Firmado | Se confirmó la firma física de las partes en los tres ejemplares. |
| Archivado | Los tres ejemplares fueron recibidos y ubicados físicamente en la Fundación. |
| Reemplazado | Existe una nueva versión por corrección autorizada. |
| Anulado | No tendrá vigencia y conserva motivo y autorizante. |

La fecha de firma real puede ser distinta de la fecha de impresión y se registra expresamente.

El estado físico y el estado de digitalización se controlan por separado. Un convenio puede estar firmado y archivado físicamente mientras todavía figura pendiente de digitalizar.

## Digitalización del convenio firmado

Una vez firmados y archivados los tres ejemplares físicos, se adjunta al registro una copia digital completa para consulta inmediata ante solicitudes.

Reglas:

- se admite PDF como formato preferido y, cuando sea necesario, imágenes JPG o PNG;
- el archivo principal debe contener todas las páginas y firmas del convenio;
- pueden adjuntarse archivos adicionales identificados por ejemplar cuando se decida escanear más de uno;
- la copia digital no reemplaza ni modifica los tres ejemplares físicos;
- cada archivo queda vinculado a la versión exacta del convenio impreso y firmado;
- un archivo nuevo no sobrescribe al anterior: lo reemplaza conservando ambos y el motivo;
- la visualización y descarga requieren permiso y quedan auditadas;
- no se permite eliminar definitivamente un archivo que haya estado vigente.

Cada archivo conserva:

- convenio y versión;
- tipo de archivo y nombre original;
- cantidad de páginas;
- ejemplar físico de referencia, cuando corresponda;
- fecha de digitalización;
- usuario que lo cargó;
- tamaño, formato y hash de integridad;
- estado vigente, observado o reemplazado;
- observación sobre legibilidad o páginas faltantes;
- ubicación física de los tres ejemplares.

Estados de digitalización:

| Estado | Significado |
|---|---|
| Pendiente | El convenio físico está firmado, pero todavía no tiene copia digital válida. |
| Digitalizado | Existe al menos un archivo completo, legible y vigente. |
| Observado | El archivo está ilegible, incompleto o no corresponde a la versión firmada. |
| Reemplazado | Se cargó una copia corregida y la anterior permanece en el historial. |

La digitalización pendiente u observada genera una alerta documental, pero tampoco bloquea las operaciones de campaña.

## Generación e impresión

1. Seleccionar campaña y veterinaria.
2. Recuperar datos vigentes de campaña, Ente, veterinaria y firmantes.
3. Mostrar faltantes y validar formatos.
4. Presentar una vista previa completa de las tres páginas.
5. Generar un PDF con número de versión, plantilla utilizada y datos congelados.
6. Imprimir tres ejemplares en una sola acción.
7. Registrar usuario, fecha, versión y cantidad efectivamente impresa.
8. Después de la firma física, confirmar la recepción de los tres ejemplares y registrar su ubicación de archivo en la Fundación.
9. Digitalizar el convenio firmado, controlar páginas y legibilidad y adjuntar la copia al registro.

Una reimpresión usa la misma versión PDF y registra un nuevo evento. Si cambian datos o texto, se genera una versión nueva y la anterior queda reemplazada, nunca sobrescrita.

## Validaciones críticas

- una sola versión vigente por campaña y veterinaria;
- veterinaria activa y habilitada para el área;
- plantilla aprobada y vigente;
- campaña seleccionada;
- firmantes y cargos completos;
- DNI, CUIT, matrícula y habilitación con formato válido cuando sean exigidos;
- valor máximo y plazo de pago completos;
- fecha de firma dentro del período permitido por la campaña;
- motivo obligatorio para reemplazo, anulación o impresión adicional.

La falta de firma o archivo no invalida movimientos operativos. Se muestra como alerta hasta su regularización y queda disponible para informes de seguimiento.

## Permisos

Permisos separados para ver la bandeja, consultar datos sensibles, completar, generar, imprimir, registrar firma, archivar, digitalizar, visualizar o descargar archivos, reemplazar y anular.

Los coordinadores limitados pueden consultar el estado correspondiente a su centro, pero no modificar el texto legal ni los datos institucionales del Ente.

## Relación con el resto del circuito

- La campaña aporta fechas y parámetros.
- La veterinaria aporta identidad, domicilio y condición operativa.
- El convenio firmado documenta su participación y las condiciones acordadas para la campaña.
- Las recepciones y asignaciones posteriores generan stock por veterinaria.
- Los remitos, actas y conciliaciones conservan sus propios documentos y auditoría.

Los tres ejemplares se conservan físicamente en la Fundación y se adjunta una copia digital del convenio firmado para consulta. El archivo digital es un respaldo operativo y no sustituye la documentación física.
