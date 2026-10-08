# Panel operativo del subcentro

## Objetivo

Dar a cada subcentro una entrada diaria simple y limitada a su propio ámbito. El usuario no elige libremente otro centro: el sistema determina el alcance desde su cuenta y lo muestra de forma visible durante toda la sesión.

El [prototipo navegable](../prototipos/inicio-subcentro.html) representa el inicio del Subcentro Ceres.

## Identidad y alcance

El encabezado muestra:

- nombre del usuario;
- rol operativo;
- subcentro activo;
- campaña activa;
- fecha del último acceso;
- opción de cerrar sesión.

Si una persona está excepcionalmente asignada a más de un subcentro, debe cambiar de contexto mediante una acción explícita. Cada operación conserva el centro activo y vuelve a validarlo en el servidor.

## Resumen diario

Las tarjetas principales presentan únicamente información del centro activo:

- frascos llenos disponibles;
- dosis sobrantes disponibles;
- vacuna en tránsito hacia el subcentro;
- transferencias pendientes de control;
- remitos entregados pendientes de devolución o conciliación;
- actas impresas pendientes de carga;
- alertas locales.

Frascos y dosis sobrantes se muestran por separado. Las cantidades en tránsito no se incluyen en el disponible.

## Trabajo pendiente

La bandeja prioriza acciones concretas:

1. recibir y controlar transferencias;
2. atender diferencias observadas;
3. preparar o entregar remitos locales;
4. registrar devoluciones;
5. cargar actas devueltas por los vacunadores;
6. conciliar retiros, vacunaciones y devoluciones;
7. revisar vencimientos y alertas de stock.

Cada tarea indica antigüedad, documento, veterinaria o profesional relacionado y acción permitida. Las tareas de otro centro nunca se descargan al navegador.

## Accesos rápidos

- `Recibir transferencia`;
- `Consultar mi stock`;
- `Nuevo remito`;
- `Registrar devolución`;
- `Cargar acta`;
- `Conciliar remito`;
- `Informes locales`.

Los accesos aparecen solamente cuando el usuario posee el permiso correspondiente. La falta de un botón no sustituye la validación de autorización al ejecutar la acción.

`Nuevo remito` fija el subcentro activo como lugar de salida y ofrece únicamente las veterinarias asociadas. No permite retirar del depósito principal ni de otro subcentro.

`Registrar devolución` utiliza la misma pantalla que la sede principal, pero solo permite abrir remitos emitidos por el subcentro activo. La devolución reingresa al mismo stock local.

## Stock local resumido

El panel muestra las existencias principales por veterinaria, lote y condición, con acceso al detalle de movimientos. No permite editar saldos.

Se destacan:

- lotes próximos a vencer;
- sobrantes disponibles;
- cantidades reservadas;
- transferencias todavía en tránsito;
- stock observado o bloqueado;
- diferencias pendientes de resolución superior.

## Actividad reciente

Incluye los últimos eventos operativos del centro: recepción de transferencia, emisión de remito, devolución, carga de acta y conciliación. Los nombres técnicos de usuarios y demás datos de auditoría privada requieren permiso específico.

## Funcionamiento con permisos limitados

El operador del subcentro no puede desde este panel:

- consultar la distribución global de la Fundación;
- abrir otro subcentro modificando una dirección o filtro;
- reasignar vacuna entre veterinarias;
- resolver diferencias que requieran autorización superior;
- ajustar o revertir movimientos;
- administrar usuarios, campañas o catálogos.

Cuando una acción requiere intervención de la principal, la tarea permanece visible como `Enviada a revisión`, sin conceder al operador permisos adicionales.

## Ausencia de campaña activa

Si no existe campaña activa, el usuario puede consultar historia e informes permitidos, pero no puede crear remitos, recibir vacuna ni registrar nuevas operaciones. El panel explica el motivo y no ofrece acciones inválidas.

