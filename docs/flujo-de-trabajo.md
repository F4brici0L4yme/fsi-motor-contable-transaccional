# Flujo de trabajo

## Ramas

| Rama | Propósito |
| :--- | :--- |
| `main` | Rama estable. Solo recibe cambios integrados y revisados. |
| `develop` | Rama de integración del equipo. Aquí se juntan las tareas terminadas. |
| `feat/<descripcion>` | Nueva funcionalidad. Sale de `develop`. |
| `fix/<descripcion>` | Corrección de un defecto. Sale de `develop`. |
| `docs/<descripcion>` | Cambios en documentación. Sale de `develop`. |
| `chore/<descripcion>` | Tareas de mantenimiento, configuración o estructura. Sale de `develop`. |
| `test/<descripcion>` | Pruebas y aseguramiento de calidad. Sale de `develop`. |

Reglas:

- Las ramas de trabajo se crean **desde `develop`**, no desde `main`.
- El nombre va en minúsculas, con guiones y sin acentos. Ejemplo: `feat/registro-asiento-contable`.
- No se hace push directo a `main` ni a `develop`: todo entra por Pull Request.
- Una rama se elimina después de fusionarse.
- Mantén tu rama actualizada con `develop` (`git merge develop` o `git rebase develop`) antes de pedir revisión.

## Convención de commits

Formato: `tipo: descripción en minúsculas`

Tipos permitidos: `feat`, `fix`, `docs`, `chore`, `test`.

| Ejemplo | Cuándo usarlo |
| :--- | :--- |
| `feat: agrega endpoint de registro de asientos` | Funcionalidad nueva |
| `fix: corrige validación de moneda en asiento` | Corrección de un error |
| `docs: actualiza diccionario de datos` | Solo documentación |
| `chore: configura docker compose del backend` | Configuración o mantenimiento |
| `test: agrega pruebas del cálculo de partida doble` | Pruebas |

Buenas prácticas:

- Un commit = un cambio coherente y pequeño.
- La descripción va en minúsculas, sin punto final.
- Si el commit cierra un issue, indícalo en el cuerpo: `Closes #12`.

## Ciclo del Pull Request

1. Crear la rama desde `develop`.
2. Hacer commits pequeños siguiendo la convención.
3. Abrir el PR hacia `develop` (o hacia `main` en el caso de un pase de integración).
4. Completar la plantilla del PR: issue relacionado, cambios, cómo se comprobó y checklist de la Definition of Done.
5. Asignar un revisor distinto del autor.
6. Atender los comentarios y resolver las conversaciones.
7. **Mínimo 1 aprobación.** El autor no puede aprobar su propio PR.
8. Fusionar con *Squash and merge* y eliminar la rama.

Reglas adicionales:

- Si el PR cambia el contrato de una API o una tabla compartida, se avisa en el issue y se etiqueta `dependencia-externa` o `decision-pendiente`.
- Un PR sin descripción o sin checklist completo no se revisa.
- Si el PR queda sin actividad por más de dos días, el Scrum Master lo escala en el daily.

## Estados del tablero

| Estado | Significado |
| :--- | :--- |
| **Pendiente** | La tarea está en el sprint/backlog y todavía no se empieza. |
| **En proceso** | Alguien está trabajando en ella; existe rama y commits. |
| **En revisión** | Hay Pull Request abierto esperando revisión o ajustes. |
| **Terminado** | El PR fue fusionado y la Definition of Done se cumple. |

Reglas del tablero:

- Cada integrante mueve sus propias tarjetas.
- No se pasa a **En revisión** sin PR abierto y descripción completa.
- No se pasa a **Terminado** sin aprobación y sin cumplir la Definition of Done.
- Un bloqueo se registra en `docs/registro-impedimentos.md` y se etiqueta con `bloqueo`.
