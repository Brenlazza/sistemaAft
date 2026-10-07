# Flujo de remitos y conciliación de vacuna

## Regla confirmada

La presentación es un frasco de 125 cc equivalente operativamente a 125 dosis. El stock lleno se controla por frascos y, para conciliar cada retiro, también se calcula su equivalente en dosis.

`dosis teóricas retiradas = cantidad de frascos llenos × 125`

Cada retiro pertenece a una veterinaria propietaria y a un profesional receptor, pero puede destinarse a uno o más productores/establecimientos. El remito, sus destinos, las actas y las devoluciones deben quedar vinculados para poder reconstruir el circuito completo.

## Flujo completo

### 1. Preparar el retiro

La veterinaria propietaria autoriza que un vacunador/veterinario retire vacuna de su stock almacenado en la UEL. Se registran:

- veterinaria propietaria;
- uno o más destinos, cada uno formado por cliente/productor canónico, registro SENASA de la campaña, establecimiento y RENSPA;
- campaña;
- profesional autorizado;
- serie/lote y vencimiento;
- cantidad prevista de animales/dosis por destino, cuando se conozca;
- cantidad de frascos llenos a retirar;
- autorización firmada.

La cantidad de frascos sugerida puede calcularse como:

`frascos necesarios = redondear hacia arriba(dosis previstas / 125)`

El operador puede modificar la sugerencia con motivo cuando existan circunstancias operativas especiales. La previsión puede distribuirse entre varios destinos y no representa todavía consumo real.

### 2. Emitir y entregar el remito

El sistema genera el remito. Al entregar la vacuna:

1. se valida el stock lleno de la veterinaria propietaria;
2. se imprimen veterinaria, profesional, lote, cantidad y los destinos previstos;
3. el profesional receptor firma el retiro;
4. el remito pasa a `Entregado`;
5. los frascos salen del stock lleno en UEL y pasan a custodia del profesional.

Un remito puede corresponder a varios productores/establecimientos. Los destinos seleccionados quedan como líneas del remito y la conciliación se realiza sobre la suma de todas las actas vinculadas.

Ejemplo:

- retiro: 1 frasco = 125 dosis;
- productor A: 50 animales vacunados;
- productor B: 50 animales vacunados;
- productor C: 25 animales vacunados;
- devolución: 0;
- diferencia del remito: 0.

Si aparece un destino no previsto después del retiro, puede agregarse antes de cerrar la conciliación, siempre que corresponda a la misma campaña, profesional y circuito de vacuna. La incorporación genera una nueva versión completa del remito, que se imprime y firma nuevamente. La versión y firma anteriores se conservan para auditoría y la nueva versión queda identificada como `REIMPRESIÓN MODIFICADA`.

### 3. Realizar la vacunación

El profesional aplica la vacuna y completa el acta. La cantidad vacunada por categorías determina las dosis aplicadas:

`dosis vacunadas = suma de animales vacunados en el acta`

Las excepciones autorizadas de categorías bovinas o bubalinas también forman parte de esta suma.

Cada acta debe vincularse al remito utilizado y a uno de sus destinos. Un acta corresponde a un único productor/establecimiento; un remito puede reunir muchas actas, incluso de productores diferentes.

Una acta parcial mantiene su propio número, fecha, lote y dosis. Puede utilizar el mismo remito que otras actas si todas consumieron vacuna de ese retiro.

### 4. Devolver vacuna

Después de la visita pueden regresar dos tipos de existencia:

- **Frascos llenos sin abrir:** vuelven al stock lleno de la misma veterinaria, lote y vencimiento.
- **Sobrante de frasco abierto:** ingresa al stock de sobrantes de la misma veterinaria y conserva productor, establecimiento, acta, lote, profesional y cantidad remanente.

Ejemplo confirmado:

- retiro: 1 frasco = 125 dosis;
- vacunación: 100 dosis;
- devolución como sobrante: 25 dosis;
- diferencia: 0.

Los sobrantes nunca se convierten nuevamente en frascos llenos.

### 5. Conciliar el remito

La conciliación utiliza:

`diferencia = dosis retiradas - suma de dosis vacunadas en actas vinculadas - dosis devueltas en frascos llenos - dosis devueltas como sobrante`

La devolución de un frasco lleno equivale a 125 dosis.

