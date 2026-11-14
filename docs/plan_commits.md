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

## Commit 4 — Seguimiento de la integración

**Mensaje:** `SCRUM-14 docs: record Jira automation setup and history`

Archivos: `README.md`, `docs/bitacora.md`, `docs/plan_commits.md`, `docs/automatizacion_jira.md`.

## Commit 5 — Reconciliar historial

**Mensaje:** `SCRUM-14 docs: reconcile Jira and GitHub history`

Archivos: bitácora y plan de commits.

## Commit 6 — Corregir fechas del Sprint en zona local

**Mensaje:** `SCRUM-5 docs: clarify active sprint dates in Bogota time`

Archivo: `docs/scrum.md`.

## Commit 7 — Sincronizar plan e historial

**Mensaje:** `SCRUM-5 docs: sync Sprint dates and commit history`.

Archivos: bitácora y plan de commits.

## Commits posteriores — Fases aprobadas

Cada fase aprobada tendrá uno o más commits con un alcance pequeño, mensaje que describa el cambio y una clave Jira. Ejemplo: `SCRUM-6 docs: close conceptual questions`. No marcar una historia como iniciada por la mera presencia de una clave hasta que la automatización se configure y compruebe.

## Commit 8 — Decisiones P11 y propuesta lógica

**Mensaje:** `SCRUM-7 docs: draft normalized logical model`

**Archivos:** análisis y evidencias conceptuales revisadas, modelo lógico E5, normalización E6, diagrama relacional E7, trazabilidad E8, seguimiento Scrum y bitácora.

**Estado:** documentos preparados y REVISADOS ESTÁTICAMENTE; Fase 2 pendiente de aprobación del usuario. Sin SGBD, SQL/DDL, datos sintéticos ni pruebas.

Publicado en [commit 4145c18](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/4145c1820fede67ecce237eb9749fbb57bd55ede) el 2026-11-14.

## Commit 9 — Organizar columnas del board Jira

**Mensaje:** `SCRUM-5 docs: map Jira board columns and work status`

**Archivos:** `README.md`, `docs/scrum.md`, `docs/backlog_jira_propuesta.md`, `docs/bitacora.md` y este plan.

**Alcance:** registrar que Jira presenta un único board visible con cuatro estados, SCRUM-6 finalizado y SCRUM-7 en curso dentro de Sprint 0. La columna Pruebas queda sin P11 hasta que existan pruebas documentadas.

Publicado en [commit a4c926d](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/a4c926df13ba713cbc3ded2ab00b1f1edcff8c10).

## Commit 10 — Refinar la propuesta lógica E5–E8

**Mensaje:** `SCRUM-7 docs: refine logical model and 3NF evidence`

**Archivos:** análisis y trazabilidad conceptual actualizados; modelo lógico E5, normalización E6, diagrama lógico E7, trazabilidad E8, Scrum y bitácora.

**Alcance:** enlazar cada paso de ruta con el conjunto de paraderos habilitados, explicitar supuestos pendientes y revisar las dependencias funcionales. Estado PROPUESTO / REVISADO ESTÁTICAMENTE; la fase requiere aprobación y no contiene DDL ni pruebas de SGBD.

Publicado en cuatro commits de alcance individual: [E5 modelo lógico](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/ddc4335d1a87d0be47c7d1cd509edbee0951a019), [E6 normalización](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/b819435d224fc2542a9000e1a8bc00b8bd8ae5a9), [E7 diagrama lógico](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/70e1193dc3bb80a86b9de6e3164030ae3645dafa) y [E8 trazabilidad](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/cd79122e8da70d245d525b3a7f900b9fb07c1a53).

La Fase 2 continúa pendiente de aprobación del usuario. Jira queda en pausa por decisión del usuario; no se modificaron tableros ni tickets durante esta actualización.

## Archivos de referencia del curso

La guía del proyecto y el ejemplo de P01 se conservarán como referencias locales. No se incluyen en los commits propuestos por ahora; el repositorio contendrá las decisiones y entregables de este proyecto y el Prompt Maestro.

## Estado

- Repositorio remoto: [jjCastillok/DB-Transporte-publico-y-mototaxismo](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo), público, rama `main`.
- Jira muestra GitHub for Atlassian instalado; conexión de este repositorio y regla automática de commits a `En curso` pendientes de verificación.
- Los commits agrupados se publican con las herramientas de GitHub; no incluyen los documentos de referencia del curso.
- El historial verificado también está enlazado en `docs/bitacora.md`.

