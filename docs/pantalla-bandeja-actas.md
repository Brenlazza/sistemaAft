# Bandeja de actas y control de demora

## Objetivo

Concentrar el seguimiento de todos los juegos de actas entregados, localizar rápidamente los formularios que regresan del campo y evitar que un vacunador continúe recibiendo nuevas actas cuando mantiene documentación demorada sin cargar.

El [prototipo navegable](../prototipos/bandeja-actas.html) representa la bandeja del Subcentro Ceres. La sede principal y los demás subcentros utilizan la misma pantalla con el alcance correspondiente a su sesión.

## Alcance del usuario

- Un operador de subcentro ve únicamente actas originadas en remitos de su centro.
- Un coordinador con alcance sobre varios centros puede cambiar entre los centros autorizados.
- Los administradores pueden consultar el conjunto completo.
- El alcance se vuelve a validar en el servidor al buscar, abrir, cargar o resolver un acta.

La limitación visual no reemplaza el control de permisos.

## Registro que alimenta la bandeja

Cada fila representa un juego de acta, no una vacunación ya realizada. Se crea desde un destino del remito y conserva:

- número de acta o identificador provisional mientras se resuelve la numeración;
- remito y versión que originaron la impresión;
- campaña y centro;
- veterinaria propietaria;
- vacunador responsable;
- productor, establecimiento y RENSPA;
- fecha y hora de impresión y entrega al profesional;
- estado físico y estado de la carga digital;
- días transcurridos desde la entrega;
- seguimientos y alertas generados.

## Resumen superior

La cabecera muestra contadores de pendientes, próximas a vencer, demoradas, observadas, confirmadas durante el día y vacunadores con preimpresión bloqueada. Todos respetan el centro y los demás filtros activos.

## Filtros y búsqueda

La bandeja permite combinar:

- número de acta o remito, productor, establecimiento o RENSPA;
- estado y antigüedad;
- vacunador y veterinaria;
- campaña y rango de fechas de entrega;
- centro, únicamente para usuarios con más de un centro autorizado.

Vistas rápidas: `Todas`, `Pendientes`, `Por vencer`, `Demoradas`, `Observadas` y `Confirmadas`.

## Columnas de la grilla

| Columna | Contenido |
|---|---|
| Acta | Número, o identificador provisional. |
| Remito | Número y versión vigente. |
| Entregada | Fecha de entrega del juego al profesional. |
| Antigüedad | Días completos transcurridos desde la entrega. |
| Vacunador | Profesional responsable de devolver el formulario. |
| Productor / establecimiento | Destino del preborrador. |
| RENSPA | Identificador del establecimiento. |
| Veterinaria | Propietaria de la vacuna retirada. |
| Estado | Situación física y digital. |
| Seguimiento | Última gestión y próxima acción. |
| Acción | Cargar, continuar, observar, resolver o consultar. |

Los resultados se ordenan inicialmente por prioridad y luego por mayor antigüedad.

## Estados visibles

| Estado | Significado |
|---|---|
| Preimpresa | Juego generado, todavía no entregado. |
| Entregada | En poder del vacunador y dentro del plazo. |
| Por vencer | Lleva entre 5 y 7 días sin resolución. |
| Demorada | Superó 7 días desde la entrega sin resolución. |
| Devuelta | Regresó el papel y espera transcripción. |
| Borrador | La transcripción comenzó, pero no fue confirmada. |
| Observada | Requiere corregir o resolver una inconsistencia. |
| Confirmada | La vacunación fue validada e imputada. |
| Devuelta sin utilizar | El juego regresó y se resolvió sin vacunación. |
| Anulada | Se cerró con motivo y permiso. |
| Faltante | No regresó y se abrió una incidencia formal. |

`Borrador`, `Observada` y `Faltante` no liberan por sí solos una demora. El juego deja de contar para el bloqueo solamente cuando queda `Confirmado`, `Devuelto sin utilizar` o `Anulado` mediante una resolución autorizada.

## Cómputo de antigüedad

El plazo comienza en la fecha y hora en que el remito registra la entrega del juego al vacunador.

- Días 0 a 4: seguimiento normal.
- Días 5 a 7: alerta preventiva `Por vencer`.
- Desde el comienzo del día 8: estado `Demorada` y bloqueo de nuevas preimpresiones para el vacunador.

La regla corresponde a días corridos. El valor se calcula, no se edita manualmente. Una reimpresión del mismo juego no reinicia el plazo; una nueva versión conserva la fecha de entrega que corresponda a su entrega física real.

## Bloqueo de preimpresión

Cuando un vacunador tiene al menos un juego demorado, el sistema impide generar o imprimir nuevos juegos destinados a ese profesional, aunque la acción la ejecute otro operador.

El bloqueo se verifica en el servidor al:

- generar preborradores desde un remito;
- abrir la vista previa de nuevas actas;
- usar `Imprimir remito y actas`;
- reintentar una impresión masiva.

