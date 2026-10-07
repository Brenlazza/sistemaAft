# Relevamiento del sistema de vacunación

## Objetivo

Crear un sistema para campañas de vacunación antiaftosa reutilizando los datos y las funcionalidades útiles del sistema existente, e incorporando mejoras. Esta primera etapa documenta el modelo antes de implementar la aplicación.

Fuente: capturas de Microsoft Access aportadas por el usuario. No se dispone todavía de la base original, su diseño de tablas ni sus relaciones. Los nombres visibles están confirmados; las equivalencias y los campos propuestos requieren validación. El detalle de las capturas adicionales está en [relevamiento de establecimientos y tablas vinculadas](establecimientos.md).

## Tablas visibles en el sistema anterior

| Nombre visible | Información disponible |
|---|---|
| semanas | Calendario local para informes con año, mes, semana y fechas desde/hasta; no representa por sí solo una campaña. Ver [campañas y calendario](campanias-calendario.md). |
| senasa | Tabla local simplificada con RENSPA, productor y establecimiento, sin clave ni fecha de importación. Ver [padrón SENASA](padron-senasa.md). |
| TABGEO1 | Diseño y finalidad por revisar. |
| Veterinarias | Tabla local con IdVeterinario y nombre de la veterinaria; VACUNA guarda esa referencia. Ver [vacunadores y veterinarias](vacunadores-veterinarias.md). |
| DISTRI-VETE | Entregas por marca y UEL a un vacunador identificado por matrícula; no tiene clave primaria ni lote. Ver [stock y distribución](stock-vacunas.md). |
| DISTRUBUCION | Recepciones o entregas por marca, UEL y fecha, con vencimiento, serie y cantidad; ver [stock y distribución](stock-vacunas.md). |
| EMPRESA | ID_EMPRE numérico y NOMBRE; relación uno a muchos hacia ESTABLECIMIENTOS. Ver [establecimientos](establecimientos.md); significado operativo pendiente. |
| EPIDEMIA | Controles por establecimiento y fecha con muestras, protocolo, laboratorio y resultado. Ver [relevamiento sanitario](sanidad.md). |
| ESTABLECIMIENTOS | Columnas de propietario, establecimiento, existencias y sanidad observadas mediante el vínculo ESTABLECIMIENTOS1; [detalle](establecimientos.md). Diseño por revisar. |
| LABORATORIO | Catálogo con Nro, nombre, localidad y teléfono; no se observa relación formal con Marca. Ver [stock y distribución](stock-vacunas.md). |
| TABGEO | Catálogo geográfico con clave compuesta de departamento y distrito/localidad. Ver [geografía](geografia.md). |
| UEL | Unidad responsable de stock y actas, con autoridades, ubicación y datos institucionales. Ver [relevamiento de UEL](uel.md); significado exacto de la sigla pendiente. |
| VACUNA | Registros por establecimiento y fecha de vacunación, vinculados con vacunador. Tipos documentados en [VACUNA y VACUNABT](vacunaciones-access.md); significado exacto por revisar. |
| VACUNABT | Estructura semejante a VACUNA, con campos adicionales distintos. Tipos documentados; enfermedad y finalidad exactas por revisar. |
| VACUNADOR | Personas vacunadoras, de tipo veterinario o idóneo, identificadas por matrícula. Ver [vacunadores y veterinarias](vacunadores-veterinarias.md). |

