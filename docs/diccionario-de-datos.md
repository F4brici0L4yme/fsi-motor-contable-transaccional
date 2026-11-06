# Diccionario de datos — Motor Contable Transaccional

Diccionario de datos maestro de las entidades del **Módulo II: Operaciones Contables y Motor de Automatización (Core)**. Diseñado por Arquitectura y Backend para su implementación en PostgreSQL.

---

## 1. Tabla: `plantillas_contabilizacion`
Define las plantillas maestras asociadas a eventos de negocio para la generación automática de asientos.

| Campo | Tipo | Nulo | Clave | Descripción | Origen / Catálogo |
| :--- | :--- | :---: | :---: | :--- | :--- |
| `id` | `uuid` | No | PK | Identificador único universal de la plantilla | Generado internamente (`uuid_generate_v4()`) |
| `codigo` | `varchar(30)` | No | UK | Código alfanumérico identificador (ej. `PLANT_VENTA_FACT`) | Único por sistema |
| `nombre` | `varchar(150)` | No | — | Nombre descriptivo de la plantilla | Definido por el contador |
| `modulo_origen` | `varchar(50)` | No | — | Módulo emisor del evento (VENTAS, COMPRAS, RRHH, etc.) | Módulos del ERP |
| `evento_negocio` | `varchar(50)` | No | — | Código del evento que activa la plantilla | Catálogo interno de eventos |
| `descripcion` | `text` | Sí | — | Explicación funcional del propósito contable | Opcional |
| `is_active` | `boolean` | No | — | Indicador de borrado lógico / estado de vigencia | Default: `true` |
| `created_at` | `timestamptz` | No | — | Fecha y hora de creación | Timestamp automático |
| `updated_at` | `timestamptz` | No | — | Fecha y hora de última modificación | Timestamp automático |

---

## 2. Tabla: `reglas_contables`
Detalle de las reglas de imputación contable parametrizadas para cada plantilla.

| Campo | Tipo | Nulo | Clave | Descripción | Origen / Catálogo |
| :--- | :--- | :---: | :---: | :--- | :--- |
| `id` | `uuid` | No | PK | Identificador único de la regla | `uuid_generate_v4()` |
| `plantilla_id` | `uuid` | No | FK | Referencia a la plantilla padre | `plantillas_contabilizacion.id` |
| `orden` | `integer` | No | — | Secuencia de evaluación y generación de línea | Orden correlativo (1, 2, 3...) |
| `cuenta_codigo` | `varchar(20)` | No | FK | Código de la cuenta contable de destino | PCGE (Módulo I) |
| `naturaleza` | `varchar(5)` | No | — | Posición contable: `DEBE` o `HABER` | `DEBE` / `HABER` |
| `tipo_calculo` | `varchar(30)` | No | — | Modo de cálculo (`MONTO_TOTAL`, `BASE_IMPONIBLE`, `IGV`, `PORCENTAJE`) | Enum de cálculo |
| `porcentaje` | `numeric(5,2)` | Sí | — | Porcentaje a aplicar si el tipo de cálculo es porcentual | Ej. `18.00` para IGV |
| `glosa_patron` | `varchar(255)` | Sí | — | Plantilla de glosa para la línea del asiento | Admite interpolación de texto |

---

## 3. Tabla: `asientos_contables`
Cabecera del asiento contable oficial (Libro Diario / Libro Mayor formato SUNAT C6).

| Campo | Tipo | Nulo | Clave | Descripción | Origen / Catálogo |
| :--- | :--- | :---: | :---: | :--- | :--- |
| `id` | `uuid` | No | PK | Identificador único del asiento contable | `uuid_generate_v4()` |
| `cuo` | `varchar(20)` | Sí | UK | Correlativo Único de Operación oficial SUNAT | Generado al aprobar asiento |
| `periodo_id` | `varchar(6)` | No | FK | Periodo contable en formato `YYYYMM` | Periodos Contables |
| `fecha_contable` | `date` | No | — | Fecha del registro contable en libros | Formato `YYYY-MM-DD` |
| `fecha_emision` | `date` | No | — | Fecha de emisión de la transacción/comprobante | Fecha del documento fuente |
| `modulo_origen` | `varchar(50)` | No | — | Módulo ERP donde ocurrió el evento de negocio | COMPRAS, VENTAS, etc. |
| `referencia_origen_id` | `varchar(100)` | No | — | ID de la transacción en el módulo de origen (idempotencia) | Clave única externa |
| `tipo_comprobante` | `varchar(2)` | Sí | — | Tipo de comprobante de pago | Catálogo 01 SUNAT |
| `serie_comprobante` | `varchar(10)` | Sí | — | Serie del comprobante físico/electrónico | Ej. `F001`, `B001` |
| `numero_comprobante` | `varchar(20)` | Sí | — | Correlativo del comprobante | Ej. `00004589` |
| `tercero_tipo_doc` | `varchar(1)` | Sí | — | Tipo de documento de identidad del tercero | Catálogo 06 SUNAT (6=RUC, 1=DNI) |
| `tercero_num_doc` | `varchar(15)` | Sí | — | Número de RUC o DNI del cliente/proveedor | Registro del tercero |
| `tercero_razon_social` | `varchar(200)` | Sí | — | Apellidos y nombres o denominación social | Datos del tercero |
| `moneda` | `varchar(3)` | No | — | Código ISO de la moneda (PEN / USD) | Catálogo 02 SUNAT |
| `tipo_cambio` | `numeric(10,4)` | No | — | Tipo de cambio venta oficial SUNAT a la fecha | Módulo III (Tesorería) |
| `glosa_principal` | `text` | No | — | Descripción o glosa general de la operación | Texto descriptivo |
| `total_debe` | `numeric(18,2)` | No | — | Sumatoria total de los débitos | `NUMERIC(18,2)` |
| `total_haber` | `numeric(18,2)` | No | — | Sumatoria total de los créditos | `NUMERIC(18,2)` |
| `estado` | `varchar(15)` | No | — | Estado del ciclo de vida (`BORRADOR`, `APROBADO`, `EXTORNADO`) | Máquina de estados |
| `asiento_origen_id` | `uuid` | Sí | FK | Referencia al asiento original si este es un extorno | Autorreferencial |
| `asiento_extorno_id` | `uuid` | Sí | FK | Referencia al asiento de extorno que anuló este registro | Autorreferencial |
| `motivo_extorno` | `text` | Sí | — | Justificación auditada de anulación | Obligatorio si es extornado |
| `created_at` | `timestamptz` | No | — | Fecha y hora de creación | Timestamp automático |
| `approved_at` | `timestamptz` | Sí | — | Fecha y hora de aprobación definitiva | Timestamp de aprobación |

