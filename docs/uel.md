# Relevamiento de UEL

Fuente: captura de Vista Diseño de `UEL1`. Origen confirmado: `D:\DiscoDpc2\aftosa 31\datos.mdb`, tabla `UEL`.

| Campo | Tipo en Access | Interpretación inicial |
|---|---|---|
| ID_UEL | Número, Entero | Clave primaria de la unidad. Requerido e indexado sin duplicados. |
| NOMBRE | Texto corto | Nombre de la UEL. |
| Presidente | Texto corto | Persona que ocupa la presidencia. |
| Dpto | Número | Código de departamento; posible referencia conceptual a TABGEO. |
| Loca | Número | Código de localidad; posible referencia conceptual a TABGEO. |
| Administrativo | Texto corto | Persona o dato administrativo; significado exacto pendiente. |
| Domicilio | Texto corto | Domicilio. |
| Telefono | Texto corto | Teléfono. |
| Mail | Texto corto | Correo electrónico. |
| Perso-juri | Texto corto | Probable personería jurídica. |
| nro-afip | Texto corto | Número o identificación ante AFIP. |
| Equipa | Texto corto | Información de equipamiento; significado y formato pendientes. |
| Soft-Base | Texto corto | Software base utilizado. |
| Soft-Minis | Texto corto | Software ministerial u otro sistema; significado pendiente. |
| Oficina | Texto corto | Oficina o infraestructura. |

La propiedad Título de `ID_UEL` muestra `Id_Empre`, aunque el campo se llama ID_UEL. Parece un metadato copiado de otra tabla y no cambia la identidad real del campo.

## Relaciones confirmadas y pendientes

- UEL se relaciona uno a muchos con DISTRUBUCION mediante ID_UEL.
- DISTRI-VETE contiene Id_UEL y VACUNA/VACUNABT contienen ID_UEL; el modelo lógico indica que pertenecen a la misma unidad, aunque las reglas exactas de integridad deben verificarse.
- Dpto y Loca coinciden conceptualmente con la clave geográfica compuesta de TABGEO; ver [relevamiento geográfico](geografia.md). No se observó una relación formal inequívoca para UEL.

## Propuesta para el nuevo modelo

Mantener una entidad `unidades_ejecutoras` con una clave interna y el código histórico ID_UEL. Separar cuando corresponda:

- ubicación normalizada;
- domicilio y medios de contacto;
- autoridades y responsables con vigencia;
- datos jurídicos y fiscales;
- información de infraestructura o sistemas, si todavía se utiliza.

Los movimientos de stock, entregas y actas deberán referenciar formalmente a la UEL responsable. El nuevo sistema debería registrar también estado activo y fechas de vigencia.

## Pendiente

- Confirmar la expansión oficial de la sigla UEL y su función operativa.
- Tamaños, validaciones y valores reales de los campos de texto.
- Relación geográfica de Dpto + Loca con TABGEO.
- Significado actual de Administrativo, Equipa, Soft-Base, Soft-Minis y Oficina.
