# Pantalla de usuarios, alcances y permisos

## Objetivo

Administrar las cuentas que utilizarán el sistema sin confundir el rol de una persona con las acciones que realmente puede ejecutar. La configuración inicial mantiene exactamente dos administradores generales y permite crear usuarios limitados para coordinación, subcentros, facturación y cajas.

El acceso efectivo de un usuario resulta de cuatro elementos simultáneos:

1. estado activo de la cuenta;
2. rol o roles asignados como plantilla;
3. campañas y centros habilitados, con sus fechas de vigencia;
4. permisos concretos concedidos.

Si cualquiera de esos elementos no habilita la operación, el servidor la rechaza aunque el usuario conozca la dirección de la pantalla.

## Listado principal

La cabecera resume:

- usuarios activos;
- dos administradores generales;
- coordinadores y operadores de subcentro;
- usuarios autorizados a emitir facturas;
- usuarios autorizados a cobrar;
- cuentas suspendidas o con vigencia vencida.

La grilla permite buscar por nombre o usuario y filtrar por rol, centro, campaña y estado. Cada fila muestra:

- identidad de la cuenta;
- roles asignados;
- alcance territorial y de campaña;
- resumen de permisos operativos;
- autorización de facturación;
- autorización de caja;
- estado y vigencia;
- último acceso, visible solamente para administración.

Los indicadores comerciales deben ser inequívocos. Por ejemplo, una cajera puede mostrar `Cobra: sí` y `Emite facturas: no`.

## Alta y edición de usuario

### Identidad y estado

- nombre de referencia;
- usuario o correo de ingreso;
- estado: invitado, activo, suspendido o baja lógica;
- vigencia desde y hasta, cuando el acceso sea temporal;
- observación administrativa privada.

Suspender o dar de baja una cuenta impide nuevos accesos, pero nunca elimina operaciones, cierres, cobros, emisiones ni auditoría realizados anteriormente.

### Roles como plantillas

Se proponen estas plantillas iniciales:

| Rol | Finalidad |
|---|---|
| Administrador general | Acceso completo y administración de seguridad. Solo habrá dos cuentas iniciales. |
| Coordinador de campaña | Seguimiento operativo dentro de campañas y centros asignados. |
| Operador de subcentro | Stock, transferencias, remitos, actas y devoluciones de su centro. |
| Facturación | Preparación, revisión o emisión comercial según permisos concedidos. |
| Cajero/a | Apertura, cobro, cierre o rendición según cajas asignadas. |

Un usuario puede combinar roles. Los roles no reemplazan el alcance ni conceden automáticamente permisos críticos fuera de su función.

### Alcance

La asignación guarda:

- una o varias campañas;
- una o varias UEL;
- uno o varios centros de stock;
- fecha desde y hasta;
- condición activa o inactiva;
- usuario administrador que autorizó el cambio.

Un operador de subcentro solo consulta y utiliza el stock, las veterinarias, los remitos, las actas y las devoluciones vinculados con sus centros. La principal y los administradores conservan la vista consolidada.

La ausencia de un centro o campaña en el alcance equivale a acceso denegado. No existe una opción implícita de «todos» salvo para el administrador general.

## Matriz de permisos

La edición agrupa acciones independientes:

- **Operación:** campañas, productores, remitos, actas, transferencias y stock;
- **Importaciones y datos privados:** importar, conciliar, ver contactos o auditoría restringida;
- **Facturación:** consultar, preparar, revisar, emitir, registrar comprobante externo, ajustar y anular;
- **Cajas:** consultar caja propia, abrir, cobrar, revertir, cerrar, rendir, supervisar y consultar global;
- **Administración:** usuarios, roles, parámetros, catálogos y auditoría.

Las reglas comerciales obligatorias son:

- `caja.cobrar` no concede `facturacion.emitir`;
- `facturacion.emitir` no concede `caja.abrir`, `caja.cobrar` ni `caja.cerrar`;
- asignar una caja no concede permiso para operarla;
- conceder un permiso de caja no permite utilizar cajas no asignadas;
- preparar una liquidación no permite emitir una factura salvo concesión expresa;
- consultar no implica exportar, modificar, anular ni ajustar.

La pantalla presenta un **resumen de acceso efectivo** antes de guardar para detectar combinaciones involuntarias.

## Asignación de cajas

Para cada usuario autorizado se seleccionan una o varias cajas compatibles con sus centros. La relación conserva:

- caja;
- centro;
- vigencia;
- posibilidad de abrir;
- posibilidad de cobrar;
- posibilidad de cerrar;
- posibilidad de rendir o supervisar.

Las sesiones siguen siendo individuales. Dos usuarios autorizados sobre la misma caja lógica no comparten una sesión abierta ni mezclan sus movimientos.

## Controles críticos

- No se puede desactivar la última cuenta administradora disponible.
- La configuración inicial debe conservar las dos cuentas administradoras definidas.
- Un usuario no puede concederse a sí mismo permisos superiores ni ampliar su propio alcance.
- Los cambios críticos —administración de usuarios, emisión/anulación de facturas, ajustes de stock, supervisión de cajas y consulta global— requieren motivo.
- La necesidad de una segunda aprobación para cambios críticos queda configurable y pendiente de decisión operativa.
- Toda autorización se valida nuevamente al ejecutar la acción; ocultar botones es solo una ayuda visual.

## Auditoría

Cada cambio registra de manera privada:

- usuario afectado;
- valor anterior y nuevo;
- rol, permiso, campaña, centro o caja modificado;
- administrador que realizó el cambio;
- fecha y hora;
- motivo cuando sea obligatorio;
- aprobación adicional, si correspondiera.

El historial no se imprime ni se muestra en pantallas operativas comunes. Las cuentas eliminadas lógicamente permanecen referenciables desde las operaciones históricas.

## Casos representativos

### Operador de Ceres

- rol: operador de subcentro;
- centro: Ceres;
- campaña: 2/2026;
- puede aceptar transferencias, emitir remitos y cargar actas locales;
- no puede ver stock detallado de otros centros;
- no puede facturar ni cobrar.

### Cajera de la principal

- rol: cajero/a;
- centro: San Cristóbal;
- caja: caja principal 01;
- puede abrir su sesión, cobrar facturas ya emitidas y cerrar;
- no puede emitir ni modificar facturas;
- no puede operar remitos o stock por el solo hecho de cobrar.

### Responsable de facturación

- rol: facturación;
- puede preparar, revisar y emitir según concesiones;
- no puede cobrar ni abrir una caja salvo asignación independiente;
- puede tener alcance global comercial sin adquirir permisos sanitarios o de stock.

## Decisiones pendientes

1. Qué coordinadores podrán confirmar actas y cuáles solamente preparar borradores.
2. Si toda concesión crítica requiere una segunda aprobación o solo algunas categorías.
3. Duración máxima de los accesos temporales.
4. Método de autenticación y recuperación de cuenta.
5. Política de bloqueo por intentos fallidos y duración de sesión.
