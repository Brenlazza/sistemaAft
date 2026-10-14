# Padrón operativo de vacunadores

## Objetivo

Consultar y administrar a los veterinarios e idóneos que intervienen en la campaña, detectar rápidamente bloqueos documentales y comparar su actividad confirmada.

El [prototipo navegable](../prototipos/vacunadores.html) funciona como entrada a la [ficha operativa individual](pantalla-ficha-vacunador.md).

## Diferencias importantes

- Un vacunador es una persona; una veterinaria es la propietaria de la vacuna.
- El vacunador no pertenece de forma permanente a la veterinaria mostrada. La relación surge de cada remito y su autorización.
- Un vacunador tampoco es necesariamente un usuario del sistema.
- La habilitación profesional no equivale al permiso de preimpresión: puede estar profesionalmente activo y bloqueado por actas demoradas.

## Alcance

- El subcentro ve la actividad del vacunador generada por remitos de su propio centro.
- Un coordinador ve únicamente los centros asignados.
- Los administradores generales pueden consolidar todos los centros.
- Los datos de contacto completos requieren permiso específico.

Los indicadores respetan el mismo período, centro y campaña seleccionados. Una fila por vacunador evita duplicar a la persona cuando operó con más de una veterinaria.

## Resumen superior

La pantalla muestra:

- vacunadores activos;
- vacunadores con preimpresión bloqueada;
- actas demoradas;
- animales vacunados;
- dosis retiradas;
- dosis devueltas;
- dosis pendientes de explicar.

Los valores agregados se calculan desde registros confirmados. No existen contadores editables.

## Filtros

- nombre o matrícula;
- tipo: veterinario o idóneo;
- estado profesional;
- estado de preimpresión;
- campaña;
- centro, dentro del alcance;
- veterinaria con la que operó durante el período;
- actividad con saldo pendiente o actas demoradas.

## Columnas

| Columna | Contenido |
|---|---|
| Vacunador | Nombre, matrícula y tipo. |
| Habilitación | Activo, próximo a vencer, suspendido o inactivo. |
| Contacto | Medio preferido enmascarado y estado de verificación. |
| Veterinarias del período | Propietarias de la vacuna en sus remitos del período. |
| Actas | Confirmadas, pendientes y demoradas. |
| Animales vacunados | Bovinos/bubalinos de actas confirmadas. |
| Dosis retiradas | Entregas confirmadas de frascos llenos y sobrantes. |
| Dosis devueltas | Devoluciones confirmadas llenas y sobrantes. |
| Pendiente | Retiradas menos aplicadas menos devueltas. |
| Preimpresión | Habilitada o bloqueada por demora. |
| Acción | Abrir ficha o actas pendientes. |

## Orden y alertas

El orden inicial es:

1. vacunadores bloqueados por actas demoradas;
2. saldos pendientes de explicar;
3. habilitaciones próximas a vencer;
4. resto por nombre.

El color ayuda a priorizar, pero siempre se muestra texto y cantidad. Un bloqueo incluye acceso directo a las actas que lo originan.

## Alta de un vacunador

Campos mínimos:

- nombre completo;
- matrícula o identificación equivalente;
- tipo profesional;
- vigencia de la habilitación;
- estado inicial;
- teléfono u otro contacto, opcional;
- fuente y observaciones administrativas privadas.

Antes de crear, el sistema busca coincidencias por matrícula y nombre normalizado. Una matrícula activa no puede repetirse. Un registro histórico inactivo no se reactiva creando otro: se revisa la identidad existente y se agrega una nueva vigencia cuando corresponda.

El teléfono importado o ingresado no queda automáticamente habilitado para WhatsApp. Debe normalizarse, verificarse y contar con autorización operativa.

## Edición

Se puede actualizar:

- nombre corregido, conservando historial cuando sea relevante;
- tipo y vigencia;
- estado profesional con motivo;
- contactos;
- observaciones administrativas.

No se cambia desde esta pantalla:

- cantidad de animales vacunados;
- dosis retiradas o devueltas;
- actas demoradas;
- estado de bloqueo documental.

Estos datos se recalculan desde actas, remitos, devoluciones y formularios. No existe `Desbloquear` manual.

## Suspensión o baja

Un vacunador con historia no se elimina. Suspender o dar de baja exige motivo, fecha efectiva y permiso administrativo.

La suspensión impide asignarlo a nuevos remitos o actas, pero conserva consultas, cargas pendientes y conciliaciones históricas. Las operaciones anteriores continúan vinculadas a la misma identidad.

## Acciones

- `Nuevo vacunador`;
- `Abrir ficha`;
- `Ver actas pendientes`;
- `Registrar seguimiento`;
- `Editar datos`;
- `Suspender`, con permiso;
- `Exportar`, respetando privacidad y alcance.

## Cálculos

Para cada fila y para el total general se aplican las reglas de la ficha individual:

- animales vacunados: categorías aftosa de actas confirmadas y vigentes;
- retiradas: frascos llenos por 125 más dosis sobrantes de remitos entregados;
- devueltas: frascos llenos por 125 más sobrantes de devoluciones confirmadas;
- aplicadas: dosis por lote de actas confirmadas;
- pendiente: retiradas menos aplicadas menos devueltas.

Un remito con varios productores suma una sola vez en retiradas. Cada acta aporta únicamente su aplicación. Las reversiones y rectificaciones afectan mediante movimientos o versiones vigentes, sin borrar historia.

## Permisos

Permisos independientes:

- consultar padrón;
- ver indicadores operativos;
- ver contactos completos;
- crear o editar identidad;
- verificar contactos;
- cambiar habilitación profesional;
- registrar seguimientos;
- exportar datos;
- consultar auditoría.

Los coordinadores limitados pueden consultar y dar seguimiento dentro de su centro, pero no necesariamente modificar la identidad profesional o suspender una habilitación.
