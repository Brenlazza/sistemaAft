# Ficha del cliente/productor

## Objetivo

Reunir la identidad canónica utilizada para operación y facturación, sus establecimientos, vínculos SENASA por campaña y actividad sanitaria, sin mezclar datos personales con características del predio.

El [prototipo navegable](../prototipos/ficha-productor.html) muestra una ficha de ejemplo.

## Encabezado

- nombre o razón social;
- tipo de persona;
- documento enmascarado;
- estado activo/inactivo;
- origen del alta;
- alertas de duplicidad o vínculos pendientes.

## Pestaña Resumen

- establecimientos vigentes;
- campañas en las que aparece;
- animales previstos en la campaña seleccionada;
- animales vacunados en actas confirmadas;
- actas pendientes;
- remitos asociados como destino;
- estado de facturación cuando esa integración se implemente.

Los animales previstos proceden del padrón o existencia de campaña y son una estimación. Los vacunados proceden exclusivamente de actas confirmadas y vigentes. La diferencia puede variar por nacimientos, muertes o movimientos: se informa, pero no bloquea ni se clasifica automáticamente como error.

Cuando lo vacunado supera la estimación se muestra `Supera estimación`; cuando es menor se muestra `Saldo estimado`. Ninguno de los dos estados reemplaza el control sanitario ni impide nuevas actas parciales.

## Pestaña Establecimientos

Por cada relación:

- establecimiento y RENSPA;
- localidad y centro operativo;
- régimen;
- vigencia;
- campaña actual;
- población esperada;
- estado de cobertura.

Un establecimiento puede conservar relaciones históricas con diferentes productores. La ficha muestra la perspectiva de este cliente sin atribuirle propiedad permanente cuando el régimen indica otra condición.

## Pestaña Campañas y SENASA

- campaña e importación de origen;
- nombre del productor y establecimiento recibidos;
- cliente y establecimiento normalizados;
- estado y método del vínculo;
- diferencias detectadas;
- obligación total/parcial;
- estado de cumplimiento.

Los metadatos de usuario y confianza son privados y se muestran únicamente con permiso de auditoría.

## Pestaña Remitos y actas

Muestra cada remito donde el productor fue incluido como destino, incluso cuando comparte el retiro con otros productores. La cantidad retirada pertenece al remito completo y no se atribuye artificialmente a cada destino.

La aplicación por productor se obtiene de sus actas confirmadas:

- número de formulario o acta;
- fecha;
- establecimiento;
- vacunador;
- animales vacunados por categoría;
- dosis aplicadas por lote;
- estado y rectificaciones.

## Pestaña Datos privados y facturación

- documento completo;
- domicilio fiscal;
- condición impositiva;
- contactos con tipo, preferencia y vigencia;
- observaciones comerciales privadas;
- identificador del sistema de facturación, cuando exista.

Estos datos no se imprimen en el acta de vacunación salvo que el formato aprobado los exija. Sí pueden utilizarse en documentos comerciales autorizados.

## Pestaña Historial

- altas y modificaciones;
- alias y nombres anteriores;
- vínculos y desvinculaciones SENASA;
- unificaciones;
- cambios de documento o condición fiscal;
- usuario, fecha y motivo con acceso restringido.

## Reglas críticas

- una corrección no crea automáticamente otra persona;
- un cambio de productor no altera la identidad del establecimiento;
- una fila SENASA original nunca se modifica;
- una acta conserva el cliente, establecimiento y vínculo de campaña utilizados al confirmarla;
- un cliente inactivo conserva toda su historia y no aparece en nuevas selecciones;
- los contactos y datos fiscales se protegen por permiso;
- cualquier unificación requiere revisión y auditoría.
