# Plan de commits

Este plan organiza la publicación en el repositorio público. Se agrupa por entregable para que cada commit pueda revisarse con claridad.

## Commit 1 — Inicial del repositorio

**Mensaje:** `Initial commit` (creado por el usuario al iniciar el repositorio).

## Commit 2 — Organización y Scrum

**Mensaje:** `SCRUM-5 docs: publish project workflow and Scrum backlog`

Archivos: `README.md`, `.gitignore`, `Text_Prompt_Maestro.txt`, análisis inicial, Scrum, bitácora, backlog Jira y este plan.

## Commit 3 — Fase 1 conceptual

**Mensaje:** `SCRUM-5 docs: publish approved conceptual phase`

Archivos: `docs/01_analisis_semantico.md`, `docs/02_catalogo_conceptual.md`, `docs/03_diagrama_conceptual.md`, `docs/03_diagrama_conceptual.drawio`, `docs/04_trazabilidad_conceptual.csv`.

El usuario aprobó la fase conceptual. Las preguntas Q01–Q03 permanecen abiertas para precisar el modelo lógico.

## Commit 3 y siguientes — Fases aprobadas

Cada fase aprobada tendrá uno o más commits con un alcance pequeño, mensaje que describa el cambio y una clave Jira. Ejemplo: `SCRUM-6 docs: close conceptual questions`. No marcar una historia como iniciada por la mera presencia de una clave hasta que la automatización se configure y compruebe.

## Archivos de referencia del curso

La guía del proyecto y el ejemplo de P01 se conservarán como referencias locales. No se incluyen en los commits propuestos por ahora; el repositorio contendrá las decisiones y entregables de este proyecto y el Prompt Maestro.

## Estado

- Repositorio remoto: [jjCastillok/DB-Transporte-publico-y-mototaxismo](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo), público, rama `main`.
- Jira muestra GitHub for Atlassian instalado; conexión de este repositorio y regla automática de commits a `En curso` pendientes de verificación.
- Los commits agrupados se publicarán con las herramientas oficiales de GitHub; no incluirán los documentos de referencia del curso.
- El historial verificado también está enlazado en `docs/bitacora.md`.
