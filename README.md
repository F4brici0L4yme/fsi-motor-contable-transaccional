# ERP FSI — Motor Contable Transaccional

Proyecto académico de la Escuela Profesional de Ingeniería de Sistemas de la Universidad Nacional de San Agustín de Arequipa, desarrollado para el curso de Fundamentos de Sistemas de Información.

Este repositorio reúne el trabajo del módulo **Motor Contable Transaccional (Módulo II: Operaciones Contables y Motor de Automatización - Core)**, perteneciente a la Tribu Financiera. Su código se integrará posteriormente al repositorio común del ERP institucional, según la coordinación entre equipos.

---

## Objetivo

El objetivo del módulo Motor Contable Transaccional es recibir las operaciones económicas que generan los demás módulos del ERP (compras, cuentas por pagar, ventas, almacén, distribución, entre otros) y convertirlas en asientos contables en partida doble, de forma automática, consistente y trazable.

Para ello, el módulo se apoya en el Plan Contable General Empresarial (PCGE) y en los catálogos oficiales de la Superintendencia Nacional de Aduanas y de Administración Tributaria (SUNAT), de modo que cada operación quede registrada con las cuentas, el tipo de comprobante, la moneda y la glosa correspondientes. Asimismo, sirve de base para la consulta del libro diario, el libro mayor y el balance de comprobación, y para la generación de los libros electrónicos exigidos por SUNAT (PLE).

---

## Equipo

| Integrante | Rol |
| :--- | :--- |
| LAYME SALAS, RODRIGO FABRICIO | Scrum Master |
| NAGIN JAZMIN, CARFER CARPIO | Product Owner |
| ARAGON CARPIO, FREDY JOSÉ | Backend |
| CONDORI IDME, RAÚL WILFREDO | Frontend |
| CALLO CCAGIAVILCA, KEVIN JOEL | QA / Pruebas |
| CARLOS CCAMAQQUE, WILSON FREDDY | Arquitectura |
| DEL CASTILLO MONTOYA, CHRISTOPHER BRAD | DevOps |
| SARMIENTO TICO, LIMBERG | Patrones de diseño |

---

## Alcance y Primera Implementación

El equipo está a cargo del **MÓDULO II: OPERACIONES CONTABLES Y MOTOR DE AUTOMATIZACIÓN (CORE)** según las especificaciones del sistema financiero y contable global. Sus componentes funcionales son:

### 3.1. CRUD Maestro: Plantillas de Contabilización (Motor Contable)
* **Reglas Parametrizables para Automatización (3.1.1):** Mecanismo dinámico que permite definir cuentas deudoras y acreedoras del PCGE, cálculo automático de bases imponibles y desglose de tributos (IGV 18%).
* **Operaciones CRUD para Reglas Contables (3.1.2):** Mantenimiento de plantillas maestras y sus reglas asociadas, garantizando borrado lógico y versionado.
* **Catálogo de Reglas para Eventos de Negocio (3.1.3):** Reglas para eventos estándar (Venta con factura/boleta, Compra de mercaderías con destino a almacén, Pagos a proveedores y Cobranzas a clientes).

### 3.2. Submódulo: Gestión de Asientos Diarios (Libro Mayor C6)
* **Ciclo de Vida y Estados del Asiento C6 (3.2.1):** Control estricto de estados (`BORRADOR` $\rightarrow$ `APROBADO` $\rightarrow$ `EXTORNADO`). Se prohíbe la eliminación física; las anulaciones generan contra-asientos automáticos de extorno.
* **Lógica de Validación de Partida Doble (3.2.2):** Verificación matemática rigurosa donde $\left| \sum \text{Debe} - \sum \text{Haber} \right| = 0.00$ con alta precisión decimal (`NUMERIC(18,2)` / `Decimal`), tanto en moneda nacional como extranjera.
* **Especificación de API de Recepción (3.2.3):** Endpoint REST `POST /v1/asientos` para recepción síncrona o asíncrona de eventos transaccionales, implementando idempotencia para evitar registros duplicados.
* **Formato C6 y Trazabilidad (3.2.4):** Visualización y exportación conforme a los formatos oficiales SUNAT de Libro Diario (5.1) y Libro Mayor (6.1).

### 3.3. Submódulo: Transacciones Recurrentes
* **Automatización mediante Jobs Programados (3.3.1):** Tareas en segundo plano (Cron / Background Workers) para procesar asientos periódicos según fechas de corte preestablecidas.
* **Catálogo de Operaciones Recurrentes (3.3.2):** Parametrización de transacciones fijas (alquileres devengados, depreciación mensual de activo fijo y amortización de intangibles).

---

## Organización y Estructura del Proyecto

El repositorio adopta una arquitectura desacoplada para frontend, backend y documentación:

```text
fsi-motor-contable-transaccional/
├── .github/                      # Plantillas de issues (HU, tareas, bloqueos) y flujos de PR
├── backend/                      # API REST en Python (Django + DRF)
├── database/                     # Scripts DDL, migraciones y esquemas PostgreSQL
├── docs/                         # Documentación de arquitectura, backlog, flujos y DoD
│   ├── backlog.md                # Backlog detallado de Historias de Usuario
│   ├── ceremonias.md             # Definición de dailies, sprint planning y reviews
│   ├── definition-of-done.md     # Criterios de aceptación y terminación de tareas
│   ├── dependencias-entre-modulos.md # Contratos con módulos externos del ERP
│   ├── diccionario-de-datos.md   # Estructura de entidades y tablas relacionales
│   ├── flujo-de-trabajo.md       # GitFlow (main, develop, ramas de soporte)
│   ├── stack-tecnologico.md      # Tecnologías y dependencias acordadas
│   └── tablas-compartidas.md     # Mapeo de tablas maestras del ERP
├── frontend/                     # Aplicación web (React + Vite + TypeScript + Tailwind)
├── tests/                        # Pruebas automatizadas (unitarias, integración, E2E)
├── .editorconfig                 # Reglas de formato de código transversal
└── README.md                     # Visión general y guía del módulo
```

