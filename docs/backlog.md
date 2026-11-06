# Backlog del Producto — Módulo II: Operaciones Contables y Motor de Automatización (Core)

**Responsable:** Nagin Jazmin, Carfer Carpio (Product Owner)  
**Tribu:** Financiera  
**Módulo:** Motor Contable Transaccional (Módulo II)  
**Estado:** Definido y Cargado en Issues de GitHub  

---

## 1. Visión y Alcance del Módulo II

El **Módulo II: Operaciones Contables y Motor de Automatización (Core)** tiene como propósito gobernar el procesamiento centralizado de las transacciones financieras del ERP, convirtiendo eventos de negocio procedentes de otros módulos (Ventas, Compras, Cuentas por Pagar, Almacén, Distribución, Planillas) en asientos contables balanceados bajo el principio de partida doble ($\sum \text{Debe} = \sum \text{Haber}$), garantizando trazabilidad, inmutabilidad y cumplimiento tributario con SUNAT y el Plan Contable General Empresarial (PCGE).

### Componentes Clave:
1. **3.1. CRUD Maestro: Plantillas de Contabilización (Motor Contable)**: Definición dinámica de reglas parametrizables sin alterar código fuente.
2. **3.2. Submódulo: Gestión de Asientos Diarios (Libro Mayor C6)**: Recepción vía API REST (`POST /v1/asientos`), ciclo de vida del asiento (Borrador, Aprobado, Extornado) y validación estricta de partida doble.
3. **3.3. Submódulo: Transacciones Recurrentes**: Automatización de devengos mensuales y depreciaciones mediante jobs programados en segundo plano.
4. **Control y Gobernanza del Plan de Cuentas**: Matriz de permisos que restringe qué cuentas puede usar cada módulo (ej. Ventas no puede usar cualquier cuenta), segmentación por monedas y entidades.
5. **Mayorización y "Sacar Cuentas" (Balance de Comprobación y EEFF)**: Agregación de débitos y créditos para obtener saldos netos en tiempo real, emitiendo Balance General y Estado de Ganancias y Pérdidas (P&G).
6. **Flujo de Caja y Soporte a Decisiones Financieras**: Monitoreo de ingresos y egresos de efectivo (Cuentas 10, 12, 42, 45), control de vencimientos y alertas para toma de decisiones de financiamiento/préstamos o pagos.

---

## 2. Matriz de Trazabilidad de Requerimientos

