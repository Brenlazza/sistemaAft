# Ficha operativa de la veterinaria

## Objetivo

Explicar todo el stock y la actividad de una veterinaria desde la asignación de vacuna hasta los retiros, aplicaciones, devoluciones y conciliaciones.

El [prototipo navegable](../prototipos/ficha-veterinaria.html) muestra la Veterinaria Ceres Norte dentro del alcance del Subcentro Ceres.

## Encabezado

- denominación y estado;
- identificación fiscal, si existe;
- contactos privados;
- centros habilitados;
- campaña y período seleccionados;
- alertas de stock, vencimiento o conciliación.

La ficha diferencia la propiedad de la vacuna de su ubicación física. La misma veterinaria puede aparecer en varios centros, pero cada existencia conserva el centro donde está almacenada.

## Indicadores de stock actual

### Frascos llenos disponibles

Cantidad de frascos cerrados disponibles para nuevos remitos, calculada por centro y lote. Se muestra además su equivalente en dosis.

### Sobrantes disponibles

Dosis provenientes de frascos abiertos y devueltas a la misma veterinaria. Permanecen separadas de los frascos llenos y pueden utilizarse en nuevos remitos.

### En tránsito

Frascos o dosis enviados desde otro centro que todavía no fueron controlados y aceptados. No forman parte del stock disponible del destino.

### Reservado

Existencias comprometidas en operaciones preparadas pero todavía no entregadas. Se muestran para explicar por qué el saldo disponible puede ser menor que el físico.

## Indicadores de actividad

- dosis asignadas o recibidas durante el período;
- dosis entregadas a vacunadores;
- animales vacunados;
- dosis aplicadas;
- dosis devueltas llenas;
- dosis devueltas como sobrantes;
- diferencia acumulada de conciliaciones;
- remitos abiertos y conciliados.

Los animales vacunados y las dosis aplicadas se calculan por separado. En condiciones normales coinciden, pero su comparación permite detectar inconsistencias.

## Pestaña Stock

Desglose por:

- centro;
- marca;
- serie/lote;
- vencimiento;
- condición lleno/sobrante;
- físico;
- reservado;
- disponible;
- estado de vencimiento.

Cada fila enlaza al libro de movimientos. Un saldo nunca se modifica directamente.

## Pestaña Remitos

Muestra:

- número y fecha;
- centro de salida;
- vacunador receptor;
- productores o destinos;
- dosis retiradas llenas y sobrantes;
- dosis aplicadas;
- devoluciones;
- diferencia;
- estado de conciliación.

Un remito pertenece a una sola veterinaria, aunque incluya varios productores y establecimientos.

## Pestaña Vacunadores

Agrupa la actividad de los profesionales que retiraron vacuna de esta veterinaria durante el período:

- cantidad de remitos;
- dosis retiradas;
- animales vacunados;
- dosis devueltas;
- diferencia;
- actas pendientes y demoradas.

Esta tabla representa actividad por remitos, no una relación laboral o pertenencia permanente.

## Pestaña Movimientos

Cronología auditable de:

- recepción y asignación;
- transferencia entre centros;
- aceptación en destino;
- entrega por remito;
- devolución llena;
- devolución sobrante;
- rotura o decomiso en la UEL;
- ajuste autorizado;
- reversión.

Los movimientos confirmados no se editan ni se eliminan.

## Filtros

- campaña y período;
- centro;
- lote;
- condición;
- vacunador;
- estado de remito;
- solamente diferencias o vencimientos.

Cuando se selecciona `Todos los centros`, los totales se consolidan pero cada detalle conserva su ubicación.

## Contactos y centros

Los contactos de la veterinaria se almacenan por separado, con tipo, preferencia, vigencia y verificación. Son privados.

La relación con un centro incluye vigencia y estado. Activar una relación no crea stock; inactivarla no elimina movimientos históricos ni permite retirar existencias que aún permanezcan allí. Antes de cerrar la relación deben resolverse los saldos físicos.

## Datos legales y director técnico

La ficha también conserva los datos necesarios para preparar el convenio previo a cada campaña:

- denominación legal y CUIT;
- domicilio y localidad;
- director técnico;
- DNI y matrícula profesional;
- número de habilitación;
- fechas de vigencia de la designación.

El cambio de director técnico no sobrescribe períodos anteriores. Al generar un convenio, estos valores se copian como una instantánea y permanecen inmutables en esa versión impresa.

El estado del [convenio de campaña](convenios-veterinarias-campania.md) puede mostrarse en la ficha, sin confundirlo con la existencia o transferencia de stock.

## Reglas críticas

- una veterinaria nunca consume stock de otra;
- un remito usa stock de una sola veterinaria propietaria;
- los sobrantes regresan a la veterinaria, centro y lote del retiro original;
- una devolución superior a la esperada ingresa con su valor real y conserva la diferencia;
- las roturas y decomisos se descuentan solamente desde stock de UEL;
- solo actas confirmadas y vigentes aportan animales y dosis aplicadas;
- los borradores no afectan los acumulados;
- toda cifra permite navegar hasta los movimientos que la originan.

## Permisos

Permisos independientes para consultar, editar identidad/contactos, administrar centros, ver stock, emitir remitos, registrar devoluciones, cerrar conciliaciones, ajustar stock y consultar auditoría.

Un usuario de subcentro no puede ampliar su alcance agregando otra veterinaria o centro. Esa relación requiere un perfil administrativo.
