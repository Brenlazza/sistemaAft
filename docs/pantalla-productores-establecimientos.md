# Padrón operativo de productores y establecimientos

## Objetivo

Consultar y conciliar los destinos operativos de una campaña, relacionando cada fila importada de SENASA con el cliente/productor canónico utilizado para facturación y con el establecimiento correspondiente.

El [prototipo navegable](../prototipos/productores-establecimientos.html) muestra la bandeja de trabajo y su proceso de vinculación.

Los archivos ingresan mediante el [asistente de importación SENASA](pantalla-importacion-padron-senasa.md); esta pantalla trabaja únicamente con importaciones ya confirmadas.

## Unidad mostrada

Cada fila representa un destino de campaña:

`cliente/productor + establecimiento + RENSPA + campaña`

No representa una identidad nueva. El mismo cliente/productor puede aparecer en varias filas cuando posee o explota varios establecimientos, y cada relación conserva su régimen y vigencia.

También pueden existir destinos creados durante la campaña que no provienen del padrón SENASA, no tienen RENSPA o corresponden a productores aún no inscriptos legalmente. Reciben un identificador interno y participan del mismo circuito operativo.

En el listado real de la primera campaña 2026, la fila de origen trae RENSPA y `TITULAR`, pero no nombre de establecimiento. La pantalla debe mostrar claramente `Sin nombre informado por SENASA` hasta encontrar un establecimiento canónico o completar un alta provisional. Ese texto es un estado visual, no un nombre que deba guardarse como dato oficial.

## Principios

- El cliente/productor es la identidad canónica utilizada para facturar.
- La fila SENASA conserva exactamente los valores recibidos en cada importación.
- El establecimiento es una entidad diferente del productor.
- El vínculo productor–establecimiento puede cambiar entre campañas.
- No se confirma automáticamente una coincidencia ambigua solamente por nombre.
- El RENSPA se almacena como texto, conservando formato y ceros.
- Los datos privados se enmascaran en listados generales.
- La hoja y el número de fila originales siempre permanecen visibles como referencia.
- Una ausencia del origen se conserva como ausencia; no se reemplaza por cero, texto inventado ni datos de otra campaña.

## Indicadores

- destinos importados para la campaña;
- vinculados;
- pendientes de vincular;
- observados;
- clientes nuevos creados durante la conciliación;
- establecimientos sin ubicación válida;
- destinos previstos todavía sin acta confirmada.

Para la importación real 1/2026 se muestran primero controles de origen independientes de la conciliación con el maestro:

- 3.497 UP recibidas y 3.497 RENSPA únicos;
- 3.440 filas de la hoja principal y 57 de convenios;
- 4 filas con alertas por cantidades negativas;
- 3.497 filas disponibles para consultar y utilizar.

Las cantidades de vínculos exactos, probables, pendientes o nuevos no se presentan hasta ejecutar la comparación contra la base vigente.

Estos totales se calculan sobre la campaña e importación seleccionadas, no sobre todo el historial.

## Filtros

- nombre original o canónico;
- documento;
- RENSPA;
- establecimiento;
- estado del vínculo;
- campaña e importación;
- centro o localidad operativa;
- obligación de vacunación;
- con o sin acta confirmada;
- solamente posibles duplicados.

## Columnas

| Columna | Contenido |
|---|---|
| Titular SENASA | `TITULAR` original, hoja y fila de origen. |
| Cliente/productor | Identidad canónica vinculada y documento enmascarado. |
| Establecimiento | Nombre canónico si existe, RENSPA y ubicación informada; nunca un nombre inferido. |
| Relación | Régimen y vigencia del productor en el predio. |
| Vínculo | Vinculado, pendiente u observado; método y confianza como ayuda. |
| Padrón de campaña | Estimación inicial SENASA y condición total/parcial. |
| Actividad | Remitos, formularios y actas confirmadas. |
| Cobertura | Pendiente, parcial, completa o excepción documentada. |
| Acciones | Abrir ficha, conciliar o revisar historial. |

Los valores importados y los normalizados se muestran juntos cuando difieren; el original nunca se sustituye.

## Conciliación de una fila

1. Mostrar la fila SENASA original sin editarla.
2. Buscar primero el establecimiento por RENSPA exacto.
3. Buscar el cliente/productor por CUIT exacto cuando esté informado.
4. Presentar similitudes de nombre o ubicación solo como ayuda.
5. Detectar contradicciones: mismo RENSPA en más de un establecimiento, CUIT asociado a identidades distintas o relación incompatible.
6. Permitir seleccionar un cliente y establecimiento existentes.
7. Si no existen, crear primero el cliente o un establecimiento provisional identificado por RENSPA.
8. Elegir régimen y vigencia del vínculo productor–establecimiento.
9. Confirmar la correspondencia guardando método, usuario y fecha de manera privada.

