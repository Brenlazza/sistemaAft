# Relevamiento del sistema de vacunación

## Objetivo

Crear un sistema para campañas de vacunación antiaftosa reutilizando los datos y las funcionalidades útiles del sistema existente, e incorporando mejoras. Esta primera etapa documenta el modelo antes de implementar la aplicación.

Fuente: dos capturas de Microsoft Access aportadas por el usuario. No se dispone todavía de la base original, su diseño de tablas ni sus relaciones. Los nombres visibles están confirmados; las equivalencias y los campos propuestos requieren validación.

## Tablas visibles en el sistema anterior

| Nombre visible | Información disponible |
|---|---|
| semanas | Columnas visibles: año, mes, semana, fechad, fecgah. Las dos últimas muestran fechas que parecen delimitar un período; confirmar su significado. |
| senasa | Diseño y finalidad por revisar. El menú incluye consulta del padrón de SENASA. |
| TABGEO1 | Diseño y finalidad por revisar. |
| Veterinarias | Diseño y finalidad por revisar. |
| DISTRI-VETE | Diseño y finalidad por revisar; no asumir su relación con Veterinarias. |
| DISTRUBUCION | Nombre transcrito tal como aparece. Diseño por revisar. |
| EMPRESA | Diseño y finalidad por revisar. |
| ESTABLECIMIENTOS | Diseño por revisar. |
| LABORATORIO | Diseño por revisar. |
| TABGEO | Diseño y relación con TABGEO1 por revisar. |
| UEL | Diseño y significado exacto de la sigla por confirmar. |
| VACUNA | Diseño por revisar. |
| VACUNABT | Diseño y finalidad por revisar. |
| VACUNADOR | Diseño por revisar. |

Varias tablas muestran el icono de tabla vinculada en Access. Se deberá identificar su origen antes de planificar la extracción; las capturas no permiten determinar dónde están almacenados sus datos.

## Funciones visibles para conservar y revisar

- Gestión de vacunadores.
- Recepción y stock de vacuna antiaftosa.
- Distribución de vacuna a vacunadores.
- Ingreso de actas de vacunación de aftosa y brucelosis.
- Informes de aftosa y brucelosis.
- Gestión de laboratorios.
- Consulta alfabética por propietario y consulta ordenada por total de rodeo.
- Consulta de establecimientos que no vacunaron.
- Búsqueda de diferencias; falta identificar qué magnitudes compara.
- Informe de situación de un productor.
- Consulta del padrón de SENASA y nómina de UEL.
- Exportación a Excel; formato y destinatario por confirmar.

La interfaz anterior incluye brucelosis. Su inclusión en el nuevo sistema queda pendiente de decisión; por ahora el alcance solicitado es antiaftosa.

## Propuesta inicial de tablas principales

Los campos siguientes son candidatos, no un esquema definitivo ni una transcripción del diseño de Access.