Solo se suman actas confirmadas y vigentes. Un acta anulada deja de integrar el cálculo mediante su reversión auditada.

Las roturas y los decomisos no justifican diferencias de un remito entregado. Se registran únicamente como bajas separadas del stock que permanece físicamente en la UEL, con motivo y autorización.

El resultado esperado es cero. Si no coincide:

- el remito queda `Observado`;
- se muestra el valor de la diferencia;
- se exige motivo y observación;
- no se cierra la conciliación automáticamente;
- la resolución queda reservada a un usuario autorizado;
- cualquier ajuste genera un movimiento auditable.

## Estados de conciliación

| Estado | Significado |
|---|---|
| Pendiente | El remito fue entregado y todavía no regresaron acta/devoluciones. |
| En carga | Se registraron datos parciales. |
| Cuadrado | La diferencia calculada es cero. |
| Observado | Existe diferencia o documentación incompleta. |
| Conciliado | Un usuario autorizado revisó y cerró el circuito. |
| Anulado | El remito fue anulado mediante un procedimiento auditado. |

Un remito `Cuadrado` todavía puede requerir revisión documental antes de pasar a `Conciliado`.

## Casos contemplados

### Vacunación exacta

Se retiran 2 frascos (250 dosis), se vacunan 250 animales y no hay devolución. Diferencia cero.

### Vacunación con sobrante

Se retira 1 frasco (125 dosis), se vacunan 100 animales y se devuelven 25 dosis como sobrante. Diferencia cero.

### Devolución de frasco lleno

Se retiran 2 frascos (250 dosis), se vacunan 100 animales, se devuelve 1 frasco lleno (125 dosis) y un sobrante de 25 dosis. Diferencia cero.

### Rotura o decomiso en la UEL

Una rotura o decomiso se registra con un movimiento propio sobre el stock físico de la UEL. Reduce la disponibilidad de la veterinaria propietaria, pero no se vincula como descargo de un remito entregado ni integra su ecuación de conciliación.

### Diferencia sin justificar

Si las dosis retiradas no coinciden con la suma de vacunaciones y devoluciones, el sistema conserva la diferencia y no permite ocultarla mediante la edición del saldo.

## Datos mínimos de la conciliación

- remito;
- veterinaria propietaria;
- profesional receptor;
- uno o más destinos de productor/establecimiento vinculados al padrón y al cliente canónico;
- campaña;
- serie/lote;
- frascos y dosis retiradas;
- actas vinculadas y dosis vacunadas;
- frascos llenos devueltos;
- dosis sobrantes devueltas;
- diferencia calculada;
- estado;
- observación y motivo cuando corresponda;
- usuario y fecha de revisión privada.

## Informes necesarios

El sistema debe poder informar por período, campaña, veterinaria, profesional, productor, establecimiento y lote:

- frascos y dosis recibidas;
- frascos y dosis retiradas mediante remito;
- dosis vacunadas;
- frascos llenos devueltos;
- dosis sobrantes devueltas;
- roturas y decomisos;
- diferencias pendientes;
- remitos pendientes, cuadrados, observados y conciliados;
- stock lleno actual;
- stock de sobrantes actual.

Los informes agregados deben permitir abrir el detalle hasta llegar al remito, acta y movimientos que originan cada valor.

## Reglas de integridad

- un remito entregado tiene exactamente una veterinaria propietaria, un profesional receptor y uno o más destinos de productor/establecimiento;
- todo destino debe vincular un cliente/productor canónico con el registro SENASA y establecimiento correspondiente;
- toda acta confirmada que consume vacuna debe indicar el remito y uno de sus destinos;
- un remito puede vincular muchas actas y la conciliación utiliza la suma de sus dosis confirmadas y vigentes;
- agregar un destino a un remito entregado incrementa su versión y exige reimpresión y nueva firma antes de conciliar;
- un acta no puede descontar dos veces las mismas dosis;
- una devolución debe utilizar la misma veterinaria y lote del retiro;
- las cantidades nunca son negativas;
- la diferencia se calcula, no se escribe manualmente;
- un ajuste no modifica movimientos anteriores: agrega un nuevo movimiento vinculado;
- cerrar una campaña requiere resolver o autorizar todos los remitos observados.

La distribución y el comportamiento de la interfaz se detallan en [pantalla de remitos](pantalla-remitos.md).
