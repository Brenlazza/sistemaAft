# Relevamiento sanitario

## EPIDEMIA

Fuente: captura de Vista Diseño de la tabla vinculada `EPIDEMIA`. Origen confirmado: `D:\DiscoDpc2\aftosa 31\datos.mdb`, tabla `EPIDEMIA`.

| Campo | Tipo en Access | Clave | Interpretación inicial |
|---|---|---|---|
| Ficha | Texto corto | Sí | Identificador del establecimiento/RENSPA. Tamaño 20, requerido y con título Renspa. |
| Fecha | Fecha/Hora | Sí | Fecha del control o toma de muestras. |
| Muestras | Número | No | Cantidad de muestras; la descripción original es incongruente. |
| Protocolo | Número | No | Número de protocolo; la descripción original fue copiada de geografía y no es confiable. |
| Laboratorio | Texto corto | No | Laboratorio interviniente; no referencia formalmente LABORATORIO.Nro en la evidencia disponible. |
| Resultado | Texto corto | No | La descripción enumera `+`, `-` y `S`; falta conocer el significado de S. |
| Matricula | Texto corto | No | Profesional asociado; probable referencia conceptual a VACUNADOR.Matricula. |
| Cantidadp | Número | No | Probable cantidad de positivos, pendiente confirmación. |

La clave primaria compuesta es `(Ficha, Fecha)`. Esta regla impide guardar dos controles del mismo establecimiento con el mismo valor de fecha/hora, incluso si tienen protocolos diferentes.

Las descripciones de Muestras, Protocolo y Laboratorio no coinciden con sus nombres y parecen copiadas de ESTABLECIMIENTOS/TABGEO. Deben ignorarse como evidencia funcional.

## Interpretación provisional

La tabla almacena un evento de muestreo o diagnóstico sanitario por establecimiento. No contiene un campo de enfermedad, por lo que el proceso podría estar implícito en el formulario o en el uso histórico de la tabla. No se puede afirmar todavía si corresponde a brucelosis, tuberculosis, garrapata u otro control.

## Propuesta para el nuevo modelo

- Crear eventos sanitarios con clave interna y referencia al establecimiento.
- Registrar enfermedad o programa sanitario explícitamente.
- Separar toma de muestras, protocolo de laboratorio y resultados cuando exista más de una muestra o prueba.
- Referenciar formalmente laboratorio y profesional.
- Registrar cantidad muestreada, resultados positivos/negativos/sospechosos y unidad.
- Conservar documentos, fechas, estado y correcciones con historial.
- Evitar usar `(establecimiento, fecha)` como única identidad del evento.

## Pendiente

- El formulario `Laboratorio` no puede abrirse actualmente debido a un error de la base Access informado durante el relevamiento. Queda pendiente recuperar sus etiquetas, origen de registro y validaciones desde una copia funcional o mediante sus dependencias/código.
- Significado de Resultado `S` y de Cantidadp.
- Enfermedad o programa al que pertenece EPIDEMIA.
- Relación con Estado Sanitario B, Estado Sanitario T, Fgarrapata y Presencia.
