# Flujo operativo propuesto — campaña antiaftosa

## Alcance inicial

La primera versión se plantea para campañas de vacunación antiaftosa. El modelo conserva capacidad para incorporar brucelosis en terneras y otros controles sanitarios sin mezclar sus reglas con las de aftosa.

Este flujo es un borrador para validar con usuarios del sistema. No reemplaza todavía el procedimiento oficial.

## 1. Preparar la campaña

Un usuario autorizado crea la campaña e informa:

- nombre y año;
- tipo total o parcial;
- fechas previstas;
- UEL participantes;
- categorías animales alcanzadas;
- reglas de cumplimiento;
- períodos administrativos para informes, si se utilizan.

La campaña comienza en estado `Borrador`. Pasa a `Preparada` cuando tiene configuración, padrón inicial y responsables. Solo una campaña `Activa` admite nuevas actas y movimientos operativos.

## 2. Importar y conciliar el padrón

Cada archivo o extracción de SENASA se registra como una importación independiente. El sistema conserva las filas originales y ejecuta una conciliación contra el maestro canónico de clientes/productores y sus establecimientos.

Cada fila puede quedar:

- vinculada automáticamente;
- pendiente de revisión;
- vinculada manualmente;
- descartada con motivo;
- identificada como alta o diferencia.

Al aprobar la conciliación se genera el padrón de campaña. Esta instantánea define los establecimientos esperados y permite calcular pendientes sin que los cambios posteriores alteren el informe histórico.

Todo registro SENASA utilizado en el flujo debe quedar vinculado directamente con un cliente/productor canónico, que es la identidad utilizada también para facturar. Si no existe, se crea el cliente y luego se completa el vínculo. Ver [relación entre clientes/productores y padrón SENASA](clientes-productores-senasa.md).

## 3. Recibir vacunas

La UEL registra la recepción indicando:

- veterinaria propietaria;
- laboratorio y marca de la vacuna;
- lote o serie;
- vencimiento;
- cantidad de frascos cerrados de 125 cc;
- proveedor u origen;
- fecha y comprobante;
- responsable de la recepción.

Al confirmar el movimiento, los frascos quedan físicamente en la UEL y se suman al stock lleno de la veterinaria que los compró. Los borradores no modifican existencias.

## 4. Entregar vacunas mediante remito

La veterinaria propietaria presenta una autorización manual firmada para que un vacunador o veterinario retire vacuna de su stock almacenado en la UEL. El sistema genera el remito de retiro, reemplazando o integrando el circuito que actualmente se realiza en Xubio.

Cada línea identifica la veterinaria propietaria, lote y cantidad de frascos llenos. El remito se imprime y lo firma el profesional receptor, quien asume responsabilidad por lo retirado.

El encabezado del remito permite seleccionar uno o más destinos. Cada destino vincula el cliente/productor canónico con su registro SENASA, establecimiento y RENSPA. Un mismo retiro puede utilizarse en varias vacunaciones: por ejemplo, 125 dosis pueden distribuirse en actas de 50, 50 y 25 animales de tres productores diferentes.

Si luego del retiro debe incorporarse otro productor, se agrega al mismo remito. El sistema genera una versión nueva del documento completo, lo identifica como reimpresión modificada y exige que el profesional vuelva a firmarlo. El número de remito no cambia y las versiones anteriores quedan conservadas para auditoría.

El sistema valida:

- vacunador habilitado;
- lote vigente y en condición disponible;
- stock lleno suficiente perteneciente a esa veterinaria;
- autorización de la veterinaria;
- campaña activa;
- al menos un destino con su vínculo cliente/productor–SENASA resuelto.

La confirmación descuenta frascos llenos del stock de la veterinaria en la UEL y asigna su custodia al profesional receptor. Los sobrantes de frascos utilizados regresan a la UEL como un stock separado, manteniendo veterinaria propietaria, lote, productor, establecimiento y acta de origen. Los decomisos, roturas y ajustes se registran como movimientos separados.

Antes de que el profesional se retire, el sistema genera un preborrador de acta por cada destino confirmado. Se imprimen el remito y los juegos triplicados; el profesional firma el remito y se lleva la vacuna y las actas para completarlas en campo. Ver [preborradores de actas desde el remito](preborradores-actas-desde-remito.md).

## 5. Registrar el acta

El flujo inicial será híbrido: a partir de cada destino del remito, el sistema emite un preborrador como juego de tres ejemplares con el mismo número —original para la Fundación, duplicado para SENASA y triplicado para el productor—. Los datos del establecimiento, del vacunador y de la vacuna asignada salen preimpresos. Las dosis previstas del remito no se copian como animales vacunados. El vacunador completa en papel los datos reales y luego un operador los transcribe. Ver [diseño de actas numeradas](actas-papel-digital.md), [preborradores desde el remito](preborradores-actas-desde-remito.md) y [lógica funcional de actas](logica-actas.md).

El acta identifica:

- campaña;
- establecimiento/RENSPA;
- fecha de vacunación;
- número de acta;
- UEL;
- vacunador;
- veterinaria, cuando corresponda;
- resultado y observaciones.

El detalle registra las categorías y cantidades vacunadas. El consumo de vacuna identifica los lotes y dosis utilizados.

