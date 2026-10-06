# Relevamiento de vacunadores y veterinarias

## VACUNADOR

Fuente: captura de Vista Diseño de `VACUNADOR1`. Origen confirmado: `D:\DiscoDpc2\aftosa 31\datos.mdb`, tabla `VACUNADOR`.

| Campo | Tipo en Access | Descripción original |
|---|---|---|
| Matricula | Texto corto | Matrícula del veterinario. Es la clave primaria. |
| Nombre | Texto corto | Nombre del veterinario. |
| Domicilio | Texto corto | Domicilio del veterinario. |
| Telefono | Texto corto | Teléfono. |
| Tipo | Texto corto | Veterinario o Idóneo. |

### Propiedades visibles de Matricula

| Propiedad | Valor observado |
|---|---|
| Tamaño del campo | 6 |
| Máscara de entrada | Patrón visible equivalente a dos dígitos, guion y cuatro dígitos (`00-0000`) |
| Título | Matricula |
| Regla de validación | Acepta patrones con prefijo 0, 1 o 2 seguido por guion y cuatro caracteres; conservar la expresión exacta pendiente de extracción. |
| Requerido | Sí |
| Permitir longitud cero | No |
| Indexado | Sí, sin duplicados |
| Compresión Unicode | Sí |

El campo `Tipo` confirma que el vacunador puede ser veterinario o idóneo. Todavía falta conocer los valores exactos almacenados y qué habilitación exige cada tipo.

### Consecuencias para el nuevo modelo

- Usar una clave interna para la persona y conservar Matricula como identificador profesional único, con su formato original.
- Separar datos personales, medios de contacto y habilitaciones cuando se conozcan las reglas actuales.
- Modelar el tipo o rol mediante un catálogo validado y con vigencia, porque una persona puede cambiar su condición o habilitación.
- Registrar estado activo, fecha de alta/baja y alcance territorial si el proceso lo requiere.
- No identificar automáticamente `VACUNADOR` con `Veterinarias`: el primero representa personas; la segunda tabla se usa desde VACUNA mediante `idVeterinario` y debe relevarse por separado.

## Pendiente

- Valores permitidos en `Tipo` y reglas de habilitación.
- Tamaños de Nombre, Domicilio, Telefono y Tipo.
- Datos históricos duplicados, matrículas vencidas o vacunadores sin matrícula.

## VETERINARIAS

Fuente: captura de Vista Diseño de la tabla local `Veterinarias`. A diferencia de las tablas con sufijo 1, su hoja de propiedades no muestra una ruta `DATABASE=...`, por lo que la evidencia indica que reside en la base de interfaz actual.

| Campo | Tipo en Access | Observación |
|---|---|---|
| IdVeterinario | Número, Entero largo | Clave primaria; requerido, índice único y valor predeterminado 0. |
| Veterinaria | Texto corto | Nombre de la veterinaria; tamaño pendiente. |

El selector del formulario Vacunacion muestra `Veterinaria` y guarda `IdVeterinario` en `VACUNA.idVeterinario`. A pesar del nombre singular del campo, la entidad representa una veterinaria o establecimiento, no la persona vacunadora. La persona se registra por separado mediante `Matricula` y VACUNADOR.

No se observa una relación formal de Veterinarias en el diagrama aportado. Tampoco hay domicilio, localidad, contacto, estado o responsable en esta tabla.

### Consecuencias para el nuevo modelo

- Renombrar conceptualmente la referencia como `veterinaria_id` para evitar confundirla con una persona.
- Definir si la veterinaria actúa como punto de recepción, proveedor, intermediario o lugar de stock.
- Incorporar identificación, contacto, ubicación y vigencia solo si el flujo operativo los necesita.
- Relacionarla formalmente con los movimientos o actas correspondientes.
- No usar 0 como identificador predeterminado; las referencias ausentes deben representarse de forma explícita y validada.
