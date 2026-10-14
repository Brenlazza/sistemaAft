# Importación y conciliación de clientes de facturación

## Fuente relevada

Se analizó el archivo real `Clientes.xlsx`, utilizado actualmente para facturación. Contiene una hoja `Clientes`, 2.085 filas de datos y 22 columnas.

La fuente debe conservarse como archivo original inmutable. Los ejemplos visuales del sistema deben utilizar datos ficticios o enmascarados.

## Estructura observada

| Grupo | Columnas de origen |
|---|---|
| Identidad | `Nombre`, `Tipo de identificación`, `Número de identificación` |
| Clasificación fiscal | `Condición IVA`, `Cliente activo`, `Es proveedor` |
| Código externo | `Código` |
| Ubicación | `País`, `Provincia`, `Localidad`, `Domicilio` |
| Contactos | `Mail`, `CC`, `CCO`, `Teléfono` |
| Facturación | `Cuenta contable`, `Percepciones`, `Lista de precios` |
| Bancarios y organización | `CBU`, `Tiene sucursales` |
| Otros | `avisofactvenchabilitado`, `Observaciones` |

`Observaciones` contiene texto libre de distintos tipos. Se conserva íntegro y privado, pero no se interpreta automáticamente como fecha de nacimiento, instrucción fiscal ni vínculo con otra persona.

## Perfil observado del archivo

- 2.085 clientes;
- 2.085 marcados activos;
- 2.046 con número de identificación y 39 sin número;
- 2.081 identificados como CUIT, 3 como DNI y 1 como CUIL;
- 1.916 con `Código`, de los cuales 1.907 tienen apariencia de RENSPA;
- 169 sin código;
- no se observaron números de identificación ni códigos duplicados dentro del archivo;
- se detectaron 2 nombres normalizados repetidos, que abarcan 4 filas y requieren conservar identidades separadas salvo evidencia adicional;
- localidad, domicilio, correo y teléfono presentan una cobertura parcial y nunca deben ser obligatorios para importar.

Distribución fiscal observada:

| Condición IVA | Cantidad |
|---|---:|
| Consumidor final | 1.420 |
| Responsable inscripto | 443 |
| Monotributista | 221 |
| Exento | 1 |

La importación conserva los valores originales y utiliza catálogos normalizados para búsqueda e informes.

## Separación de conceptos

El archivo representa clientes comerciales. El listado SENASA representa productores y establecimientos previstos para la campaña. No existe una correspondencia uno a uno:

- un cliente puede tener varias UP o establecimientos;
- un establecimiento puede cambiar de productor o régimen entre campañas;
- puede existir un cliente sin actividad de vacunación en la campaña;
- puede vacunarse un productor todavía no incluido como cliente comercial;
- un productor interno puede carecer temporalmente de CUIT, RENSPA o inscripción formal.

Por eso se mantienen entidades separadas:

`cliente/productor canónico ↔ vínculo vigente ↔ establecimiento/UP`

La facturación utiliza la identidad comercial. El acta utiliza el productor y establecimiento que representan la vacunación real. La relación permite navegar entre ambos sin mezclar ni sobrescribir las fuentes.

## Comparación con el padrón SENASA 1/2026

La comparación normalizada de los dos archivos produce, sobre 3.497 UP válidas:

| Resultado por fila SENASA | Cantidad | Tratamiento |
|---|---:|---|
| CUIT y RENSPA coinciden con el mismo cliente | 1.482 | Propuesta de vínculo de alta confianza; requiere confirmación del proceso de importación. |
| CUIT y RENSPA apuntan a clientes distintos | 5 | Alerta de contradicción para revisión humana; nadie queda bloqueado. |
| Solo coincide el CUIT | 1.388 | Vincular el cliente como candidato y revisar/agregar el establecimiento correspondiente. |
| Solo coincide el RENSPA | 6 | Revisar la identidad del productor antes de confirmar el vínculo. |
| Solo coincide exactamente el nombre | 9 | Mostrar sugerencia débil; nunca confirmar automáticamente. |
| Sin coincidencia exacta | 607 | Mantener o crear identidad operativa independiente y completar después. |

