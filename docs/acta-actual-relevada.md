# Relevamiento del acta actual

## Fuente

PDF escaneado de dos actas A4 completas aportado por el usuario. No se transcriben en este documento nombres, documentos, firmas, RENSPA ni otros datos personales de los ejemplos.

Las dos páginas muestran actas consecutivas, lo que permite confirmar el esquema de numeración y la estructura general.

## Identificación y numeración

- Título: Acta de Vacunación.
- Código de plan: 175.
- Entidad emisora: Fundación Control Fiebre Aftosa San Cristóbal - Este.
- Número correlativo impreso con ocho posiciones y ceros iniciales.
- Fecha de impresión en el pie.
- Rango de impresión indicado como «desde N° … hasta N° …».
- El acta se emite por triplicado, con el mismo número en los tres ejemplares:
  1. `ORIGINAL: FUNDACIÓN`.
  2. `DUPLICADO: SENASA`.
  3. `TRIPLICADO: PRODUCTOR`.

Los ejemplos confirman que la numeración se administra por lotes o talonarios impresos. El nuevo sistema debe mantener esta numeración simple y controlar formalmente cada rango.

## Secciones del formulario

### 1. Vacunación

- Sistemática o estratégica.
- Total o parcial.

### 2. Período

- Primero o segundo.
- Totales o menores.
- Año.

### 3. Propietario

- RENSPA.
- Nombre y apellido o razón social.
- Documento/CUIT.
- Domicilio.
- Código postal.
- Localidad.
- Provincia.
- Régimen de tenencia.
- Recuadro de 2,5 × 2,5 cm destinado a la marca del ganado.

### 4. Establecimiento

- Nombre.
- Ubicación.
- Cantidad de hectáreas.

### 5. Tipo de rodeo vacunado

- Cría.
- Invernada.
- Mixto.
- Cabaña.
- Tambo.
- Feed Lot.
- Otro.

### 6. Bovinos vacunados contra fiebre aftosa

- Vacas.
- Toros.
- Toritos.
- Novillos/Bueyes.
- Novillitos.
- Vaquillonas.
- Terneras.
- Terneros.
- Total.

### 7. Vacuna antiaftosa

- Marca.
- Serie.
- Vencimiento.

### 8. Brucelosis

Terneras vacunadas:

- Total.
- Parcial.
- Identificación.

Vacuna:

- Marca.
- Serie.
- Vencimiento.

### 9. Existencia de otras especies

| Grupo | Categorías impresas |
|---|---|
| Ovinos | Carneros, ovejas, borrego/a, capones, cordero/a y total. |
| Porcinos | Padrillos, cerdas, lechones, capones, cachorro/a y total. |
| Caprinos | Chivos, chivas, cabrito, capón, castrón y total. |
| Equinos | Padrillos, caballos, yeguas, potrillos, mula/burro y total. |
| Otras especies | Aves, caninos, ciervos, conejos, camélidos y colmenas, con columnas macho, hembra y total donde corresponde. |

### Declaración, firmas y cierre

- Declaración jurada del propietario o responsable, con referencia al artículo 293 del Código Penal.
- Lugar y fecha.
- Vacunador: firma, aclaración y DNI.
- Propietario/responsable: firma, aclaración y DNI.
- Observaciones.

### Vacunación contra carbunclo

- Total vacunados.
- Marca.
- Serie.
- Vencimiento.

## Qué puede preimprimir el nuevo sistema

- numeración y datos del lote de impresión;
- entidad, código de plan y tipo de ejemplar;
- campaña: tipo de vacunación, total/parcial, período, población y año;
- propietario, documento y domicilio;
- RENSPA y establecimiento;
- ubicación, hectáreas, régimen y tipo de rodeo;
- vacunador y matrícula, porque se conocen antes de emitir;
- vacuna, serie/lote y vencimiento, porque se conocen antes de emitir.

Si un dato preimpreso puede cambiar durante la visita, el papel debe ofrecer una forma clara de marcar «sin cambios» o consignar una corrección. La carga posterior conservará tanto lo impreso como lo verificado en campo.

## Qué debe quedar para completar en campo

- fecha y lugar efectivos;
- cantidades vacunadas por categoría;
- existencias de ovinos, porcinos, caprinos, equinos y otras especies;
- corrección de los datos de vacuna cuando excepcionalmente se use otro lote o más de uno;
- sección de brucelosis cuando corresponda;
- carbunclo cuando corresponda;
- observaciones;
- firmas, aclaraciones y DNI exigidos, completados manualmente en papel.

## Cambios derivados para el modelo

- Agregar `lotes_impresion_acta` con fecha, rango desde/hasta, cantidad de juegos, responsable y plantilla utilizada.
- Modelar cada número como un juego de tres ejemplares con destinos fijos: Fundación, SENASA y productor.
- Registrar la impresión y eventual reimpresión de cada ejemplar sin consumir un número nuevo.
- Mantener el número visible como texto de ocho caracteres para preservar ceros iniciales.
- Permitir múltiples programas sanitarios dentro de una misma acta física: aftosa, brucelosis y carbunclo.
- Permitir más de un lote de vacuna por programa, aunque el papel actual tenga una sola línea.
- Mantener una instantánea de los datos preimpresos y registrar por separado las correcciones verificadas en campo.
- Conservar el texto legal y las reglas de copias como configuración versionada de la plantilla.

## Pendiente

1. Quién asigna los rangos y si la secuencia es global o por entidad/código de plan.
2. Tratamiento oficial de tachaduras y correcciones.

## Forma de llenado de los ejemplares

Los tres ejemplares se apilan utilizando hojas de papel carbónico. El vacunador escribe una sola vez sobre el original y la información manuscrita se transfiere al duplicado y al triplicado. Por este motivo:

- los tres deben conservar exactamente las mismas posiciones y dimensiones;
- únicamente cambia la leyenda de destino del pie;
- el número de acta es idéntico en las tres hojas;
- no se deben desplazar campos entre ejemplares;
- la impresión debe respetar escala 100 %, tamaño A4 y desactivar opciones como «ajustar a página».
