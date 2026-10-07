# Usuarios, roles y permisos

## Objetivo

Controlar qué información y acciones puede utilizar cada persona. La primera versión tendrá dos usuarios administradores con acceso completo y otros usuarios que actuarán como coordinadores de campaña con alcance limitado.

## Principios

- acceso denegado por defecto;
- cada usuario recibe únicamente las funciones necesarias;
- los permisos del coordinador se limitan a campañas y UEL asignadas;
- toda operación sensible registra usuario y fecha;
- los metadatos privados de auditoría no se imprimen ni aparecen en pantallas operativas;
- desactivar un usuario impide nuevos accesos sin eliminar su historial.

## Administrador general

La configuración inicial contempla exactamente dos cuentas administradoras. Ambas pueden:

- administrar usuarios, roles y permisos;
- configurar campañas, UEL, catálogos y parámetros;
- importar y conciliar padrones;
- gestionar recepciones, transferencias, devoluciones y ajustes de stock;
- emitir, cargar, confirmar, rectificar y anular actas;
- autorizar excepciones de campaña;
- cerrar o reabrir campañas;
- consultar informes globales y auditoría privada.

Las acciones críticas de un administrador quedan auditadas. Tener acceso completo no permite borrar el historial.

## Coordinador de campaña

El coordinador solo puede actuar sobre campañas y UEL que tenga asignadas y dentro de las fechas habilitadas. No obtiene permisos globales por el solo hecho de poseer el rol.

Permisos mínimos propuestos:

- ingresar al sistema;
- consultar las campañas asignadas;
- consultar establecimientos del padrón asignado;
- buscar formularios y actas de su alcance;
- iniciar o modificar cargas en borrador cuando se le habilite esa acción;
- consultar informes operativos limitados a su campaña/UEL;
- informar observaciones o incidencias.

Restricciones iniciales:

- no administra usuarios ni permisos;
- no modifica parámetros o catálogos globales;
- no consulta auditoría privada;
- no accede a campañas o UEL no asignadas;
- no realiza ajustes de stock, anulaciones, rectificaciones, cierres o reaperturas salvo permiso adicional explícito;
- no exporta información global ni datos sensibles fuera de su alcance.

## Alcance de asignación

La relación entre usuario y campaña debe guardar:

- usuario;
- campaña;
- una o varias UEL, cuando corresponda;
- fecha desde y hasta;
- permisos adicionales concedidos;
- administrador que autorizó la asignación;
- estado activo/inactivo.

La aplicación verifica el alcance tanto en la interfaz como en el servidor. Ocultar un botón no reemplaza la validación del permiso al ejecutar la operación.

## Acciones configurables

Los permisos se representan por acciones independientes:

| Módulo | Acciones posibles |
|---|---|
| Campañas | Consultar, preparar, activar, cerrar, reabrir. |
| Padrón | Consultar, importar, conciliar, exceptuar. |
| Formularios | Consultar, emitir, entregar, recibir, reimprimir, anular, declarar extravío. |
| Actas | Consultar, crear borrador, modificar, observar, confirmar, rectificar, anular. |
| Stock | Consultar, recibir, transferir, devolver, ajustar, conciliar. |
| Remitos | Consultar, crear borrador, autorizar, confirmar entrega, imprimir, agregar destino posterior, registrar nueva firma, devolver, revisar, conciliar y anular. |
| Informes | Consultar, exportar. |
| Administración | Usuarios, roles, catálogos, parámetros y auditoría. |

Esto permite ampliar un coordinador concreto sin convertirlo en administrador general.

## Datos privados

Se consideran privados, como mínimo:

- usuario que realizó cada operación;
- fecha y hora detallada de cambios;
- valores anteriores y posteriores de una rectificación;
- registros de inicio de sesión y accesos;
- motivos internos de investigación o auditoría;
- identificadores técnicos y datos de seguridad.

Estos datos se conservan para futuras consultas autorizadas, pero no se muestran en el acta impresa, el ejemplar del productor, el ejemplar de SENASA ni las vistas comunes de coordinación.

## Decisiones pendientes

1. Qué acciones concretas tendrá inicialmente cada coordinador.
2. Si los coordinadores pueden confirmar actas o solo preparar borradores.
3. Si pueden emitir formularios y registrar entrega/devolución.
4. Qué informes pueden consultar y cuáles pueden exportar.
5. Si alguna acción crítica requerirá aprobación de uno de los dos administradores.
