# Tablas compartidas entre módulos — Tribu Financiera

Mapeo de entidades compartidas que interactúan con el **Módulo II: Motor Contable Transaccional**.

| Tabla / Entidad | Módulo Dueño | Módulos que la Consumen | Estado | Observaciones |
| :--- | :--- | :--- | :--- | :--- |
| **Plan de Cuentas (PCGE)** | Módulo I: Estructura Contable | Módulo II (Motor Contable), Módulo III (Reportes) | En conversación | Catálogo oficial de cuentas según PCGE 2019 modificado |
| **Centros de Costo / Dimensiones** | Módulo I: Estructura Contable | Módulo II (Motor Contable), Compras, Ventas | En conversación | Segmentación analítica para imputación de gastos e ingresos |
| **Asientos Contables (Libro Diario / Mayor C6)** | **Módulo II: Motor Contable** | Módulo III (EEFF y Tesorería), Auditoría | **Acordada (Dueño)** | Núcleo de operaciones de partida doble y trazabilidad oficial |
| **Plantillas y Reglas Contables** | **Módulo II: Motor Contable** | Módulo II (Exclusivo) | **Acordada (Dueño)** | Reglas maestras para traducción de eventos a asientos |
| **Periodos Contables** | Módulo III: Cierres Contables | Módulo II (Motor Contable), Módulo I | En conversación | Control de apertura, cierre mensual y bloqueo tributario |
| **Catálogo de Monedas y Tipos de Cambio** | Módulo III: Tesorería | Módulo II (Motor Contable), Facturación, Compras | En conversación | Cotizaciones diarias oficiales SBS/SUNAT para bimoneda |
| **Catálogos SUNAT (Comprobantes, Doc Identidad)** | Tribu Financiera (Común) | Todos los módulos del ERP | Acordada | Tablas maestras estandarizadas (Catálogos 01, 02, 06) |
| **Terceros (Clientes / Proveedores)** | Módulos Ventas / Compras | Módulo II (Motor Contable) | En conversación | Identificación fiscal obligatoria en asientos contables |

---

## Directrices de Gobierno
- **Dueño Único:** Ninguna tabla posee múltiples escritores concurrentes de datos maestros.
- **Inmutabilidad de Asientos:** El Módulo II es la única entidad facultada para crear asientos oficiales; Módulo III los consume en modo de solo lectura para la consolidación de estados financieros (Balance General y P&G).