| Código | Requerimiento / Regla de Negocio | Historia de Usuario | Issue GitHub | Prioridad |
| :---: | :--- | :--- | :---: | :---: |
| **3.1.1** | Reglas parametrizables para automatización de eventos de negocio | [HU-02](#hu-02-reglas-parametrizables-para-automatización-de-eventos-de-negocio) | [#13](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/13) | **Alta** |
| **3.1.2** | Especificación de operaciones CRUD para reglas contables y plantillas | [HU-01](#hu-01-crud-maestro-de-plantillas-de-contabilización) | [#12](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/12) | **Alta** |
| **3.1.3** | Ejemplos de reglas de contabilización para eventos de negocio | [HU-02](#hu-02-reglas-parametrizables-para-automatización-de-eventos-de-negocio) | [#13](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/13) | **Alta** |
| **3.2.1** | Ciclo de vida y estados del asiento C6 (Borrador, Aprobado, Extornado) | [HU-05](#hu-05-ciclo-de-vida-y-gestión-de-estados-del-asiento-contable-c6) | [#16](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/16) | **Alta** |
| **3.2.2** | Lógica de validación de partida doble ($\sum \text{Debe} = \sum \text{Haber}$) | [HU-04](#hu-04-validación-estricta-de-partida-doble-y-consistencia-contable) | [#15](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/15) | **Alta** |
| **3.2.3** | Especificación de API de recepción (`POST /v1/asientos`) | [HU-03](#hu-03-api-de-recepción-de-eventos-y-transacciones-post-v1asientos) | [#14](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/14) | **Alta** |
| **3.2.4** | Formato C6 del asiento procesado y visualización | [HU-05](#hu-05-ciclo-de-vida-y-gestión-de-estados-del-asiento-contable-c6) / [HU-06](#hu-06-consulta-filtros-avanzados-y-trazabilidad-del-libro-diario-y-mayor-c6) | [#16](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/16), [#17](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/17) | **Alta / Media** |
| **3.3.1** | Automatización mediante jobs programados para asientos periódicos | [HU-07](#hu-07-automatización-de-asientos-periódicos-mediante-jobs-programados) | [#18](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/18) | **Media** |
| **3.3.2** | Frecuencia y lista de operaciones recurrentes (alquileres, depreciaciones) | [HU-08](#hu-08-configuración-y-catálogo-de-operaciones-recurrentes) | [#19](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/19) | **Media** |
| **RN-01** | Restricción de uso de cuentas PCGE por módulo emisor y segmentación | [HU-09](#hu-09-restricción-y-control-de-cuentas-contables-por-módulo-emisor) | [#21](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/21) | **Alta** |
| **RN-02** | Mayorización y cálculo de saldos en tiempo real ("Sacar cuentas" / Sumas y Saldos) | [HU-10](#hu-10-mayorización-y-cálculo-automático-de-saldos-sacar-cuentas) | [#22](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/22) | **Alta** |
| **RN-03** | Consolidación y emisión de Estados Financieros (Balance General y P&G) | [HU-11](#hu-11-consolidación-y-generación-de-estados-financieros) | [#23](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/23) | **Alta** |
| **RN-04** | Seguimiento de Flujo de Caja y proyección de liquidez (Cobros, Pagos y Préstamos) | [HU-12](#hu-12-seguimiento-de-flujo-de-caja-y-proyección-de-liquidez) | [#24](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/24) | **Alta** |

---

## 3. Detalle de Historias de Usuario

### HU-01: CRUD maestro de plantillas de contabilización
- **Issue:** [#12](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/12)
- **Prioridad:** Alta | **Área:** Backend / Frontend
- **Historia:**
  > **Como** Contador General / Administrador del Sistema,  
  > **quiero** gestionar (crear, listar, consultar detalle, actualizar y desactivar) plantillas maestras de contabilización en el motor contable,  
  > **para** definir de forma parametrizable cómo se deben estructurar los asientos contables según cada tipo de operación sin depender de modificaciones directas en el código fuente.
- **Criterios de Aceptación:**
  - [ ] Endpoint REST en el backend (`/api/v1/plantillas-contabilizacion/`) que soporte operaciones de listado con paginación, búsqueda por código/nombre, filtro por módulo origen y estado (activo/inactivo).
  - [ ] Interfaz gráfica en el frontend (React + Tailwind) para consultar, crear y editar plantillas con validación de campos obligatorios.
  - [ ] Validación de unicidad para el código de la plantilla a nivel de modelo de base de datos.
  - [ ] Implementación de borrado lógico (desactivación `is_active=False`).
  - [ ] Auditoría de cambios en cada plantilla (creador, modificador, fechas).

---

### HU-02: Reglas parametrizables para automatización de eventos de negocio
- **Issue:** [#13](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/13)
- **Prioridad:** Alta | **Área:** Backend / Investigación
- **Historia:**
  > **Como** Contador / Analista Funcional Contable,  
  > **quiero** parametrizar reglas contables dentro de cada plantilla (mapeo de cuentas del PCGE al Debe/Haber, fórmulas de distribución porcentual, base imponible e impuestos),  
  > **para** que el motor contable traduzca eventos transaccionales complejos en asientos de partida doble de forma automática y consistente.
- **Criterios de Aceptación:**
  - [ ] Modelo de datos relacional para reglas contables asociadas a una plantilla (orden, cuenta PCGE, Debe/Haber, tipo de cálculo, fórmula/porcentaje, glosa).
  - [ ] Soporte para cálculo automático de IGV (18%) imputando a la subcuenta correspondiente (ej. 40111).
  - [ ] Validación de consistencia teórica en la plantilla.
  - [ ] Registro inicial de 4 reglas estándar de eventos de negocio (Venta de mercaderías, Compra con destino a almacén, Cobranza, Pago a proveedor).
  - [ ] Interfaz frontend para gestionar reglas dentro de la plantilla.

---

### HU-03: API de recepción de eventos y transacciones (POST /v1/asientos)
- **Issue:** [#14](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/14)
- **Prioridad:** Alta | **Área:** Backend / Dependencia Externa
- **Historia:**
  > **Como** Módulo emisor del ERP (Compras, Ventas, Cuentas por Pagar, Almacén, Distribución, RRHH),  
  > **quiero** enviar las operaciones económicas mediante el endpoint REST `POST /v1/asientos` con un payload JSON estructurado,  
  > **para** que el Motor Contable Transaccional las procese, aplique las reglas de contabilización y genere el asiento contable correspondiente de manera automática, consistente y trazable.
- **Criterios de Aceptación:**
  - [ ] Endpoint `POST /v1/asientos` implementado en DRF con validación estricta de esquema y cabeceras.
  - [ ] Idempotencia asegurada: peticiones repetidas con `(modulo_origen, referencia_origen_id)` devuelven el asiento existente sin duplicar registros.
  - [ ] Respuestas HTTP estandarizadas: 201 Created (con ID y CUO), 400 Bad Request, 422 Unprocessable Entity.
  - [ ] Registro de logs estructurados con ID de correlación técnica.

---

### HU-04: Validación estricta de partida doble y consistencia contable
- **Issue:** [#15](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/15)
- **Prioridad:** Alta | **Área:** Backend / QA
- **Historia:**
  > **Como** Motor Contable Transaccional / Auditoría del Sistema,  
  > **quiero** validar matemáticamente que la suma total de débitos (Debe) sea exactamente igual a la suma total de créditos (Haber) ($\sum \text{Debe} = \sum \text{Haber}$) con tolerancia cero en cada asiento contable,  
  > **para** garantizar el principio fundamental de la partida doble y evitar registros contables descuadrados en los libros oficiales y estados financieros.
- **Criterios de Aceptación:**
  - [ ] Verificación obligatoria: $\left| \sum \text{Debe} - \sum \text{Haber} \right| = 0.00$.
  - [ ] Uso de tipos `Decimal` en Python y `NUMERIC(18,2)` en PostgreSQL.
  - [ ] Soporte bimoneda: validación de cuadre en moneda origen y en moneda nacional (PEN) con tipo de cambio oficial del día.
  - [ ] Rechazo atómico e inmediato (rollback de transacción) ante cualquier descuadre.
  - [ ] Batería de pruebas automatizadas con casos balanceados, de borde y de fallo forzado.

---

### HU-05: Ciclo de vida y gestión de estados del asiento contable C6
- **Issue:** [#16](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/16)
- **Prioridad:** Alta | **Área:** Backend / Frontend
- **Historia:**
  > **Como** Contador / Asistente Contable,  
  > **quiero** gestionar el ciclo de vida y la transición de estados de los asientos contables (Borrador, Aprobado y Extornado),  
  > **para** asegurar la inmutabilidad de los registros contables oficiales, permitir revisión previa y realizar anulaciones auditadas mediante extorno sin eliminar registros históricos.
- **Criterios de Aceptación:**
  - [ ] Máquina de estados: `BORRADOR` -> `APROBADO` -> `EXTORNADO`.
  - [ ] Prohibición estricta de borrado físico (`DELETE`) en asientos aprobados o extornados.
  - [ ] Operación de **Extorno**: marca el asiento como `EXTORNADO` y genera automáticamente un contra-asiento espejo balanceado con Debe y Haber invertidos.
  - [ ] Asignación de Correlativo Único de Operación (CUO) al aprobar.

---

### HU-06: Consulta, filtros avanzados y trazabilidad del Libro Diario y Mayor C6
- **Issue:** [#17](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/17)
- **Prioridad:** Media | **Área:** Frontend / Backend
- **Historia:**
  > **Como** Contador / Auditor Interno o Externo,  
  > **quiero** consultar, filtrar y visualizar detalladamente los asientos contables procesados en el formato del Libro Diario y Libro Mayor (Formato SUNAT C6),  
  > **para** verificar la trazabilidad de cada registro contable desde su documento origen hasta sus saldos en el mayor, facilitando auditorías y revisiones fiscales.
- **Criterios de Aceptación:**
  - [ ] Interfaz web interactiva con filtros por fecha, periodo contable, cuenta PCGE, centro de costo y estado.
  - [ ] Vista detallada del asiento con cabecera y líneas Debe/Haber.
  - [ ] Indicadores visuales de estado (badges de colores).
  - [ ] Exportación a CSV y vista previa para impresión de comprobantes de diario.

---

### HU-07: Automatización de asientos periódicos mediante jobs programados
- **Issue:** [#18](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/18)
- **Prioridad:** Media | **Área:** Backend / DevOps
- **Historia:**
  > **Como** Contador General / Administrador del Sistema,  
  > **quiero** automatizar la ejecución periódica de asientos contables mediante tareas en segundo plano programadas (Jobs / Celery / Cron),  
  > **para** asegurar que los asientos recurrentes (provisiones fijas, devengos y amortizaciones) se generen de manera puntual y autónoma sin intervención manual.
- **Criterios de Aceptación:**
  - [ ] Comando Django (`python manage.py procesar_asientos_recurrentes`) ejecutable vía cron/worker.
  - [ ] Ejecución idempotente por periodo contable.
  - [ ] Registro de historial de ejecuciones con tiempos, estados y errores detallados.
  - [ ] Tolerancia a fallos: errores en una regla no detienen la ejecución de las demás.

---

### HU-08: Configuración y catálogo de operaciones recurrentes
- **Issue:** [#19](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/19)
- **Prioridad:** Media | **Área:** Backend / Frontend
- **Historia:**
  > **Como** Asistente Contable / Contador,  
  > **quiero** configurar un catálogo de operaciones recurrentes definiendo frecuencia, periodicidad, vigencia, montos y plantilla contable asociada,  
  > **para** mantener un registro estructurado de los gastos e ingresos periódicos de la empresa (alquileres devengados, depreciación de activo fijo y amortizaciones).
- **Criterios de Aceptación:**
  - [ ] CRUD para operaciones recurrentes (frecuencia mensual/quincenal, día de corte, montos, vigencias y plantilla vinculada).
  - [ ] Configuración inicial de las 4 operaciones fijas principales: Alquileres, Depreciación, Amortización y Seguros devengados.
  - [ ] Funcionalidad para pausar, reanudar o finalizar programaciones recurrentes.
  - [ ] Enlace directo desde la recurrencia al historial de asientos generados en el Libro Diario.

---

### HU-09: Restricción y control de cuentas contables por módulo emisor
- **Issue:** [#21](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/21)
- **Prioridad:** Alta | **Área:** Backend / Gobernanza
- **Historia:**
  > **Como** Contador General / Administrador Contable,  
  > **quiero** configurar una matriz de permisos y restricciones que limite qué cuentas del Plan Contable (PCGE) puede utilizar cada módulo del ERP (ej. Ventas solo puede usar cuentas 12, 40, 70; Compras solo cuentas 60, 40, 42; Planillas solo 62, 40, 41; etc.),  
  > **para** evitar que un módulo externo utilice cuentas contables indebidas o desvirtúe la contabilidad con imputaciones arbitrarias.
- **Criterios de Aceptación:**
  - [ ] Matriz de permisos relacional (`permisos_cuentas_modulo`) que configure módulos autorizados por código o prefijo de cuenta contable.
  - [ ] Validación automática en `POST /v1/asientos`: si un módulo emisor intenta imputar a una cuenta no autorizada, se rechaza la solicitud con HTTP 403 Forbidden o 422 Unprocessable Entity.
  - [ ] Configuración por cuenta del PCGE: moneda permitida (PEN, USD o bimoneda) y obligatoriedad de datos de entidad/auxiliar (RUC/DNI en cuentas 12 y 42).
  - [ ] Interfaz gráfica en el frontend para administrar las reglas de restricción de cuentas por módulo.

---

### HU-10: Mayorización y cálculo automático de saldos (Sacar Cuentas)
- **Issue:** [#22](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/22)
- **Prioridad:** Alta | **Área:** Backend / Frontend
- **Historia:**
  > **Como** Contador / Analista Financiero,  
  > **quiero** que el sistema procese automáticamente los débitos (montos positivos en el Debe) y créditos (montos en el Haber) registrados en el Libro Diario para liquidar y mayorizar los saldos acumulados de cada cuenta ("sacar cuentas"),  
  > **para** obtener en tiempo real el saldo neto (deudor o acreedor) por cuenta y generar el Balance de Comprobación de sumas y saldos.
- **Criterios de Aceptación:**
  - [ ] Motor de agregación que procese débitos y créditos del libro diario para calcular: $\text{Total Debe}$, $\text{Total Haber}$, $\text{Saldo Deudor}$ y $\text{Saldo Acreedor}$ por cada cuenta PCGE.
  - [ ] Generación automática del **Balance de Comprobación** cuadrado: $\sum \text{Debe} = \sum \text{Haber}$ y $\sum \text{Saldo Deudor} = \sum \text{Saldo Acreedor}$.
  - [ ] Consulta jerárquica de saldos (clase, cuenta, subcuenta y divisionaria).
  - [ ] Vista del Libro Mayor C6 con movimientos cronológicos y saldo progresivo por cuenta.

---

### HU-11: Consolidación y generación de Estados Financieros
- **Issue:** [#23](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/23)
- **Prioridad:** Alta | **Área:** Backend / Frontend
- **Historia:**
  > **Como** Gerente Financiero / Contador General,  
  > **quiero** generar y visualizar automáticamente los Estados Financieros oficiales (Balance General y Estado de Ganancias y Pérdidas / Resultados) a partir de los saldos mayorizados de las cuentas del sistema,  
  > **para** conocer la situación económica y patrimonial real de la empresa y respaldar la toma de decisiones estratégicas.
- **Criterios de Aceptación:**
  - [ ] Emisión del **Balance General (Estado de Situación Financiera)** clasificando:
    - Activo Corriente y No Corriente (Elementos 1, 2, 3).
    - Pasivo Corriente y No Corriente (Elemento 4).
    - Patrimonio Neto (Elemento 5).
    - Verificación matemática obligatoria: $\text{Total Activo} = \text{Total Pasivo} + \text{Patrimonio}$.
  - [ ] Emisión del **Estado de Resultados (P&G)**: Ventas netas (70) - Costo de ventas (69) = Utilidad Bruta; Gastos operativos (94, 95); Gastos e ingresos financieros (67, 77); Utilidad neta del ejercicio.
  - [ ] Exportación oficial a formatos PDF y Excel/CSV.

---

### HU-12: Seguimiento de Flujo de Caja y proyección de liquidez
- **Issue:** [#24](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/24)
- **Prioridad:** Alta | **Área:** Backend / Frontend
- **Historia:**
  > **Como** Tesorero / Gerente de Finanzas,  
  > **quiero** proyectar y monitorear el flujo de caja operativo (entradas y salidas de efectivo reales y previstas), evaluando vencimientos de cobros y pagos,  
  > **para** saber exactamente cuándo cobrar y cuándo pagar, detectar atrasos, anticipar iliquidez y tomar decisiones fundamentadas sobre renegociación o solicitud de préstamos.
- **Criterios de Aceptación:**
  - [ ] Consolidación de **Entradas de Efectivo** (Disponibilidades 10 + Cobranzas proyectadas de Cuentas por Cobrar 12 por fecha de vencimiento).
  - [ ] Consolidación de **Salidas de Efectivo** (Obligaciones a proveedores 42 + Tributos 40 + Planillas 41 + Amortizaciones de préstamos 45).
  - [ ] Cálculo de **Brecha de Liquidez** proyectada: $\text{Flujo Neto} = \text{Entradas Previstas} - \text{Salidas Comprometidas}$.
  - [ ] Reporte de **Antigüedad de Deuda y Atrasos** (alertas de cobros vencidos 30/60/90 días y pagos próximos).
  - [ ] **Soporte a la Decisión Financiera:** Alertas ante flujo proyectado negativo (recomendación de préstamos bancarios o financiamiento) y exceso de liquidez (pronto pago con descuento).
  - [ ] Dashboard interactivo con calendario financiero de cobros y pagos.
