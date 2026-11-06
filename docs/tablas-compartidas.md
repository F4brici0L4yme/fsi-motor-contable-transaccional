# Tablas compartidas entre módulos

Tablas o entidades que el Motor Contable Transaccional usa junto con otros módulos del ERP. Las filas marcadas como **por confirmar** son candidatas obvias y deben acordarse con el equipo dueño antes de implementarse.

| Tabla / entidad | Módulo dueño | Módulos que la consumen | Estado | Observaciones |
| :--- | :--- | :--- | :--- | :--- |
| Plan de cuentas (PCGE) | Por confirmar | Motor Contable Transaccional | Por confirmar | Catálogo base para registrar asientos |
| Catálogo SUNAT: tipo de comprobante | Por confirmar | Motor Contable Transaccional | Por confirmar | Códigos oficiales SUNAT |
| Catálogo SUNAT: moneda | Por confirmar | Motor Contable Transaccional | Por confirmar | Códigos oficiales SUNAT |
| Catálogo SUNAT: tipo de documento de identidad | Por confirmar | Motor Contable Transaccional | Por confirmar | Códigos oficiales SUNAT |
| Catálogo SUNAT: unidad de medida | Por confirmar | Motor Contable Transaccional | Por confirmar | Códigos oficiales SUNAT |
| Terceros (proveedores / clientes) | Por confirmar | Motor Contable Transaccional | Por confirmar | Identificación del tercero en cada operación |
| Periodos contables | Por confirmar | Motor Contable Transaccional | Por confirmar | Apertura, cierre y bloqueo de periodos |

## Cómo completarla

- **Módulo dueño**: equipo que crea y mantiene la tabla.
- **Estado**: `Por confirmar`, `En conversación`, `Acordada`, `En duda`.
- Toda tabla compartida debe tener dueño único y un acuerdo escrito (issue o acta).
- Si la tabla no existe todavía, indicarlo en observaciones y registrar la dependencia en `docs/dependencias-entre-modulos.md`.
