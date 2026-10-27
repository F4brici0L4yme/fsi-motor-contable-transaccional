# Diccionario de datos

Plantilla para documentar las tablas y campos del módulo. La completan **Arquitectura** y **Backend** a medida que avanza el diseño.

| Tabla | Campo | Tipo | Nulo | Clave | Descripción | Origen / catálogo SUNAT |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| TODO | TODO | TODO | Sí / No | PK / FK / — | TODO | TODO |

## Convenciones

- **Tabla**: nombre en `snake_case` y plural. Ejemplo: `asientos_contables`.
- **Campo**: nombre en `snake_case`.
- **Tipo**: tipo de PostgreSQL (`uuid`, `varchar(20)`, `numeric(18,2)`, `date`, `timestamp`, etc.).
- **Nulo**: `Sí` o `No`.
- **Clave**: `PK` (primaria), `FK` (foránea), `UK` (única) o `—`.
- **Origen / catálogo SUNAT**: indicar el catálogo oficial cuando el campo dependa de SUNAT (tipo de comprobante, moneda, tipo de documento de identidad, unidad de medida) o del PCGE.
- Toda tabla que se comparta con otro módulo debe figurar además en `docs/tablas-compartidas.md`.
- Toda relación con otro módulo debe estar acordada y registrada en `docs/dependencias-entre-modulos.md`.

## Orden sugerido de avance

1. Periodos contables.
2. Plan de cuentas (PCGE).
3. Catálogos SUNAT.
4. Terceros.
5. Asientos contables y sus líneas (detalle en partida doble).
6. Libros y reportes derivados.
