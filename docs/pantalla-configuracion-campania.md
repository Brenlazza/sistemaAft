# Pantalla de configuración y activación de campaña

## Objetivo

Centralizar los parámetros que rigen una campaña sin mezclarlos con las operaciones diarias. Desde esta pantalla los usuarios autorizados preparan fechas, tipo de campaña, centros participantes, categorías de referencia, reglas de control y tarifas antes de activarla.

La numeración definitiva de las actas continúa postergada y no impide configurar ni activar el resto del circuito.

## Estados

| Estado | Uso |
|---|---|
| Borrador | Permite completar y modificar la configuración. |
| Preparada | Superó los controles mínimos y está lista para revisión. |
| Activa | Admite remitos, preimpresiones, actas y movimientos operativos. |
| Cerrada | Queda disponible para consulta e informes; no admite nuevas operaciones comunes. |
| Anulada | Conserva historia, pero no puede utilizarse operativamente. |

Toda transición registra usuario, fecha y motivo. Reabrir una campaña cerrada requiere permiso especial y auditoría.

## Datos generales

- nombre visible;
- código interno;
- año y número de campaña;
- programa o enfermedad;
- tipo total o parcial;
- fecha prevista de inicio y finalización;
- fecha real de activación y cierre, completadas por el sistema;
- UEL responsable;
- estado;
- observaciones administrativas privadas.

Las fechas de vacunación fuera del período necesitan una excepción documentada, pero no se modifica la fecha original del acta.

## Centros participantes

Se seleccionan la sede principal y los subcentros que operarán durante la campaña. Para cada centro se conserva:

- fecha desde y hasta;
- responsable operativo;
- capacidad para recibir transferencias;
- capacidad para emitir remitos y actas;
- condición activa o inactiva.

Habilitar un centro en la campaña no concede acceso a usuarios. Los usuarios siguen necesitando una asignación explícita de campaña, centro y permisos.

## Categorías y cobertura

La campaña configura las categorías bovinas o bubalinas previstas y, para cada una:

- si integra la población objetivo;
- si es obligatoria como dato del acta;
- orden de visualización;
- regla de cobertura esperada;
- observación normativa.

Las cantidades importadas desde SENASA son estimativas. Nacimientos, cambios del rodeo, productores no incluidos, ausencia de RENSPA o vacunaciones por encima de lo previsto no bloquean la carga.

Si se vacuna una categoría no prevista o una cantidad excepcional, el sistema solicita motivo y autorización cuando corresponda, pero conserva la cantidad real. Las existencias de otras especies se cargan manualmente como dato complementario y no se deducen del padrón bovino.

## Reglas de actas

La configuración muestra:

- acta física por triplicado;
- preborrador por productor/destino confirmado del remito;
- alerta preventiva desde el día 5 sin devolución;
- acta demorada y bloqueo de nuevas preimpresiones desde el día 8;
- permiso requerido para una excepción al bloqueo;
- carga tardía con autorización y motivo;
- posibilidad de varias actas parciales para el mismo establecimiento.

Estos valores se guardan como versión de la campaña. Cambiarlos durante una campaña activa requiere motivo y no altera eventos ya calculados o resueltos.

El bloqueo por demora afecta al vacunador que conserva actas pendientes, no al productor ni al establecimiento.

## Tarifas

Cada campaña mantiene dos importes por animal:

- tarifa de veterinario;
- tarifa de idóneo.

Ambos importes pueden ser distintos entre sí. Permanecen fijos durante la campaña y pueden variar en la siguiente.

La pantalla guarda moneda, fecha de vigencia, responsable y versión. Las tarifas pueden corregirse mientras no hayan sido utilizadas en una liquidación confirmada. Después quedan congeladas; cualquier corrección se realiza mediante una nueva versión autorizada y ajustes comerciales sin reescribir facturas previas.

Configurar una tarifa no habilita a facturar: la emisión continúa dependiendo de permisos comerciales.

## Períodos administrativos

Los períodos son opcionales y se utilizan para informes. Cada uno contiene:

- etiqueta;
- fecha desde y hasta;
- orden;
- estado abierto o cerrado.

No se convierten automáticamente en semanas ISO. El sistema evita superposiciones dentro de la misma campaña.

## Control previo a la activación

### Requisitos mínimos

- identidad y tipo de campaña;
- fechas válidas;
- UEL y al menos un centro participante;
- categorías de referencia configuradas;
- reglas de actas vigentes;
- tarifas de veterinario e idóneo informadas si se utilizará la liquidación interna;
- al menos uno de los dos administradores disponible para supervisión.

### Avisos no bloqueantes

- padrón SENASA todavía no importado o parcialmente conciliado;
- convenios pendientes de firma o digitalización;
- veterinarias o vacunadores pendientes de completar;
- stock todavía no recibido o distribuido;
- numeración definitiva de actas pendiente.

Estos avisos requieren visibilidad y seguimiento, pero no deben transformarse en bloqueos de productores. El padrón puede completarse y conciliarse durante la campaña.

## Cierre

Antes de cerrar se informa, sin ocultar diferencias:

- actas demoradas, observadas o sin resolución;
- stock sin conciliar;
- transferencias o remitos abiertos;
- cajas pendientes de cierre o rendición;
- facturación pendiente;
- establecimientos del padrón sin resolución.

El cierre normal exige resolver los controles definidos como críticos. Una excepción de cierre necesita permiso superior, motivo y registro de auditoría.

## Permisos

- `campania.consultar`;
- `campania.crear`;
- `campania.modificar`;
- `campania.preparar`;
- `campania.activar`;
- `campania.cerrar`;
- `campania.reabrir`;
- `campania.configurar_tarifas`;
- `campania.configurar_reglas`.

Consultar una campaña no permite cambiar reglas ni tarifas.

## Decisiones pendientes

1. Categorías exactas exigidas por SENASA para cada tipo de campaña.
2. Autoridad que aprobará excepciones de categorías durante la campaña.
3. Controles que permitirán cierre excepcional con pendientes.
4. Ámbito y continuidad de la numeración de actas.