El administrador de tablas vinculadas confirma dos archivos Access de origen y las correspondencias del grupo expandido, documentadas en el [detalle](establecimientos.md). También aparece la tabla EPIDEMIA, no observada en las primeras capturas. Falta confirmar el archivo operativo vigente antes de planificar la extracción.

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
| clientes_productores | Registrar la identidad canónica de personas o entidades responsables del ganado y utilizadas también para facturación. | Identificador, nombre o razón social, identificación fiscal, contacto, datos comerciales y origen por importación o alta manual. |
| establecimientos | Registrar los lugares donde se vacuna. | Identificador, nombre, ubicación, identificadores oficiales y estado. Confirmar cómo se representa RENSPA y si puede haber varios productores por establecimiento. |
| productor_establecimiento | Representar vínculos cuando existan varios titulares o cambios de responsable. | Productor, establecimiento, rol y vigencia. Crear solo si el relevamiento confirma esta necesidad. |
| vacunadores | Identificar quién realiza la vacunación. | Identificador, nombre, contacto, habilitación y estado, según los datos existentes. |
| unidades_ejecutoras | Registrar las UEL si intervienen en la operación. | Identificador, denominación, jurisdicción y responsables; confirmar significado y funciones. |
| laboratorios | Identificar fabricantes o proveedores según el uso real. | Identificador, nombre y datos existentes. |
| vacunas | Catalogar productos utilizados. | Identificador, denominación, enfermedad objetivo y laboratorio. |
| lotes_vacuna | Permitir trazabilidad de las dosis. | Vacuna, número de lote, vencimiento y presentación. Confirmar unidad de stock y conversión entre frascos y dosis. |
| movimientos_stock | Registrar recepción, entrega, devolución, consumo, rotura/decomiso en UEL y ajustes autorizados. | Fecha, tipo, lote, cantidad, unidad, origen, destino, comprobante y motivo. Definir los lugares o responsables que mantienen stock. |
| actas_vacunacion | Registrar cada intervención. | Número, campaña, fecha, establecimiento, productor, vacunador, estado y observaciones. Confirmar cómo se asigna y qué hace único al número de acta. |
| detalle_acta | Registrar cantidades por categoría animal. | Acta, categoría, cantidad y, si corresponde, especie. Confirmar categorías oficiales y cómo se registra el total de rodeo. |
| vacunas_aplicadas | Vincular las dosis aplicadas con sus lotes. | Acta, lote y dosis. Permite varios lotes por acta si la operación lo requiere. |
| categorias_animales | Mantener las categorías empleadas en actas e informes. | Código, descripción, especie y vigencia según reglas reales. |
| padron_campaña | Conservar los establecimientos y productores esperados al inicio de cada campaña. | Campaña, referencias al padrón y situación inicial. Necesario para definir correctamente quiénes no vacunaron y evitar que cambios posteriores alteren informes históricos. |
| vinculos_padron_cliente | Relacionar obligatoriamente cada registro SENASA operativo con el cliente/productor canónico. | Importación, fila SENASA, cliente/productor, establecimiento, método y estado de conciliación. |
| ubicaciones | Normalizar localidades y otras divisiones si TABGEO lo justifica. | Códigos y jerarquías existentes. |

## Relaciones y reglas que debemos confirmar

La captura posterior de Relaciones aporta claves visibles, incluida Ficha en ESTABLECIMIENTOS y claves compuestas en VACUNA, VACUNABT y TABGEO. Ver [detalle de relaciones](relaciones-access.md). Los campos visibles de VACUNA y VACUNABT sugieren registros de vacunación; su equivalencia con las tablas propuestas aún no está definida.

- Cada registro SENASA utilizado debe vincularse directamente con el cliente/productor canónico y su establecimiento; resta precisar las reglas automáticas de coincidencia para cada formato de importación.
- Qué animales debe incluir cada campaña y cómo se registran existencias y vacunados.
- Las actas parciales y varias visitas están admitidas; cada visita lleva acta, fecha, lote y consumo propios. Queda por definir el tratamiento específico de revacunaciones.
- El historial de correcciones y anulaciones se conservará mediante rectificaciones y movimientos inversos; queda por validar el procedimiento oficial de autorización.
- Si una entrega de vacunas se asigna a vacunador, veterinaria, UEL u otro responsable.
- Cómo se concilian dosis entregadas, aplicadas y devueltas, conservando por separado cualquier diferencia sin justificar y sin descontar dos veces la misma aplicación.
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
