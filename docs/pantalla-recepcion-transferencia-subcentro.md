# Recepción de transferencias en subcentros

## Objetivo

Permitir que cada subcentro, mediante sus propios usuarios y permisos limitados, controle físicamente la vacuna enviada desde San Cristóbal y confirme su ingreso al stock local.

El [prototipo navegable](../prototipos/transferencia-recepcion-subcentro.html) muestra la bandeja y el control de una transferencia.

## Separación de responsabilidades

- La sede principal prepara y despacha la transferencia.
- El sistema la deja en estado `En tránsito`; todavía no integra el stock disponible del subcentro.
- Un usuario habilitado del subcentro de destino registra el control físico.
- La aceptación conforme genera el ingreso al stock del subcentro.

El usuario que preparó o despachó desde la principal no puede confirmar en nombre del subcentro, salvo una contingencia administrada y auditada. La recepción normal debe quedar asociada a una cuenta individual del centro receptor.

## Acceso del subcentro

Cada usuario de subcentro tiene uno o más centros expresamente asignados. Al ingresar ve únicamente:

- transferencias dirigidas a sus centros;
- stock físico y movimientos de esos centros;
- veterinarias vinculadas a esos centros;
- remitos, actas, devoluciones y conciliaciones originados allí;
- informes operativos de su alcance.

No puede consultar cantidades de otras veterinarias, otros subcentros o el depósito principal, salvo información mínima del envío que debe recibir. El filtro de alcance se aplica en el servidor y no solamente ocultando opciones en pantalla.

## Bandeja de transferencias

Estados visibles:

- `Pendiente de salida`;
- `En tránsito`;
- `Pendiente de control`;
- `Observada`;
- `Recibida`;
- `Recibida con diferencia`, solamente mediante resolución autorizada;
- `Devuelta` o `Anulada`, cuando corresponda.

Cada elemento muestra número, origen, destino, fecha de salida, responsable del traslado, veterinaria propietaria, lotes, envases, dosis y tiempo transcurrido.

## Control físico

La pantalla conserva como solo lectura los valores declarados por la principal y permite registrar los valores comprobados en destino.

### Cabecera

- transferencia y campaña;
- origen y subcentro de destino;
- fecha y hora de salida;
- fecha y hora efectiva de recepción;
- transportista o responsable del traslado;
- temperatura de entrega registrada en origen;
- temperatura de recepción;
- responsable nominal que recibe, opcional;
- usuario autenticado que confirma, automático y privado;
- observaciones.

### Control por línea

Por cada combinación de veterinaria, marca, serie/lote y vencimiento:

- envases enviados;
- envases recibidos físicamente;
- dosis equivalentes;
- envases devueltos con vacuna, si corresponde al documento;
- dosis devueltas;
- envases vacíos devueltos;
- diferencia calculada;
- integridad de envases y conservadoras;
- coincidencia de marca, serie y vencimiento.

### Lista de comprobación

Antes de aceptar se confirma expresamente:

- identidad de todos los lotes;
- vencimientos;
- cantidad física;
- temperatura de recepción;
- integridad de envases y conservadoras;
- documentación y acta acompañante.

## Recepción conforme

La acción `Aceptar e ingresar al stock` se habilita cuando:

- todos los controles obligatorios están completos;
- las cantidades coinciden;
- no existen observaciones críticas;
- el usuario tiene permiso `Stock: recibir transferencia` para ese subcentro.

La confirmación es atómica:

1. registra fecha, hora, usuario y controles;
2. cambia la transferencia a `Recibida`;
3. crea los movimientos de entrada reales en el subcentro;
4. libera esas existencias para remitos locales;
5. conserva la veterinaria propietaria, lote, vencimiento y condición;
6. vincula la recepción con la salida original y el acta de subcentro.

Si cualquiera de estos pasos falla, no se modifica ningún saldo.

## Recepción con diferencias

Cuando existe una diferencia, el operador usa `Informar observación`. Debe registrar el valor real y el motivo, y puede adjuntar evidencia. La transferencia queda `Observada` y la vacuna no queda disponible para remitos.

La principal o un administrador autorizado resuelve mediante una de estas acciones:

- corregir documentalmente el envío mediante reversión y nuevos movimientos;
- ordenar devolución total o parcial;
- autorizar una recepción con diferencia.

Si se autoriza `Recibida con diferencia`, ingresa únicamente la cantidad física comprobada. La diferencia queda abierta y vinculada a la transferencia para investigación e informes; nunca se reemplaza el valor enviado para hacerlo coincidir.

## Documento impreso

La operación usa el formato oficial `Acta de entrega y recepción de vacunas — Subcentros Operativos`. El sistema puede imprimirla antes del traslado y reimprimirla con los valores de recepción.

Las firmas manuales permanecen en el documento. Usuario, fecha técnica, historial y decisiones internas son datos privados de auditoría y no se imprimen.

## Controles de concurrencia

- una transferencia no puede recibirse dos veces;
- al abrirla se vuelve a verificar que siga pendiente;
- la confirmación utiliza la versión vigente del envío;
- si la principal anula u observa mientras el subcentro controla, la aceptación se bloquea y se recargan los datos;
- ninguna cantidad en tránsito puede utilizarse en un remito.

