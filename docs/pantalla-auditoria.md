# Pantalla de auditoría e historial crítico

## Objetivo

Permitir que los dos administradores generales y los usuarios expresamente autorizados reconstruyan una operación sensible: quién la realizó, cuándo, desde qué alcance, cuál fue el valor anterior, cuál es el nuevo y por qué se hizo.

La auditoría es privada. No forma parte de actas, remitos, facturas, recibos ni planillas impresas y no debe aparecer en las vistas operativas comunes.

## Principios

- los eventos confirmados son inmutables;
- no se permite editar ni eliminar una entrada de auditoría;
- una corrección genera un nuevo evento vinculado con el anterior;
- la fecha y hora provienen del servidor;
- se conserva el usuario y el alcance efectivo que tenía al ejecutar la acción;
- desactivar una cuenta no rompe la referencia histórica;
- consultar auditoría y exportarla son permisos distintos;
- los datos técnicos se muestran solamente cuando resultan útiles para investigar.

## Bandeja principal

La pantalla resume:

- eventos críticos del día;
- cambios de permisos;
- ajustes o diferencias de stock;
- rectificaciones y anulaciones de actas;
- emisiones, ajustes o anulaciones comerciales;
- reversiones y diferencias de caja;
- accesos rechazados o acciones denegadas.

Los filtros permiten combinar:

- fecha desde y hasta;
- usuario afectado o ejecutor;
- módulo;
- tipo de acción;
- campaña;
- centro;
- nivel: informativo, relevante o crítico;
- identificador del documento: acta, remito, transferencia, factura, recibo o caja.

El listado muestra una descripción entendible y evita exponer contraseñas, secretos, documentos personales completos u otros valores innecesarios.

## Detalle de un evento

Al abrir una fila se presenta:

- identificador único del evento;
- fecha y hora exactas;
- usuario ejecutor;
- rol, campaña y centro efectivos en ese momento;
- módulo, acción y resultado;
- entidad y registro afectado;
- motivo informado;
- valor anterior y valor nuevo por campo;
- documento u operación relacionada;
- aprobación adicional, si existió;
- referencia al evento corregido o revertido;
- datos técnicos restringidos, como sesión y origen, sin mostrarlos en exportaciones comunes.

Para cantidades sensibles —dosis, animales, importes y permisos— los valores anterior y nuevo deben quedar explícitos, no solamente una leyenda genérica de «modificado».

## Eventos que deben auditarse

### Seguridad

- alta, invitación, activación, suspensión o baja lógica de usuarios;
- asignación o retiro de roles;
- cambio de campañas, centros o cajas asignadas;
- concesión o denegación de permisos;
- intentos rechazados sobre funciones o alcances no autorizados.

### Actas y formularios

- preimpresión, reimpresión, entrega, devolución, extravío o anulación;
- confirmación, rectificación o anulación de un acta;
- autorización excepcional frente a demora del vacunador;
- modificación del profesional, productor, establecimiento o cantidades.

### Stock, transferencias y remitos

- recepción y asignación de vacuna;
- salida, devolución y conciliación por remito;
- aceptación u observación de una transferencia;
- diferencias físicas;
- rotura o decomiso;
- ajustes autorizados y sus motivos.

### Facturación y cajas

- aprobación o cambio de tarifas de campaña;
- emisión, anulación o ajuste de facturas;
- registro o reversión de cobros;
- apertura, cierre y rendición de caja;
- diferencias declaradas y su resolución.

## Exportación

La exportación requiere `auditoria.exportar`, además de `auditoria.consultar`. Debe:

- respetar los filtros aplicados;
- identificar quién generó el archivo y cuándo;
- registrar la propia exportación como evento;
- omitir datos técnicos o personales no necesarios;
- permitir un informe resumido y otro detallado según permiso.

No existe una descarga masiva sin trazabilidad.

## Retención y rendimiento

Los eventos se conservan aunque termine la campaña. La aplicación puede archivar técnicamente períodos antiguos, pero la consulta debe mantener continuidad y no alterar el contenido original.

Para búsquedas rápidas se indexan fecha, usuario, módulo, acción, centro, campaña, entidad e identificador del registro. Los valores extensos anterior/nuevo pueden almacenarse como detalle asociado sin recargar la bandeja principal.

## Permisos

| Permiso | Alcance |
|---|---|
| `auditoria.consultar` | Consulta los eventos permitidos por el alcance del usuario. |
| `auditoria.consultar_global` | Consulta todos los centros y campañas. Reservado inicialmente a administración general. |
| `auditoria.ver_datos_tecnicos` | Accede a sesión y origen técnico restringido. |
| `auditoria.exportar` | Genera una exportación auditable con los filtros actuales. |

Ningún permiso permite modificar o borrar eventos.

## Decisiones pendientes

1. Tiempo de conservación en línea antes del archivo técnico.
2. Qué perfiles, además de los dos administradores, podrán consultar auditoría limitada.
3. Qué campos técnicos se conservarán de acuerdo con la política de privacidad.
4. Formato de exportación requerido para controles externos.
