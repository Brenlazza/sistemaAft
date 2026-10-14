# Alta rápida de productor desde un remito

## Objetivo

Permitir que la vacunación continúe cuando el productor o el establecimiento no aparece en el padrón SENASA, no tiene RENSPA o todavía no dispone de identificación legal completa.

El alta rápida crea un destino operativo real y reutilizable. No inventa un RENSPA, CUIT, nombre de establecimiento ni otro dato oficial para completar casilleros.

## Acceso

Desde `Nuevo remito > Productores y establecimientos > Agregar productor` se abre una ventana con dos caminos:

1. buscar y seleccionar un productor ya existente;
2. crear un productor operativo mediante `Alta rápida`.

El [prototipo de remito del subcentro](../prototipos/remito-subcentro.html) y el [prototipo general de remitos](../prototipos/remito-vacunas.html) muestran este comportamiento.

## Datos del alta rápida

### Obligatorio

- nombre, razón social o referencia con la que el personal reconoce al productor;
- motivo/origen, completado inicialmente como `Alta durante campaña`;
- campaña y centro que originaron el alta, heredados del remito;
- usuario y fecha, conservados en auditoría privada.

### Opcional

- nombre del establecimiento;
- RENSPA;
- CUIT, CUIL, DNI u otra identificación;
- localidad o referencia rural;
- teléfono de contacto;
- dosis o animales estimados para este retiro;
- observación operativa.

La ausencia de cualquiera de los datos opcionales no impide guardar, agregar el destino al remito, preimprimir el acta, registrar la vacunación ni conciliar el retiro.

## Identificación interna

Al guardar, el sistema asigna identificadores internos independientes de los documentos externos:

- `productor_id`, por ejemplo `PROD-INT-003498`;
- `establecimiento_id`, por ejemplo `EST-INT-003612`, si se crea un nuevo destino físico;
- `productor_establecimiento_id` para la relación operativa utilizada en el remito.

Si no se conoce el nombre del establecimiento, la interfaz muestra `Sin nombre informado`. Esa leyenda no se almacena como nombre oficial. Lo mismo se aplica a `No informado` para RENSPA o documento.

## Coincidencias y duplicados

Mientras se escribe, el sistema busca coincidencias por nombre, documento, RENSPA, teléfono y ubicación. Una coincidencia probable genera una advertencia y permite abrir el registro existente.

La advertencia nunca bloquea el alta. Si la persona en campo no puede confirmar la correspondencia, puede crear el registro interno y continuar. Después, un usuario autorizado podrá:

- completar los datos faltantes;
- vincularlo con una fila SENASA;
- unificarlo con un productor existente conservando la trazabilidad;
- mantenerlo como identidad independiente si corresponde.

## Resultado en el remito

`Guardar y agregar al remito` realiza una sola operación lógica:

1. crea el productor interno;
2. crea o selecciona su destino operativo;
3. registra el vínculo del productor con ese destino;
4. agrega la fila al remito actual y la deja seleccionada;
5. conserva los datos del remito aunque el usuario complete la ficha maestra más adelante.

La fila se identifica como `Alta interna · Disponible`. El estado SENASA continúa siendo una referencia informativa.

Después del alta, los datos pendientes se administran en la [bandeja de altas internas](pantalla-altas-internas-productores.md). La revisión posterior no modifica la disponibilidad operativa.

## Separación de la operación y la información fiscal

La falta de CUIT, condición fiscal o domicilio no bloquea la vacunación. Si esos datos son necesarios para emitir una factura, el sistema deja una tarea o alerta de completitud comercial vinculada al productor. La tarea fiscal no invalida el remito, el acta ni el consumo de vacuna ya registrados.

## Permisos y auditoría

- los usuarios de la sede principal y de cada subcentro pueden crear destinos operativos dentro de su alcance;
- los contactos y documentos quedan ocultos en impresiones y vistas sin permiso;
- toda alta conserva centro, campaña, usuario, fecha y valores ingresados;
- una unificación posterior nunca borra el identificador utilizado originalmente en remitos y actas.

## Validaciones

Bloquean solamente:

- referencia del productor vacía;
- intento de repetir exactamente el mismo destino dentro del remito actual;
- pérdida del alcance del usuario sobre el centro al momento de guardar.

No bloquean:

- productor ausente del padrón SENASA;
- RENSPA, documento, CUIT, teléfono o establecimiento no informados;
- coincidencia posible con otro registro;
- estimación SENASA inexistente o diferente de la vacunación real.
