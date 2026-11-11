# Plan de commits

Este plan organiza la publicación en el repositorio público. Se agrupa por entregable para que cada commit pueda revisarse con claridad.

## Commit 1 — Organización del proyecto

**Mensaje propuesto:** `docs: establish project workflow and scrum`

Archivos: `README.md`, `.gitignore`, `Text_Prompt_Maestro.txt`, `docs/analisis-inicial.md`, `docs/scrum.md`, `docs/bitacora.md`, `docs/plan_commits.md`, `docs/backlog_jira_propuesta.md`.

## Commit 2 — Fase 1 conceptual

**Mensaje propuesto:** `docs: add phase 1 conceptual analysis`

Archivos: `docs/01_analisis_semantico.md`, `docs/02_catalogo_conceptual.md`, `docs/03_diagrama_conceptual.md`, `docs/03_diagrama_conceptual.drawio`, `docs/04_trazabilidad_conceptual.csv`.

Este commit se publicará después de registrar las correcciones de revisión de Fase 1. El estado de aprobación de la fase quedará consignado en la bitácora.

## Commit 3 y siguientes — Fases aprobadas

Cada fase aprobada tendrá uno o más commits con un alcance pequeño, mensaje que describa el cambio y una clave Jira. Ejemplo: `SCRUM-6 docs: close conceptual questions`. No marcar una historia como iniciada por la mera presencia de una clave hasta que la automatización se configure y compruebe.

## Archivos de referencia del curso

La guía del proyecto y el ejemplo de P01 se conservarán como referencias locales. No se incluyen en los commits propuestos por ahora; el repositorio contendrá las decisiones y entregables de este proyecto y el Prompt Maestro.

## Estado

- Repositorio remoto: [jjCastillok/DB-Transporte-publico-y-mototaxismo](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo), público, rama `main`.
- Jira muestra GitHub for Atlassian instalado; conexión de este repositorio y regla automática de commits a `En curso` pendientes de verificación.
- Los commits agrupados se publicarán con las herramientas oficiales de GitHub; no incluirán los documentos de referencia del curso.