---

## 4. Tabla: `lineas_asiento`
Detalle de imputaciones contables en partida doble para cada asiento.

| Campo | Tipo | Nulo | Clave | Descripción | Origen / Catálogo |
| :--- | :--- | :---: | :---: | :--- | :--- |
| `id` | `uuid` | No | PK | Identificador único de la línea | `uuid_generate_v4()` |
| `asiento_id` | `uuid` | No | FK | Referencia al asiento contable cabecera | `asientos_contables.id` |
| `numero_linea` | `integer` | No | — | Número correlativo de la línea dentro del asiento | 1, 2, 3... |
| `cuenta_codigo` | `varchar(20)` | No | FK | Código de la subcuenta del PCGE | Catálogo PCGE (Módulo I) |
| `centro_costo_id` | `varchar(30)` | Sí | — | Código de dimensión o centro de costo | Segmentación (Módulo I) |
| `glosa_linea` | `varchar(255)` | Sí | — | Glosa específica de la línea | Opcional |
| `debe` | `numeric(18,2)` | No | — | Monto debitado en moneda de la transacción | Mayor o igual a 0.00 |
| `haber` | `numeric(18,2)` | No | — | Monto acreditado en moneda de la transacción | Mayor o igual a 0.00 |
| `debe_pen` | `numeric(18,2)` | No | — | Monto debitado convertido a moneda nacional (PEN) | Cálculo con tipo de cambio |
| `haber_pen` | `numeric(18,2)` | No | — | Monto acreditado convertido a moneda nacional (PEN) | Cálculo con tipo de cambio |

---

## 5. Tabla: `transacciones_recurrentes`
Definición de transacciones periódicas para su ejecución automática por el scheduler.

| Campo | Tipo | Nulo | Clave | Descripción | Origen / Catálogo |
| :--- | :--- | :---: | :---: | :--- | :--- |
| `id` | `uuid` | No | PK | Identificador de la programación recurrente | `uuid_generate_v4()` |
| `codigo` | `varchar(30)` | No | UK | Código identificador (ej. `REC_ALQUILER_SUR`) | Único |
| `descripcion` | `varchar(200)` | No | — | Concepto o nombre de la operación periódica | Ej. "Depreciación de Maquinaria" |
| `plantilla_id` | `uuid` | No | FK | Plantilla de contabilización a aplicar | `plantillas_contabilizacion.id` |
| `frecuencia` | `varchar(20)` | No | — | Frecuencia: `MENSUAL`, `QUINCENAL`, `ANUAL` | Enum de frecuencia |
| `dia_corte` | `integer` | No | — | Día del mes en que debe ejecutarse (1 al 31) | Calendario |
| `monto_base` | `numeric(18,2)` | No | — | Importe base para el cálculo | `NUMERIC(18,2)` |
| `moneda` | `varchar(3)` | No | — | Moneda de la transacción (`PEN`, `USD`) | Catálogo 02 SUNAT |
| `fecha_inicio` | `date` | No | — | Fecha inicial de vigencia del contrato o devengo | Formato `YYYY-MM-DD` |
| `fecha_fin` | `date` | Sí | — | Fecha final (nulo si es indefinido) | Formato `YYYY-MM-DD` |
| `estado` | `varchar(15)` | No | — | Estado: `ACTIVO`, `PAUSADO`, `FINALIZADO` | Enum de control |

---

## 6. Tabla: `historial_ejecucion_recurrente`
Log de auditoría de cada ejecución del job de transacciones periódicas.

| Campo | Tipo | Nulo | Clave | Descripción | Origen / Catálogo |
| :--- | :--- | :---: | :---: | :--- | :--- |
| `id` | `uuid` | No | PK | Identificador del registro de ejecución | `uuid_generate_v4()` |
| `recurrencia_id` | `uuid` | No | FK | Referencia a la transacción recurrente programada | `transacciones_recurrentes.id` |
| `fecha_ejecucion` | `timestamptz` | No | — | Marca de tiempo exacta del inicio de la ejecución | Timestamp |
| `periodo_procesado` | `varchar(6)` | No | — | Periodo tributario procesado (`YYYYMM`) | Ej. `202611` |
| `estado` | `varchar(15)` | No | — | Resultado: `EXITOSO`, `ERROR` | Enum |
| `asiento_id` | `uuid` | Sí | FK | Asiento contable generado (si fue exitoso) | `asientos_contables.id` |
| `mensaje_error` | `text` | Sí | — | Traza de error detallada si falló | Diagnóstico técnico |
| `duracion_ms` | `integer` | No | — | Tiempo de ejecución en milisegundos | Métrica de desempeño |