Una coincidencia sugerida no se convierte en vínculo definitivo hasta su confirmación, pero esto no impide utilizar al productor como destino operativo.

## Alta manual de cliente/productor

Para una carga urgente durante la preparación de un remito existe además el [alta rápida de productor](pantalla-alta-rapida-productor.md). Solicita solamente una referencia reconocible, asigna un identificador interno y permite completar después los datos maestros que todavía no se conocen.

Campos iniciales:

- tipo persona humana o jurídica;
- nombre o razón social;
- tipo y número de documento, si existe;
- domicilio de facturación;
- condición fiscal y datos comerciales necesarios;
- contactos privados;
- origen del alta y observación.

Antes de crear, se buscan coincidencias por documento normalizado, nombres y contactos. Una identidad con historia no se elimina: se inactiva o se unifica mediante una operación autorizada.

Crear un cliente no crea automáticamente un establecimiento ni resuelve la fila SENASA. Son pasos explícitos para evitar asociaciones incorrectas.

## Alta o vinculación del establecimiento

Campos iniciales:

- RENSPA original y normalizado;
- nombre del establecimiento, opcional mientras el origen no lo informe;
- provincia, departamento, distrito o localidad;
- domicilio o referencia rural cuando corresponda;
- estado y vigencia;
- fuente del dato.

Luego se registra el vínculo con el cliente/productor, incluyendo régimen —por ejemplo propietario, arrendatario, pastajero o capitalización— y fechas desde/hasta.

Un alta provisional sin nombre:

- conserva el RENSPA como identificador operativo;
- se muestra como `Sin nombre informado` en las vistas;
- exige fuente y observación;
- puede completarse después sin cambiar su identificador interno ni romper vínculos históricos;
- no permite que el usuario escriba una denominación supuesta solo para avanzar.

## Acciones masivas seguras

Después de comparar contra el maestro, el sistema puede agrupar coincidencias exactas. La confirmación masiva solo se habilita cuando:

- el RENSPA identifica un único establecimiento;
- el CUIT, si está informado, identifica un único cliente compatible;
- no hay contradicción entre el titular recibido y el vínculo vigente;
- la fila no tiene contradicciones que impidan una aprobación automática;
- el usuario posee permiso de aprobación masiva.

El sistema muestra previamente cantidad, criterio aplicado y casos que requieren revisión. Los casos probables, sin CUIT, con RENSPA nuevo o con conflicto se revisan individualmente, sin dejar de estar disponibles para la operación.

## Uso operativo sin bloqueos por padrón

Una fila pendiente, observada, sin RENSPA o con cantidades estimadas inconsistentes:

- permanece visible y seleccionable;
- puede incorporarse a un remito;
- puede generar y confirmar actas;
- participa de la conciliación de vacuna con las cantidades reales;
- puede facturarse mediante el cliente interno, aun cuando sus datos legales estén pendientes;
- conserva alertas para completar o revisar información posteriormente.

El sistema distingue `Estado del vínculo` de `Estado operativo`: el primero informa la calidad de la correspondencia; el segundo permanece habilitado. La única cantidad efectiva de vacunación es la registrada en el acta. Compararla con la estimación SENASA genera una variación informativa, no un bloqueo.

Si lo vacunado supera la estimación, la cobertura muestra `Supera estimación` y la diferencia positiva. No exige excepción ni corrección porque puede responder a nacimientos u otros cambios reales. Si resulta menor, se muestra el saldo estimado como referencia de seguimiento, sin afirmar automáticamente que faltan animales por vacunar.

## Duplicados y unificación

El sistema alerta ante:

- mismo documento en clientes diferentes;
- mismo RENSPA en establecimientos diferentes;
- misma fila importada vinculada más de una vez;
- nombres muy similares con documentos distintos o ausentes;
- un vínculo productor–establecimiento incompatible en fechas.

La unificación conserva identificadores anteriores, alias, documentos, vínculos y referencias históricas. Nunca se reasignan actas o facturas mediante una edición silenciosa.

## Privacidad y permisos

Documento, domicilio, teléfono, correo y datos fiscales son privados. El listado muestra valores enmascarados y los archivos exportados respetan el alcance del usuario.

Permisos separados para importar, consultar datos sensibles, crear clientes, crear establecimientos, vincular, observar, aprobar, unificar y exportar.

Los usuarios de subcentro solo consultan destinos dentro de su alcance operativo; no pueden modificar una importación ni ampliar su centro.

## Navegación

- Desde el padrón se abre la [ficha del productor](pantalla-ficha-productor.md).
- Desde un remito se selecciona cualquier destino operativo, vinculado o no con SENASA y con o sin RENSPA.
- Las altas creadas durante la operación se completan en la [bandeja de altas internas](pantalla-altas-internas-productores.md).
- Desde una acta se visualiza la identidad canónica y la fila SENASA histórica.
- Desde la ficha se recorren establecimientos, campañas, actas y datos comerciales.
