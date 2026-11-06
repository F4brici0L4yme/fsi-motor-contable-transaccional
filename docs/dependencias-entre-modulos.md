# Dependencias entre módulos — Motor Contable Transaccional

Relaciones de integración técnica del **Módulo II: Operaciones Contables y Motor de Automatización (Core)** con los demás módulos del ERP institucional.

---

## 1. Matriz de Dependencias

| Módulo Externo | Qué necesitamos de ellos / Qué les entregamos | Mecanismo de Integración | Estado | Contacto / Dueño |
| :--- | :--- | :--- | :--- | :--- |
| **Módulo I: Estructura y Planificación** | Consumimos el Catálogo Maestro de Cuentas Universales (PCGE) y dimensiones/centros de costo | API REST / Tabla compartida (`pcge_cuentas`) | En conversación | Módulo I |
| **Módulo III: Tesorería y Gestión Fiscal** | Consumimos el registro diario de tipos de cambio oficiales (PEN/USD) y porcentajes de impuestos (IGV 18%) | API REST / Tabla compartida (`tipos_cambio`) | En conversación | Módulo III |
| **Módulo III: Tesorería y Reportabilidad** | Entregamos la base de asientos contables aprobados (Libro Diario / Mayor C6) para balances y EEFF | Lectura de asientos contables | En conversación | Módulo III |
| **Compras** | Recibimos facturas y notas de compra para generar asientos con destino a almacén | `POST /v1/asientos` (`COMPRA_FACTURA`) | Por conversar | Módulo Compras |
| **Cuentas por Pagar** | Recibimos liquidación de obligaciones y egresos para asientos de cancelación | `POST /v1/asientos` (`PAGO_PROVEEDOR`) | Por conversar | Módulo Ctas por Pagar |
| **Ventas Virtuales** | Recibimos facturas/boletas de venta para generar asientos de ingresos e IGV | `POST /v1/asientos` (`VENTA_FACTURA`) | Por conversar | Módulo Ventas |
| **Comercio Electrónico** | Recibimos transacciones de e-commerce y pasarelas de pago | `POST /v1/asientos` (`VENTA_ECOMMERCE`) | Por conversar | Módulo E-Commerce |
| **Almacén** | Recibimos movimientos de salida de inventario para registrar costo de ventas (Cuentas 69 vs 20) | `POST /v1/asientos` (`SALIDA_INVENTARIO`) | Por conversar | Módulo Almacén |
| **Distribución** | Recibimos costos de fletes y logística de distribución para imputación contable | `POST /v1/asientos` (`GASTO_DISTRIBUCION`) | Por conversar | Módulo Distribución |
| **Control de Asistencia / RRHH** | Recibimos provisiones de nómina mensual, gratificaciones, CTS y aportes ESSALUD | `POST /v1/asientos` (`DEVENGADO_PLANILLA`) | Por conversar | Módulo RRHH |

---

## 2. Protocolo de Integración

1. **Canal Oficial:** Todas las transacciones entrantes deben dirigirse al endpoint `POST /v1/asientos` documentado en el [README.md](../README.md).
2. **Idempotencia Obligatoria:** Cada emisor debe enviar el identificador único de la operación en su sistema (`referencia_origen_id`) para evitar duplicaciones por reintentos de red.
3. **Manejo de Errores:** Respuestas HTTP 400 (error de validación de campos) y 422 (descuadre de partida doble o falta de regla contable) deben ser gestionadas por el cliente emisor.
4. **Disponibilidad y Mocking:** El equipo del Motor Contable Transaccional proveerá un ambiente mock y una colección de Postman/curl para que los demás equipos prueben sus integraciones.