---

## Propuesta de Tecnologías

Stack tecnológico oficial acordado por votación de la tribu:

| Capa | Tecnología | Propósito |
| :--- | :--- | :--- |
| **Backend** | Python 3.11+ / Django REST Framework (DRF) | API REST de alto rendimiento, ORM robusto y tipado contable |
| **Frontend** | React 18+ / Vite / TypeScript | Interfaz de usuario reactiva, tipado estático y velocidad de carga |
| **Estilos** | Tailwind CSS | Sistema de diseño modular y responsivo |
| **Base de Datos** | PostgreSQL 16+ | Integridad relacional, soporte ACID y tipos `NUMERIC(18,2)` |
| **Background Jobs** | Celery / Redis o Django Management Commands + Cron | Automatización de asientos recurrentes y tareas batch |
| **Contenedores** | Docker & Docker Compose | Entorno de desarrollo local reproducible |

---

## Historias de Usuario y Tareas

El Product Backlog del Módulo II está estructurado en 8 Historias de Usuario principales con trazabilidad completa a los requerimientos:

| Código | Título de la Historia de Usuario | Requerimiento | Prioridad | Issue |
| :---: | :--- | :---: | :---: | :---: |
| **HU-01** | CRUD maestro de plantillas de contabilización para el motor contable | 3.1.2 | **Alta** | [#12](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/12) |
| **HU-02** | Reglas parametrizables para automatización de eventos de negocio | 3.1.1 / 3.1.3 | **Alta** | [#13](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/13) |
| **HU-03** | API de recepción de eventos y transacciones (POST /v1/asientos) | 3.2.3 | **Alta** | [#14](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/14) |
| **HU-04** | Validación estricta de partida doble y consistencia contable | 3.2.2 | **Alta** | [#15](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/15) |
| **HU-05** | Ciclo de vida y gestión de estados del asiento contable C6 | 3.2.1 / 3.2.4 | **Alta** | [#16](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/16) |
| **HU-06** | Consulta, filtros avanzados y trazabilidad del Libro Diario y Mayor C6 | 3.2.4 | **Media** | [#17](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/17) |
| **HU-07** | Automatización de asientos periódicos mediante jobs programados | 3.3.1 | **Media** | [#18](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/18) |
| **HU-08** | Configuración y catálogo de operaciones recurrentes (alquileres/depreciaciones) | 3.3.2 | **Media** | [#19](https://github.com/F4brici0L4yme/fsi-motor-contable-transaccional/issues/19) |

> Para conocer los criterios de aceptación detallados y las notas técnicas de cada historia, consultar [docs/backlog.md](docs/backlog.md).

---

## Integración con Otros Módulos

El Motor Contable Transaccional actúa como el receptor central de todas las operaciones con impacto financiero generadas por los demás módulos del ERP institucional.

### Contrato de Integración Principal: `POST /v1/asientos`

Los módulos emisores envían una petición HTTP POST con el siguiente esquema JSON de referencia:

```json
{
  "modulo_origen": "VENTAS",
  "evento_negocio": "VENTA_FACTURA",
  "referencia_origen_id": "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
  "fecha_emision": "2026-11-06T10:30:00Z",
  "tipo_comprobante": "01",
  "serie_comprobante": "F001",
  "numero_comprobante": "00004589",
  "moneda": "PEN",
  "tipo_cambio": 1.000,
  "glosa": "Por venta de mercaderías según factura F001-00004589",
  "tercero": {
    "tipo_documento": "6",
    "numero_documento": "20123456789",
    "razon_social": "DISTRIBUIDORA COMERCIAL ANDINA S.A.C."
  },
  "detalle_financiero": {
    "monto_total": 1180.00,
    "base_imponible": 1000.00,
    "monto_igv": 180.00,
    "centro_costo_id": "CC-VENTAS-SUR"
  }
}
```

### Respuestas del Endpoint:
* **`201 Created`**: Asiento procesado exitosamente. Retorna el ID asignado, número de asiento/CUO, estado (`APROBADO`), sumatoria Debe/Haber y hash de integridad.
* **`200 OK` (Idempotencia)**: Si la tupla `(modulo_origen, referencia_origen_id)` ya fue procesada, responde con el asiento existente sin duplicar líneas contables.
* **`400 Bad Request`**: Datos faltantes o tipos incompatibles en el payload.
* **`422 Unprocessable Entity`**: Error de cuadre de partida doble o ausencia de regla contable aplicable para el evento.

### Matriz de Módulos Integrados:
* **Compras & Cuentas por Pagar:** Registro de facturas de proveedores (Cuentas 60, 40, 42) y liquidación de pagos (Cuentas 42, 10).
* **Ventas Virtuales & Comercio Electrónico:** Registro de facturación (Cuentas 12, 40, 70) y cobranzas pasarela/banco (Cuentas 10, 12).
* **Almacén & Distribución:** Asientos de variación de existencias y costos de ventas (Cuentas 69, 20).
* **Control de Asistencia & Recursos Humanos:** Asientos de provisiones de planilla de sueldos y cargas sociales (Cuentas 62, 40, 41).
