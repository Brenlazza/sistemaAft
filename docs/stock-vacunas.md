# Relevamiento de stock y distribución de vacunas

## DISTRUBUCION

Fuente: captura de Vista Diseño de la tabla vinculada `DISTRUBUCION1`. El nombre original conserva la grafía `DISTRUBUCION`.

La propiedad de la tabla confirma como origen `D:\DiscoDpc2\aftosa 31\datos.mdb`, tabla `DISTRUBUCION`.

| Campo | Tipo en Access | Clave | Observación |
|---|---|---|---|
| Marca | Texto corto | Sí | Tamaño 15, requerido, no permite longitud cero. |
| ID_UEL | Número | Sí | Referencia a UEL según el diagrama de relaciones. Subtipo numérico pendiente. |
| Fecha Entrega | Fecha/Hora | Sí | Fecha de entrada o entrega; el sentido exacto debe confirmarse con el formulario de stock. |
| Fecha Vencimiento | Fecha/Hora | No | Vencimiento de la vacuna. |
| Serie | Número | No | Probable serie o lote. Confirmar subtipo, reglas y si históricamente pudo contener ceros iniciales o caracteres. |
| Cantidad | Número | No | Cantidad recibida o distribuida. Falta confirmar unidad: dosis, frascos, cajas u otra. |

La clave primaria compuesta es `(Marca, ID_UEL, Fecha Entrega)`. Aunque la propiedad individual visible de Marca indica `Indexado: No`, Access muestra los tres primeros campos con icono de llave; la clave compuesta debe verificarse en Índices.

### Lectura funcional provisional

La tabla parece registrar entregas o ingresos de una marca de vacuna a una UEL, con vencimiento, serie y cantidad. El diagrama anterior confirma una relación de UEL uno a muchos con DISTRUBUCION.

Limitaciones que no deben copiarse automáticamente al nuevo esquema:

- La clave no incluye `Serie`; no permite representar claramente dos series de la misma marca recibidas por la misma UEL en la misma fecha.
- `Marca` forma parte de la identidad del movimiento como texto libre de hasta 15 caracteres. En el nuevo sistema debería referenciar un catálogo validado de productos o presentaciones.
- `Serie` está guardada como número. Un lote moderno puede requerir letras o ceros iniciales, por lo que se propone almacenarlo como texto tras validar los datos reales.
- Una sola columna `Cantidad` no define unidad ni permite convertir de frascos a dosis.
- La tabla no expone un identificador único de movimiento, proveedor, laboratorio, documento de recepción, usuario ni fecha de registro.

### Propuesta para el nuevo modelo

Separar:

- producto o vacuna;
- lote/serie y vencimiento;
- presentación y unidad;
- ubicación o responsable de stock;
- movimiento de recepción, transferencia, devolución, pérdida o ajuste;
- detalle de cantidades por lote.

Cada movimiento debería tener una clave interna, fecha efectiva, origen, destino, comprobante, responsable y estado. Las cantidades no se sobrescribirían: el stock se obtendría de movimientos trazables.

## Pendiente del circuito

- Formulario `Stock-vete` en Vista Diseño para conocer el origen del registro, controles y reglas.
- Subtipos y propiedades de ID_UEL, Serie y Cantidad.
- Significado operativo exacto de Fecha Entrega y unidad de Cantidad.
- Relación con LABORATORIO y catálogo completo de marcas.

## DISTRI-VETE

Fuente: captura de Vista Diseño de la tabla vinculada `DISTRI-VETE1`. La propiedad de tabla confirma el origen `D:\DiscoDpc2\aftosa 31\datos.mdb`, tabla `DISTRI-VETE`.

| Campo | Tipo en Access | Observación |
|---|---|---|
| Marca | Texto corto | Tamaño 15, no requerido, indexado con duplicados y sin compresión Unicode. |
| Id_UEL | Número | UEL que realiza la entrega; subtipo pendiente. |
| Matricula | Texto corto | Vacunador receptor según la relación observada con VACUNADOR.Matricula. |
| Cantidad | Número | Cantidad entregada; unidad pendiente. |
| Fecha Entrega | Fecha/Hora | Fecha de entrega al vacunador. |

No se observa ningún icono de llave: la tabla no tiene una clave primaria visible. Esto permite registros idénticos y dificulta corregir, auditar o referenciar una entrega específica.

### Lectura funcional

El flujo histórico parece ser:

1. DISTRUBUCION registra el ingreso o disponibilidad por marca, UEL, vencimiento y serie.
2. DISTRI-VETE registra la entrega por marca desde una UEL a un vacunador identificado por matrícula.
3. VACUNA registra vacunas suministradas y aplicadas en el establecimiento.

Esta lectura coincide con el menú «Distribución de vacuna antiaftosa a los vacunadores», aunque debe validarse con los formularios `Vacu-distri`, `Sub_vacu_distri` y `Sub_vacu_vete`.

### Riesgos y mejoras necesarias

- No existe identificador único de entrega.
- No se conserva el lote o serie entregado al vacunador, por lo que se corta la trazabilidad iniciada en DISTRUBUCION.
- No se registran devolución, pérdida, rotura, ajuste ni responsable de la operación.
- `Marca` se repite como texto en lugar de referenciar un producto.
- La tabla no muestra documento o comprobante ni fecha de creación del registro.
- La unidad de Cantidad sigue sin estar definida.

En el nuevo sistema, la entrega debe ser otro tipo de movimiento de stock y sus detalles deben referenciar lotes concretos. El destinatario puede ser un vacunador, veterinaria u otra ubicación, modelado explícitamente y con historial.

## LABORATORIO

Fuente: captura de Vista Diseño de `LABORATORIO1`. Origen confirmado: `D:\DiscoDpc2\aftosa 31\datos.mdb`, tabla `LABORATORIO`.

| Campo | Tipo en Access | Observación |
|---|---|---|
| Nro | Texto corto | Clave primaria; tamaño 7, requerido, índice único y sin compresión Unicode. |
| NOMBRE | Texto corto | Nombre del laboratorio; tamaño pendiente. |
| LOCALIDAD | Texto corto | Localidad; no se observa una referencia formal a TABGEO en el diagrama. |
| TE | Texto corto | Teléfono o contacto telefónico. |

La tabla identifica laboratorios, pero el diagrama no muestra una relación con DISTRUBUCION, DISTRI-VETE, VACUNA o VACUNABT. `Marca` permanece como texto o lista embebida en formularios. Por lo tanto, la base anterior no garantiza mediante claves que cada marca pertenezca a un laboratorio válido.

Para el nuevo modelo se propone separar:

- laboratorio o fabricante;
- producto/vacuna y enfermedad objetivo;
- presentación;
- lote o serie y vencimiento.

Cada producto debería referenciar su laboratorio. Las marcas históricas deberán depurarse y relacionarse durante la migración, conservando el valor original para auditoría.