La carga puede comenzar como `Borrador`. Al confirmar:

- se validan categorías y cantidades;
- se valida el número de acta dentro del ámbito que se defina;
- se consumen las dosis del stock del vacunador;
- se actualiza el cumplimiento del padrón de campaña;
- se registra usuario y fecha de confirmación.

Un acta confirmada no se elimina. Puede corregirse mediante un procedimiento auditado o anularse con motivo, usuario y fecha, revirtiendo los movimientos asociados cuando corresponda.

## 6. Controlar cobertura y diferencias

Durante la campaña se muestran:

- establecimientos vacunados, pendientes y exceptuados;
- animales esperados y vacunados por categoría;
- cobertura por UEL, departamento, distrito, vacunador y período;
- diferencias entre padrón, existencias declaradas y actas;
- actas duplicadas, incompletas o pendientes de revisión;
- lotes próximos a vencer;
- frascos llenos y sobrantes por veterinaria en la UEL;
- vacunas en poder de vacunadores/veterinarios;
- remitos pendientes de firma o confirmación.

Los casos sin vacunación necesitan un motivo explícito. No se deducen únicamente porque la cantidad sea cero.

## 7. Conciliar stock

Para cada veterinaria, vacunador y lote:

`frascos llenos = recibidos - retirados - rotos - decomisados ± ajustes`

Los sobrantes de frascos abiertos se concilian por separado según la cantidad remanente, el productor/establecimiento y el acta que originó la devolución.

Para cada remito se suman todas las actas confirmadas y vigentes de sus destinos y se calcula:

`devolución esperada = dosis retiradas llenas y sobrantes - suma de dosis vacunadas`

`uso inferido = dosis retiradas llenas y sobrantes - dosis devueltas reales`

Ambos controles deben coincidir con sus valores reales. Los descuadres positivos o negativos quedan observados y pueden cerrarse con motivo y autorización, conservando todas las cantidades declaradas. Ver [flujo de remitos y conciliación](flujo-remitos-conciliacion.md).

Las roturas y los decomisos se utilizan exclusivamente para descontar el stock físico de la UEL. Se registran como movimientos separados y no forman parte de la conciliación del remito entregado a un profesional.

La conciliación señala diferencias y exige motivo para los ajustes. El cierre del vacunador puede ser requisito para cerrar la campaña.

El comportamiento detallado de movimientos, saldos, consumos por acta y reversiones se encuentra en [lógica funcional de stock](logica-stock.md).

## 8. Cerrar la campaña

Antes del cierre se verifica:

- actas pendientes o con errores;
- establecimientos del padrón sin resolución;
- stock sin conciliar;
- movimientos en borrador;
- importaciones pendientes de revisión.

Una campaña `Cerrada` queda disponible para consulta e informes. Su reapertura requiere permiso especial y queda auditada.

## Estados propuestos

| Entidad | Estados iniciales |
|---|---|
| Campaña | Borrador, Preparada, Activa, Cerrada, Anulada |
| Importación | Cargada, En conciliación, Conciliada, Rechazada |
| Formulario físico | Reservado, Impreso, Entregado, Devuelto, Cargado, Anulado, Extraviado |
| Acta digital | Borrador, Observada, Confirmada, Rectificada, Anulada |
| Movimiento de stock | Borrador, Confirmado, Anulado |
| Lote | Disponible, Bloqueado, Vencido, Agotado |
| Vínculo de padrón | Pendiente, Vinculado, Exceptuado, Descartado |

## Roles iniciales

| Rol | Responsabilidades propuestas |
|---|---|
| Administrador general | Dos usuarios iniciales con acceso completo a configuración, usuarios, campañas, padrón, stock, actas, rectificaciones, informes y auditoría privada. |
| Coordinador de campaña | Acceso limitado únicamente a las campañas y funciones operativas que se le asignen expresamente. No administra usuarios, permisos, parámetros globales ni auditoría privada. |

El sistema aplicará el principio de menor privilegio: toda función no concedida al coordinador queda denegada. Los permisos concretos del coordinador se definirán por acción —consultar, crear, modificar, confirmar, anular o informar— y podrán limitarse a una campaña o UEL determinada.

## Puntos que requieren decisión operativa

1. Quién crea y quién confirma un acta.
2. Si el vacunador cargará datos directamente o entregará actas para carga administrativa.
3. Permisos operativos exactos que tendrá el coordinador de campaña.
4. Tratamiento de revacunaciones y correcciones posteriores.
5. Categorías obligatorias en campañas totales y parciales.
6. Autoridad que aprueba excepciones de categorías bovinas o bubalinas.
7. Unidad real de recepción, entrega y aplicación de vacunas.
8. Momento en que se considera cumplido un establecimiento.
9. Motivos oficiales de excepción o no vacunación.
10. Necesidad de operar sin conexión y sincronizar posteriormente.

## Temas para resolver a futuro

- ámbito de la numeración del acta: global, campaña, UEL, vacunador o talonario;
- continuidad o reinicio de la secuencia entre campañas;
- autoridad que administra y entrega los rangos de números.

La definición de numeración queda postergada y no condiciona el resto del flujo funcional.