El remito puede guardarse y continuar su preparación, pero no se entregan ni imprimen nuevas actas para ese vacunador hasta resolver la demora. Las consultas, la carga de actas pendientes, las devoluciones y la conciliación permanecen habilitadas.

La pantalla informa el vacunador bloqueado, la cantidad de actas demoradas, la más antigua, los días transcurridos, el centro y los remitos involucrados. El bloqueo no se aplica a todo el centro ni a otros vacunadores sin mora.

## Excepción autorizada

Un perfil con permiso específico puede autorizar una preimpresión excepcional. Debe registrar:

- actas demoradas que originan el bloqueo;
- motivo;
- cantidad de nuevos juegos autorizados;
- remito y destinos alcanzados;
- fecha de vencimiento de la autorización;
- usuario autorizante.

La excepción no elimina la mora ni reinicia sus días. Cada uso queda auditado y se muestra en los informes de cumplimiento. No se admite una autorización general o indefinida.

## Alertas y seguimiento

El sistema genera una alerta preventiva al día 5 y una alerta crítica al entrar en demora. Las alertas aparecen en la bandeja, el inicio del centro, el remito involucrado y el panel de coordinadores con alcance sobre ese centro.

Cada seguimiento registra:

- fecha y hora automáticas;
- medio: llamada, mensaje, correo, aviso personal u otro;
- resultado y nota operativa;
- próxima fecha de seguimiento;
- usuario que realizó la gestión.

Estos datos son privados, no se imprimen en el acta ni en el remito. Agregar un seguimiento no cambia el estado ni levanta el bloqueo.

Mientras la demora continúe, la alerta crítica permanece activa. Puede marcarse como atendida para evitar avisos duplicados durante el día, pero reaparece según la próxima fecha programada.

### Aviso por WhatsApp previsto

El perfil del veterinario o vacunador podrá guardar uno o más teléfonos de contacto. Para habilitar el acceso a WhatsApp se requiere un número activo, normalizado con código de país y marcado como autorizado para comunicaciones operativas.

La bandeja ofrecerá `Avisar por WhatsApp` y abrirá el chat del vacunador en WhatsApp o WhatsApp Web con un texto preparado. Antes de salir del sistema, el operador podrá revisar el destinatario y el mensaje. El aviso debe ser breve y no incluir datos personales del productor, RENSPA ni información sanitaria.

Mensaje inicial propuesto:

> Hola, [nombre]. La Fundación registra [cantidad] acta(s) de vacunación pendiente(s) de devolución o carga en [centro]. La más antigua lleva [días] días. Por favor, comuníquese con el centro para regularizar la documentación.

Esta primera integración solo abre el chat: el sistema no puede afirmar que el mensaje fue enviado, entregado o leído. Al regresar, el operador registra el resultado como seguimiento. Se conserva de forma privada quién abrió el enlace, cuándo, el teléfono utilizado y la versión de la plantilla, sin imprimir estos datos.

Una automatización posterior mediante WhatsApp Business deberá evaluarse por separado porque requiere proveedor, consentimiento, plantillas aprobadas y recepción de estados de entrega.

## Acciones por fila

- `Cargar acta`: abre la [pantalla de carga y confirmación](pantalla-carga-confirmacion-acta.md).
- `Continuar`: recupera una transcripción en borrador.
- `Registrar seguimiento`: agrega una gestión sin alterar el acta.
- `Avisar por WhatsApp`: abre el chat con un mensaje preparado cuando existe un teléfono autorizado; se implementará en una etapa posterior.
- `Marcar observada`: registra la incidencia y su motivo.
- `Devuelta sin utilizar`: resuelve el juego sin consumo ni cobertura.
- `Ver`: consulta una acta confirmada o anulada.

Las acciones masivas se limitan a seguimiento y exportación. No se permite confirmar varias actas sin revisar individualmente sus cantidades y lotes.

## Relación con el remito y la conciliación

Cada remito muestra juegos entregados, pendientes dentro de plazo, demorados, confirmados, resueltos sin utilizar y faltantes u observados.

No puede cerrarse la conciliación mientras exista un juego entregado sin resolución. La demora no modifica las cantidades por sí misma: solo eleva la prioridad, genera alertas y bloquea nuevas preimpresiones para el vacunador responsable.

## Auditoría e informes

Se conservan de forma privada los cambios de estado, alertas, seguimientos, inicio y finalización de cada bloqueo, excepciones autorizadas y utilizadas, usuario, fecha y origen de cada operación.

Los informes podrán medir tiempos promedio de devolución, cantidad de demoras por vacunador y centro, excepciones concedidas y reincidencias.

Las tablas, relaciones y restricciones que sostienen esta pantalla se detallan en el [modelo operativo de actas, demoras y seguimiento](modelo-operativo-actas.md).