| Tabla propuesta | Finalidad | Datos y relaciones a relevar |
|---|---|---|
| campañas | Separar cada campaña y su seguimiento histórico. | Identificador, nombre, año, período, fechas, estado y alcance sanitario. |
| productores | Registrar personas o entidades responsables del ganado. | Identificador, nombre o razón social, identificación fiscal cuando corresponda, contacto. Confirmar si propietario, productor y empresa representan entidades diferentes. |
| establecimientos | Registrar los lugares donde se vacuna. | Identificador, nombre, ubicación, identificadores oficiales y estado. Confirmar cómo se representa RENSPA y si puede haber varios productores por establecimiento. |
| productor_establecimiento | Representar vínculos cuando existan varios titulares o cambios de responsable. | Productor, establecimiento, rol y vigencia. Crear solo si el relevamiento confirma esta necesidad. |
| vacunadores | Identificar quién realiza la vacunación. | Identificador, nombre, contacto, habilitación y estado, según los datos existentes. |
| unidades_ejecutoras | Registrar las UEL si intervienen en la operación. | Identificador, denominación, jurisdicción y responsables; confirmar significado y funciones. |
| laboratorios | Identificar fabricantes o proveedores según el uso real. | Identificador, nombre y datos existentes. |
| vacunas | Catalogar productos utilizados. | Identificador, denominación, enfermedad objetivo y laboratorio. |
| lotes_vacuna | Permitir trazabilidad de las dosis. | Vacuna, número de lote, vencimiento y presentación. Confirmar unidad de stock y conversión entre frascos y dosis. |
| movimientos_stock | Registrar recepción, entrega, devolución, pérdida y ajustes. | Fecha, tipo, lote, cantidad, unidad, origen, destino, comprobante y motivo. Definir los lugares o responsables que mantienen stock. |
| actas_vacunacion | Registrar cada intervención. | Número, campaña, fecha, establecimiento, productor, vacunador, estado y observaciones. Confirmar cómo se asigna y qué hace único al número de acta. |
| detalle_acta | Registrar cantidades por categoría animal. | Acta, categoría, cantidad y, si corresponde, especie. Confirmar categorías oficiales y cómo se registra el total de rodeo. |
| vacunas_aplicadas | Vincular las dosis aplicadas con sus lotes. | Acta, lote y dosis. Permite varios lotes por acta si la operación lo requiere. |
| categorias_animales | Mantener las categorías empleadas en actas e informes. | Código, descripción, especie y vigencia según reglas reales. |
| padron_campaña | Conservar los establecimientos y productores esperados al inicio de cada campaña. | Campaña, referencias al padrón y situación inicial. Necesario para definir correctamente quiénes no vacunaron y evitar que cambios posteriores alteren informes históricos. |
| ubicaciones | Normalizar localidades y otras divisiones si TABGEO lo justifica. | Códigos y jerarquías existentes. |

## Relaciones y reglas que debemos confirmar

- Cómo se relacionan productor, establecimiento e identificadores de SENASA.
- Qué animales debe incluir cada campaña y cómo se registran existencias y vacunados.
- Si se admiten actas parciales, varias visitas o revacunaciones y cómo se calcula el cumplimiento.
- Cómo se corrigen o anulan actas sin perder el historial.
- Si una entrega de vacunas se asigna a vacunador, veterinaria, UEL u otro responsable.
- Cómo se concilian dosis entregadas, aplicadas, devueltas y perdidas, sin descontar dos veces la misma aplicación.
- Qué contiene VACUNABT y qué diferencias busca la consulta del sistema anterior.
- Si semanas define períodos de informes: no sustituirlos automáticamente por semanas ISO, porque las filas visibles se dividen por mes.
- Qué campos y formatos exigen los informes y exportaciones actuales.

## Mejoras candidatas para evaluar

- Trazabilidad desde recepción de un lote hasta su aplicación.
- Historial de campañas sin sobrescribir información anterior.
- Controles de duplicados y validación de cantidades y fechas.
- Búsqueda por productor, establecimiento, identificador oficial y acta.
- Registro de correcciones y responsables de los cambios.
- Informes reproducibles de cobertura y conciliación de stock.

Estas mejoras son propuestas; se priorizarán con el usuario después de comprender el flujo actual.

## Próximo material a relevar

1. Vista Diseño de ESTABLECIMIENTOS: campos, tipos, clave primaria e índices.
2. Vista Diseño de EMPRESA y senasa para comprender productores y padrón.
3. Vista Diseño de VACUNA, VACUNABT y DISTRUBUCION y el formulario de ingreso de actas.
4. Ventana Relaciones de Access y origen de las tablas vinculadas.
5. Un acta y un informe de ejemplo con datos personales ocultos.

Registrar para cada tabla: nombre original, finalidad, campos, tipos, claves, relaciones, validaciones, volumen aproximado y tratamiento de datos históricos. No cargar datos personales reales ni credenciales en el repositorio durante este relevamiento.
