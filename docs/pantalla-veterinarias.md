# Padrón operativo de veterinarias

## Objetivo

Consultar las veterinarias propietarias de vacuna, sus centros habilitados y los principales saldos operativos de la campaña.

El [prototipo navegable](../prototipos/veterinarias.html) permite comparar cada veterinaria y abrir su [ficha detallada](pantalla-ficha-veterinaria.md).

## Regla central

La veterinaria es propietaria de la vacuna. La Fundación, la UEL principal y los subcentros mantienen su custodia física y administran los movimientos.

Por lo tanto:

- nunca se mezclan saldos de veterinarias diferentes;
- el mismo lote puede pertenecer a varias veterinarias, pero cada saldo se identifica por propietaria;
- una veterinaria puede tener vacuna físicamente en uno o más centros;
- los saldos de centros distintos permanecen separados hasta una transferencia confirmada;
- el vacunador que retira no se convierte en propietario de la vacuna.

## Alcance

- Un subcentro ve únicamente las veterinarias con relación vigente en ese centro.
- La sede principal ve las veterinarias de San Cristóbal y, con permiso, el consolidado de subcentros.
- Los administradores generales pueden consultar todos los centros.

Los filtros y totales siempre aplican el alcance del usuario en el servidor.

## Resumen superior

- veterinarias activas;
- frascos llenos disponibles;
- dosis sobrantes disponibles;
- dosis entregadas en remitos;
- dosis aplicadas en actas;
- dosis devueltas;
- diferencias pendientes;
- remitos sin conciliar.

Las unidades físicas no se mezclan: los frascos llenos se presentan como frascos y los sobrantes como dosis. Cuando se necesita comparar movimientos, el sistema muestra su equivalente en dosis sin perder la unidad original.

## Filtros

- nombre;
- estado;
- centro;
- campaña;
- lote o vencimiento;
- con stock disponible;
- con remitos pendientes;
- con diferencias de conciliación.

## Columnas

| Columna | Contenido |
|---|---|
| Veterinaria | Nombre e identificación interna. |
| Centros | Centros donde tiene relación vigente y stock. |
| Stock lleno | Frascos disponibles confirmados. |
| Sobrantes | Dosis disponibles de frascos abiertos. |
| Entregadas | Dosis retiradas mediante remitos durante el período. |
| Aplicadas | Dosis de actas confirmadas vinculadas a sus remitos. |
| Devueltas | Dosis llenas y sobrantes confirmadas. |
| Diferencia | Resultado acumulado pendiente de explicar. |
| Remitos abiertos | Entregados todavía no conciliados. |
| Estado | Activa, suspendida o inactiva. |
| Acción | Abrir ficha, stock o remitos. |

Una fila representa una veterinaria. Si opera en varios centros, el total visible depende del filtro y permite abrir el desglose por centro.

## Cálculos

### Stock disponible

Se obtiene del libro de movimientos confirmado, agrupado por:

`centro + veterinaria + lote + vencimiento + condición`

Para frascos llenos:

`frascos físicos − frascos reservados = frascos disponibles`

Para sobrantes:

`dosis físicas − dosis reservadas = dosis disponibles`

### Actividad de campaña

- entregadas: frascos llenos por 125 más sobrantes incluidos en remitos entregados;
- aplicadas: dosis por lote de actas confirmadas y vigentes;
- devueltas: frascos llenos por 125 más sobrantes confirmados;
- diferencia: se toma de las conciliaciones, sin alterar los valores reales.

Un remito con varios productores se cuenta una sola vez en entregadas. Sus diferentes actas suman las aplicaciones reales.

## Alta y edición

Campos iniciales:

- denominación;
- identificación fiscal, cuando corresponda;
- estado y vigencia;
- contactos privados;
- observaciones administrativas;
- centros habilitados y vigencia de cada relación.

Antes de crear se revisan coincidencias por denominación e identificación fiscal. Una veterinaria con historia no se elimina: se inactiva conservando movimientos y documentos.

Agregarla a un centro no mueve vacuna. Solo habilita la relación operativa; el stock aparece allí mediante una asignación o transferencia confirmada.

## Acciones

- `Nueva veterinaria`;
- `Abrir ficha`;
- `Ver stock`;
- `Ver remitos`;
- `Editar contactos`;
- `Administrar centros`, con permiso;
- `Suspender`, con motivo y permiso;
- `Exportar`, respetando alcance y privacidad.

No se editan saldos desde el padrón. Toda corrección de stock utiliza un movimiento autorizado y auditable.

## Orden inicial

1. diferencias pendientes;
2. remitos abiertos vencidos;
3. lotes próximos a vencer;
4. resto por denominación.

El color no sustituye el texto ni el valor numérico de la alerta.

La participación previa de cada veterinaria en una campaña se controla desde la [bandeja de convenios](convenios-veterinarias-campania.md). El convenio documenta las condiciones acordadas, pero no crea ni mueve stock.
