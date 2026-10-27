# Dependencias entre módulos

Relaciones con otros módulos del ERP. Cada fila debe tener un contacto identificado y una fecha de acuerdo cuando exista.

| Módulo | Qué necesitamos de ellos / qué necesitan de nosotros | Contacto | Estado | Fecha acordada |
| :--- | :--- | :--- | :--- | :--- |
| Compras | Recibir operaciones de compra para generar asientos contables | TODO | Por conversar | TODO |
| Cuentas por Pagar | Recibir obligaciones y pagos para generar asientos contables | TODO | Por conversar | TODO |
| Control de Asistencia | Recibir planillas y movimientos de personal con impacto contable | TODO | Por conversar | TODO |
| Ventas Virtuales | Recibir operaciones de venta para generar asientos contables | TODO | Por conversar | TODO |
| Almacén | Recibir movimientos de inventario con impacto contable | TODO | Por conversar | TODO |
| Comercio Electrónico | Recibir operaciones de venta electrónica para generar asientos | TODO | Por conversar | TODO |
| Distribución | Recibir operaciones de distribución con impacto contable | TODO | Por conversar | TODO |

## Estados posibles

- **Por conversar**: todavía no hay contacto ni acuerdo.
- **En conversación**: ya hay contacto y se está definiendo el mecanismo.
- **Acordada**: existe acuerdo escrito y fecha.
- **Bloqueada**: la dependencia impide avanzar (registrar en `docs/registro-impedimentos.md`).

## Qué debe quedar acordado

1. Qué datos envía cada módulo y con qué estructura.
2. El mecanismo de integración (contrato de API REST, eventos, tablas compartidas).
3. Quién es el dueño de cada dato y quién lo consume.
4. Manejo de errores, reintentos e idempotencia.
5. Fecha de disponibilidad del contrato y del ambiente de pruebas.
