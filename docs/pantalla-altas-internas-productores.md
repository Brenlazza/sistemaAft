# Bandeja de altas internas de productores

## Objetivo

Reunir los productores y establecimientos creados durante la operación para completar datos, revisar coincidencias y, cuando corresponda, unificar identidades sin interrumpir la vacunación.

La bandeja es una herramienta de calidad de datos. No es una lista de habilitación y no determina quién puede vacunarse.

El [prototipo navegable](../prototipos/altas-internas-productores.html) muestra la consulta, los filtros y la revisión individual.

## Ingreso de registros

Entran automáticamente:

- altas rápidas creadas desde un remito;
- productores creados durante la carga de un acta;
- destinos sin RENSPA o identificación legal completa;
- identidades con posibles coincidencias detectadas posteriormente;
- establecimientos cuyo nombre o ubicación todavía están pendientes.

Un registro sale de las tareas pendientes cuando se completa o se marca como revisado. No se elimina del maestro ni pierde su historia.

## Indicadores

- altas internas de la campaña;
- disponibles para operar;
- con datos identificatorios pendientes;
- con información comercial o fiscal pendiente;
- con posibles coincidencias;
- revisadas y completadas.

Las cantidades sirven para organizar el trabajo. No generan bloqueos automáticos.

## Filtros y columnas

Filtros:

- texto por referencia, ID interno, RENSPA, documento o teléfono;
- campaña y centro de origen;
- estado de revisión;
- dato faltante;
- existencia de una coincidencia sugerida;
- actividad: sin uso, con remitos, con actas o con facturación.

Columnas principales:

| Columna | Contenido |
|---|---|
| ID y referencia | Identificador interno y nombre o referencia operativa. |
| Destino | Establecimiento, RENSPA y ubicación, aun cuando estén pendientes. |
| Origen | Campaña, centro, usuario y fecha del alta; el usuario se restringe por permiso. |
| Datos pendientes | Identificación, contacto, ubicación o información comercial faltante. |
| Actividad | Remitos, actas y demás documentos que ya utilizaron la identidad. |
| Coincidencias | Candidato sugerido y criterio; siempre informativo. |
| Estado operativo | Siempre disponible salvo una baja administrativa independiente y explícita. |
| Acción | Completar, revisar coincidencia, abrir ficha o ver historial. |

## Revisión individual

La ficha lateral o ventana de revisión conserva separados:

### Datos capturados en el alta

- referencia ingresada;
- centro y campaña;
- remito o acta que la originó;
- observación original;
- identificadores internos.

Estos datos no se sobrescriben silenciosamente.

### Identidad del productor

- tipo de persona, si se conoce;
- nombre o razón social;
- CUIT, CUIL, DNI u otro documento;
- teléfono y otros contactos privados;
- domicilio y datos comerciales.

### Establecimiento

- nombre;
- RENSPA;
- localidad, distrito y referencia rural;
- régimen del vínculo con el productor;
- vigencia.

Todos estos campos pueden guardarse parcialmente. `Guardar avance` nunca exige completar todo el conjunto.

## Posibles coincidencias

El sistema explica por qué propone cada candidato: nombre parecido, mismo teléfono, documento, RENSPA o ubicación. El usuario puede:

- confirmar que es el mismo productor y preparar una unificación;
- descartar la coincidencia con motivo;
- dejarla pendiente;
- indicar que todavía no hay evidencia suficiente.

No se permite una unificación automática basada solamente en similitud de nombre.

## Unificación auditada

Antes de confirmar se presenta una vista previa con:

- identidad que permanecerá como principal;
- alta interna que quedará como alias histórico;
- documentos, contactos y establecimientos que se conservarán;
- remitos, actas, facturas y eventos vinculados a cada identidad;
- contradicciones que necesitan resolución;
- motivo y usuario autorizante.

La unificación no reescribe los documentos históricos. Cada remito y acta conserva el identificador y la instantánea que tenía al emitirse, mientras la consulta actual permite llegar a la identidad principal.

Si hay documentos incompatibles, RENSPA diferentes o relaciones simultáneas dudosas, la confirmación de la unificación se detiene. El productor continúa disponible como registro separado hasta resolverlo.

## Estados de revisión

- `Pendiente`: todavía no fue revisado;
- `En revisión`: un usuario guardó avances o analiza coincidencias;
- `Completo`: se completó la información considerada disponible;
- `Revisado sin más datos`: se comprobó que actualmente no existen otros datos;
- `Unificado`: apunta a una identidad principal y conserva su alias e historia.

Estos estados no equivalen a habilitado o bloqueado. En todos ellos los documentos existentes siguen siendo válidos y consultables.

## Información fiscal pendiente

La bandeja puede generar una tarea comercial cuando falten datos necesarios para facturar. Esa tarea se atiende desde el circuito administrativo y no impide:

- emitir o modificar el remito;
- imprimir el preborrador del acta;
- registrar la vacunación;
- devolver vacuna;
- conciliar dosis.

## Permisos

- el subcentro consulta y completa las altas creadas dentro de su alcance;
- la sede principal puede revisar todos los centros;
- consultar documentos, teléfonos o datos fiscales requiere permiso específico;
- descartar coincidencias y preparar unificaciones requiere permiso de calidad de datos;
- confirmar una unificación requiere un permiso superior y queda auditado.
