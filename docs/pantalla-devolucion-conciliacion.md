# Pantalla de devolución y conciliación de remitos

## Objetivo

Recibir las actas que regresan del campo, registrar frascos llenos y sobrantes devueltos y comprobar que toda la vacuna retirada quede explicada mediante vacunación o devolución.

La conciliación se calcula por serie/lote y también como total del remito. Cuadrar solamente el total no alcanza si existen diferencias entre lotes.

El [prototipo navegable](../prototipos/remito-devolucion-conciliacion.html) permite modificar dosis vacunadas, frascos llenos y sobrantes para observar el recálculo automático. Una diferencia no impide registrar lo ocurrido: exige motivo y permite cerrar como `Conciliado con diferencia`.

## Acceso

La pantalla se abre desde un remito `Entregado`, `En carga`, `Cuadrado` u `Observado`. La cabecera muestra bloqueados:

- número y versión vigente del remito;
- campaña y UEL;
- veterinaria propietaria;
- profesional receptor;
- lotes y frascos retirados;
- cantidad de destinos y juegos de acta entregados;
- estado de firma del remito.

No puede cerrarse la conciliación si la versión vigente del remito está pendiente de nueva firma.

## 1. Recepción de documentación

La grilla presenta una fila por preborrador entregado:

| Campo | Uso |
|---|---|
| Acta/formulario | Número del juego físico. |
| Productor y RENSPA | Destino del remito. |
| Estado físico | Entregado, devuelto completo, devuelto sin utilizar, faltante. |
| Fecha efectiva | Se carga desde el papel. |
| Lote utilizado | Debe pertenecer al remito, salvo excepción autorizada. |
| Dosis vacunadas | Total calculado desde las categorías del acta. |
| Estado digital | Pendiente de carga, borrador, observada o confirmada. |

Acciones por fila:

- `Registrar devolución del papel`;
- `Cargar acta`;
- `Abrir acta`;
- `Marcar sin utilizar`;
- `Informar faltante`.

Solo las actas confirmadas y vigentes aportan dosis a la conciliación. Recibir el papel o guardar un borrador no modifica el cálculo sanitario.

Todos los juegos entregados deben tener una resolución antes del cierre: acta confirmada, devuelto sin utilizar, anulado con motivo o faltante formalmente observado.

## 2. Devolución de frascos llenos

Se registra una fila por lote:

- serie/lote;
- vencimiento;
- frascos entregados originalmente;
- frascos llenos devueltos;
- equivalente automático en dosis;
- fecha de devolución;
- responsable de recepción en la UEL;
- observación.

La cantidad debe ser entera, no negativa y no puede superar los frascos retirados de ese lote menos cualquier devolución llena ya confirmada.

Al confirmar, los frascos regresan al stock lleno de la misma veterinaria, UEL y lote.

## 3. Devolución de sobrantes

Cada sobrante de frasco abierto se registra por separado con:

- lote;
- cantidad remanente en dosis;
- acta de origen;
- cliente/productor y establecimiento, completados desde el acta;
- profesional que devuelve;
- fecha;
- responsable que recibe en la UEL;
- observación.

La cantidad debe ser mayor que cero y menor que 125 por frasco abierto. Si se devuelven sobrantes provenientes de más de un frasco o acta, se cargan en líneas separadas para conservar su origen.

El sobrante ingresa al stock de sobrantes de la veterinaria propietaria. Nunca vuelve automáticamente al stock de frascos llenos.

## 4. Dos controles por lote

Para cada serie/lote:

- `devolución esperada = dosis retiradas - dosis vacunadas`
- `uso inferido = dosis retiradas - dosis devueltas reales`
- `variación de devolución = dosis devueltas reales - devolución esperada`
- `variación de uso = uso inferido - dosis vacunadas`

Las dos variaciones representan la misma diferencia desde lados opuestos:

`variación de uso = dosis retiradas - dosis vacunadas - dosis devueltas reales`

`variación de devolución = - variación de uso`

La pantalla muestra por lote:

| Lote | Retiradas | Vacunadas | Devolución esperada | Devolución real | Variación |
|---|---:|---:|---:|---:|---:|

Interpretación de la variación de devolución:

- cero: coincidencia exacta;
- positiva: se devolvieron más dosis que las esperadas;
- negativa: se devolvieron menos dosis que las esperadas.

Cada lote se analiza por separado. No se compensa automáticamente una diferencia positiva de un lote con una negativa de otro, aunque el total general resulte cero.

## 5. Total del remito

El resumen general muestra:

- dosis retiradas;
- dosis vacunadas en actas confirmadas;
- dosis devueltas en frascos llenos;
- dosis devueltas como sobrante;
- devolución esperada y devolución real;
- variación de devolución;
- uso inferido y dosis vacunadas declaradas;
- variación de uso;
- actas o formularios todavía pendientes;
- estado documental de la firma vigente.

Estados calculados:

- `Pendiente`: aún no comenzó el regreso;
- `En carga`: existen actas o devoluciones parciales;
- `Cuadrado`: cada lote da cero y toda la documentación está resuelta;
- `Observado`: hay diferencias, faltantes, lotes inconsistentes o documentación incompleta;
- `Conciliado`: un usuario autorizado cerró un remito cuadrado;
- `Conciliado con diferencia`: un usuario autorizado cerró el circuito conservando una diferencia explicada;
- `Anulado`: se aplicó el procedimiento de anulación auditada.

## 6. Diferencias

Si una diferencia no es cero:

- se resalta el lote y el total;
- se solicita un motivo y una explicación;
- el resultado calculado no es editable;
- no se ofrecen rotura ni decomiso como motivo de descargo;
- el remito queda `Observado` mientras se revisa;
- se permite registrar las cantidades reales aunque superen las esperadas;
- la devolución real incrementa el stock correspondiente, sin corregirla para forzar un resultado cero;
- un usuario autorizado puede cerrar como `Conciliado con diferencia` cuando el motivo y la explicación estén completos.

Las roturas y decomisos continúan siendo bajas exclusivas del stock físico que permanece en la UEL.

## 7. Cierre

`Cerrar conciliación` sin diferencias se habilita cuando:

- todos los lotes tienen diferencia cero;
- todos los formularios entregados están resueltos;
- las actas que declaran vacunación están confirmadas;
- las devoluciones están confirmadas;
- la versión vigente del remito tiene firma registrada;
- no existen observaciones bloqueantes.

Si existe diferencia, la acción cambia a `Cerrar con diferencia` y requiere:

- motivo catalogado;
- explicación obligatoria;
- usuario con permiso especial;
- confirmación explícita del valor positivo o negativo;
- toda la restante documentación resuelta.

El cierre guarda una instantánea de retiradas, vacunadas, devoluciones, devolución esperada, uso inferido y ambas variaciones. Una modificación posterior exige reapertura autorizada y queda auditada; no se sobrescribe el cierre anterior.

## Permisos

- consultar la conciliación;
- registrar recepción de actas;
- cargar actas en borrador;
- confirmar actas;
- registrar devolución llena;
- registrar sobrantes;
- observar diferencias;
- resolver observaciones;
- cerrar o reabrir la conciliación.

## Informes derivados

- remitos pendientes de documentación;
- actas entregadas y todavía no devueltas;
- diferencias por profesional, veterinaria y lote;
- frascos llenos devueltos;
- sobrantes por veterinaria, productor, establecimiento y lote;
- tiempo entre retiro, devolución y conciliación;
- remitos cuadrados pendientes de revisión final.
