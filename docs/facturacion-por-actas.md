# Facturación por animales vacunados

## Regla confirmada

La facturación se calcula según la cantidad de animales vacunados registrada en cada acta confirmada.

Cada campaña tiene dos importes unitarios fijos por animal:

- tarifa de veterinario;
- tarifa de idóneo.

Ambas tarifas pueden ser diferentes entre sí y cambiar entre campañas. Dentro de una misma campaña permanecen fijas para cada tipo de profesional.

Fórmula base:

`total facturable del acta = animales vacunados facturables × tarifa de campaña correspondiente al tipo de profesional efectivo`

El tipo se toma del vacunador que efectivamente realizó la aplicación y quedó confirmado en el acta, no necesariamente del profesional previsto en el preborrador. La línea conserva la clasificación `Veterinario` o `Idóneo` utilizada para el cálculo.

## Qué cantidad se factura

La cantidad facturable se toma del total calculado de bovinos y bubalinos vacunados contra fiebre aftosa en el acta confirmada.

No se incluyen automáticamente:

- existencias declaradas de otras especies;
- estimaciones recibidas de SENASA;
- dosis retiradas en un remito;
- dosis devueltas o sobrantes;
- cantidades de brucelosis, carbunclo u otros programas.

Si en el futuro otro programa también se factura, tendrá su propio concepto y tarifa. No se mezclará silenciosamente con la tarifa de aftosa.

## Tarifa de campaña

La campaña conserva:

- moneda;
- importe unitario por animal para veterinarios;
- importe unitario por animal para idóneos;
- fecha de aprobación;
- vigencia desde/hasta;
- concepto que aparecerá en la factura;
- tratamiento impositivo configurado;
- resolución, convenio u observación de respaldo;
- usuario que carga y usuario que aprueba.

La tarifa puede editarse mientras la campaña está en preparación y todavía no se utilizó en una liquidación confirmada. Desde el primer cálculo confirmado queda congelada.

Una corrección posterior requiere una nueva versión autorizada. Las actas ya facturadas conservan la tarifa aplicada y cualquier diferencia se resuelve mediante un ajuste, nota de débito o nota de crédito según corresponda.

## Nacimiento del pendiente de facturación

Al confirmar un acta con vacunación realizada, el sistema crea una línea facturable idempotente vinculada a esa acta.

La línea guarda una instantánea de:

- campaña y tarifa aplicada;
- número y fecha del acta;
- cliente/productor y establecimiento;
- profesional efectivo y tipo `Veterinario` o `Idóneo`;
- cantidad de animales;
- importe unitario;
- subtotal;
- estado de los datos fiscales;
- versión vigente del acta.

Repetir la confirmación no puede crear otra línea para la misma versión del acta.

Un acta parcial genera su propia línea. Si el mismo establecimiento recibe varias vacunaciones parciales, cada acta aporta solamente sus animales vacunados y puede agruparse luego en la factura del mismo cliente.

## Cliente de facturación

El cliente se obtiene de la identidad comercial vinculada con el productor del acta. Puede provenir del [listado importado de facturación](importacion-clientes-facturacion.md) o de un alta posterior.

Si falta el vínculo o algún dato fiscal necesario:

- el acta queda confirmada y continúa afectando stock y cobertura;
- la línea queda `Pendiente de datos comerciales`;
- se crea una tarea en la bandeja de altas internas;
- no se emite todavía el comprobante fiscal;
- al completar los datos, la misma línea pasa a `Lista para liquidar`.

La falta de datos fiscales nunca invalida la vacunación.

## Bandeja de liquidación

La bandeja reúne actas confirmadas y permite filtrar por:

- campaña;
- fecha del acta;
- centro y subcentro;
- veterinaria y vacunador;
- cliente, productor, establecimiento o RENSPA;
- estado comercial;
- estado de facturación.

Columnas:

- acta y fecha;
- cliente de facturación;
- productor y establecimiento vacunado;
- animales facturables;
- tipo de profesional efectivo;
- tarifa de campaña;
- subtotal;
- estado fiscal;
- estado de liquidación;
- factura o comprobante asociado, cuando exista.

## Agrupación

Por defecto, una liquidación agrupa líneas compatibles del mismo:

- cliente de facturación;
- campaña;
- moneda;
- condición fiscal y tratamiento impositivo.

Una factura puede contener varias actas del mismo cliente y mostrar cada acta como detalle o adjunto. No se mezclan clientes diferentes en un comprobante.

Si una factura agrupa actas realizadas por veterinarios e idóneos, conserva líneas separadas con sus respectivas cantidades y precios unitarios. Nunca calcula una tarifa promedio.

El sistema también puede emitir una factura separada por acta si la administración lo decide. La regla debe quedar configurada antes de confirmar la liquidación.

## Estados

### Línea facturable

- `Pendiente de datos comerciales`;
- `Lista para liquidar`;
- `Incluida en liquidación`;
- `Facturada`;
- `Ajustada`;
- `No facturable`, únicamente con motivo y permiso.

### Liquidación

- `Borrador`;
- `Revisada`;
- `Confirmada`;
- `Emitida`;
- `Anulada`.

### Factura

- `Pendiente de emisión`;
- `Emitida`;
- `Rechazada por servicio fiscal`;
- `Anulada con comprobante correspondiente`.

## Rectificaciones y anulaciones

Una rectificación de acta nunca cambia silenciosamente una factura emitida:

- si todavía no fue facturada, recalcula la línea conservando la versión anterior;
- si fue facturada, calcula la diferencia entre cantidades e importes;
- genera una tarea para nota de crédito o débito;
- enlaza acta original, rectificación, factura y comprobante de ajuste.

Un acta anulada antes de facturar retira su línea de las pendientes. Si ya fue facturada, exige el circuito de anulación o nota de crédito aplicable.

## Implementación por etapas

### Etapa 1 — Liquidación interna

Es la opción inicial recomendada y de menor dificultad:

- configurar tarifa por campaña;
- generar pendientes desde actas confirmadas;
- controlar datos comerciales;
- agrupar por cliente;
- calcular totales;
- exportar un archivo o informe para el sistema de facturación actual;
- registrar manualmente el número de factura emitida.

### Etapa 2 — Factura administrada en el sistema

- numeración y puntos de venta;
- PDF de factura;
- cuentas corrientes, estados de cobro y notas de ajuste;
- controles impositivos y permisos.

### Etapa 3 — Emisión electrónica integrada

Es la etapa de mayor dificultad porque requiere integración con el servicio fiscal vigente, certificados, autorización electrónica, manejo de rechazos y cumplimiento normativo. El modelo queda preparado para incorporarla sin cambiar actas ni liquidaciones.

La implementación puede detenerse en la etapa 1 y seguir siendo útil, manteniendo la emisión legal en la herramienta actual hasta decidir la integración completa.

## Controles esenciales

- una versión de acta no puede facturarse dos veces;
- ninguna línea usa estimaciones SENASA como cantidad real;
- la tarifa aplicada y el tipo de profesional quedan congelados en la línea;
- el subtotal se calcula en el servidor;
- una factura no mezcla clientes;
- todo cambio de cantidad, tarifa, cliente o estado conserva auditoría;
- los usuarios de subcentro pueden consultar o preparar según permisos, pero la confirmación comercial puede reservarse a la sede principal;
- los datos fiscales no se imprimen en actas ni remitos.

La emisión de facturas y el cobro son permisos distintos. El futuro [flujo de cajas y cobranzas](flujo-cajas-cobranzas.md) permitirá que usuarios autorizados cobren comprobantes sin concederles necesariamente permiso para facturar.