En el sentido inverso, 1.756 clientes encuentran alguna coincidencia exacta por CUIT, RENSPA o nombre con la campaña y 329 no encuentran ninguna. Los 329 siguen siendo clientes válidos: pueden corresponder a otra campaña, otra actividad o no tener UP en el padrón actual.

## Asistente de importación

### 1. Cargar y conservar archivo

- seleccionar campaña o marcar la importación como actualización general del maestro;
- guardar archivo, nombre, tamaño, hash, fecha y usuario;
- detectar hoja, encabezados, cantidad de filas y fórmulas;
- presentar una vista previa con datos protegidos.

### 2. Mapear columnas

El perfil inicial propone:

| Origen | Destino |
|---|---|
| Nombre | Nombre o razón social original |
| Tipo de identificación | Tipo de documento |
| Número de identificación | Documento original y normalizado |
| Condición IVA | Condición fiscal |
| Código | Código externo y posible RENSPA, pendiente de validación |
| País, Provincia, Localidad, Domicilio | Domicilio comercial estructurado |
| Mail, CC, CCO, Teléfono | Contactos privados separados |
| Cuenta contable, Percepciones, Lista de precios | Configuración comercial |
| CBU | Dato bancario restringido |
| Observaciones | Nota original privada, sin interpretación automática |
| Cliente activo | Vigencia comercial |

`Código` no se transforma directamente en RENSPA. Primero se conserva como código externo; solo se crea o confirma el vínculo con un establecimiento cuando pasa la validación de formato y la comparación con el padrón.

### 3. Validar sin excluir

Generan alertas:

- documento vacío o con formato inusual;
- nombre repetido;
- código vacío, no compatible con RENSPA o ausente del padrón actual;
- CUIT y código que proponen identidades incompatibles;
- campos comerciales desconocidos para el catálogo;
- notas libres que requieren revisión.

Las alertas no excluyen filas. Las 2.085 identidades pueden importarse y utilizarse; la calidad pendiente se atiende después.

### 4. Comparar con el maestro

Orden de coincidencia:

1. documento normalizado único;
2. código/RENSPA validado contra un establecimiento único;
3. nombre normalizado junto con ubicación o contacto, solo como sugerencia;
4. revisión manual cuando las señales se contradicen.

El asistente muestra previamente cuántos registros creará, actualizará, mantendrá sin cambios o dejará con una tarea de revisión.

### 5. Confirmar

La confirmación se realiza en una transacción y conserva:

- valores originales;
- valores normalizados;
- versión del perfil de mapeo;
- resultado de cada comparación;
- usuario, fecha y archivo de origen;
- cambios sobre datos fiscales o de contacto;
- vínculos SENASA propuestos y confirmados.

No se reemplaza un dato sensible vigente sin guardar su valor anterior y la fuente del cambio.

## Uso posterior

Después de importar:

- el selector de clientes para facturación consulta el maestro comercial;
- el selector de destinos del remito consulta productores y establecimientos operativos;
- si ambos están vinculados, la factura puede heredar el cliente correspondiente;
- si faltan datos fiscales, se crea una tarea administrativa sin bloquear la vacunación;
- las altas internas aparecen en la [bandeja de revisión](pantalla-altas-internas-productores.md);
- los conflictos entre fuentes permanecen visibles hasta que un usuario autorizado los resuelva.

## Permisos y privacidad

Documento, domicilio, correo, teléfono, CBU, condición fiscal y observaciones son privados. Las vistas generales muestran datos enmascarados y los usuarios de subcentro solo acceden a la información necesaria dentro de su alcance.

Importar, cambiar datos fiscales, consultar CBU, confirmar vínculos y unificar identidades son permisos separados.
