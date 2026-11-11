# Bitácora de trabajo

Registro cronológico para saber qué se hizo, qué falta y cómo retomar el proyecto.

## Actualización — Jira y GitHub (2026-11-10)

- El usuario confirmó crear y comentar los tickets Jira, trabajar desde el repositorio público y mover las tareas listas al Sprint activo.
- Repositorio verificado: [jjCastillok/DB-Transporte-publico-y-mototaxismo](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo), rama principal `main`, público, con commit inicial.
- Se creó el Epic [SCRUM-5](https://jjjk3022.atlassian.net/browse/SCRUM-5) y nueve historias con criterio de aceptación y comentario inicial: HU-01 `SCRUM-6`, HU-02 `SCRUM-7`, HU-03 `SCRUM-8`, HU-04 `SCRUM-9`, HU-05 `SCRUM-10`, HU-06 `SCRUM-11`, HU-07 `SCRUM-12`, HU-08 `SCRUM-13` y HU-09 `SCRUM-14`.
- El Epic tiene un enlace web al repositorio GitHub.
- `SCRUM Sprint 0` (ID 2) ya estaba activo. Se agregó únicamente `SCRUM-6`; no se reinició el ciclo ni se tocaron las fechas. Las respuestas Q01–Q03 siguen pendientes.
- Jira muestra GitHub for Atlassian instalado. No se verificó la conexión efectiva al repositorio ni una regla automática activa.
- La API Jira disponible no ofrece la operación de creación de reglas de automatización. HU-09 registra la configuración y prueba pendientes desde Jira UI.
- No se modificaron tickets genéricos preexistentes.
- Se publicaron los artefactos del proyecto en `main` mediante dos commits agrupados: [organización y backlog](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/e5d212d6a0a20137d28936383afcc79f30567811) (`SCRUM-5 docs: publish project workflow and Scrum backlog`) y [Fase 1 conceptual](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/e62b42b8c744c0fbdee9a809b998ae5877a87865) (`SCRUM-5 docs: publish approved conceptual phase`).
- Se publicó [la guía de automatización Jira–GitHub](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/6426f64cef432ac076883eaff85ef76060af88e3) con el mensaje `SCRUM-14 docs: record Jira automation setup and history`; distingue el estado observado de la configuración aún pendiente.
- La bitácora y el plan de commits se reconciliaron en [SCRUM-14 docs: reconcile Jira and GitHub history](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/489360e4a006021fa882647ce2650b364905405f).
- El Sprint se documenta en hora de Bogotá: 10 nov 2026 22:26–24 nov 2026 22:26. Jira almacena los instantes correspondientes en UTC.
- Se publicó la precisión horaria como [SCRUM-5 docs: clarify active sprint dates in Bogota time](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/aa76159797174df6dbb516a45f67a22848e30ec5).
- El repositorio contiene README, Prompt Maestro, organización Scrum, bitácora, backlog, análisis y evidencias E1–E4. Los Word de referencia del curso se mantienen fuera del repositorio público.

## 2026-11-10 — Inicio y revisión de materiales

- Se identificaron dos archivos Word: la guía del proyecto integrador y un ejemplo resuelto para el caso P01 del Parque Tayrona.
- Se extrajeron los requisitos generales de la guía: proyecto de base de datos relacional, normalización hasta 3FN, fases de diseño e implementación y documentación técnica.
- El usuario aclaró que el dominio del proyecto es **Transporte público y mototaxismo: rutas, flota y cumplimiento**, y que el banco de problemas no define el proyecto.
- Se separaron los requisitos particulares del caso P01 para evitar aplicarlos por error al dominio de transporte.
- Se creó una estructura inicial con análisis, propuesta Scrum y bitácora.
- Se propuso GitHub Projects para el seguimiento vinculado al repositorio.
- Se leyó `Text_Prompt_Maestro.txt` y se ajustó el trabajo a sus fases con aprobación entre ellas.
- Se recibió el enunciado completo del proyecto **Transporte público y mototaxismo: rutas, flota y cumplimiento**.
- Se prepararon las evidencias E1–E4 de la Fase 1: análisis semántico, catálogo conceptual, especificación Chen editable y trazabilidad conceptual.
- La Fase 1 fue **APROBADA POR EL USUARIO**. Las preguntas Q01–Q03 siguen pendientes de respuesta para precisar algunos detalles antes de cerrar el modelo lógico. No se eligió SGBD y no se generó DDL ni código Java.
- El usuario aprobó la Fase 1 y pidió un enlace de GitHub con commits organizados; se incorporó un plan de commits en `docs/plan_commits.md`.
- El GitHub conectado muestra `jjCastillok/gh-react`, un repositorio ajeno al proyecto. No se configuró ni se modificó ese repositorio.
- No hay remoto ni commits publicados. El usuario eligió crear un repositorio GitHub público y usar Jira.
- El usuario eligió crear un repositorio GitHub público y usar Jira para Scrum. Falta recibir la URL del repositorio.
- Jira está conectado. Se encontró el proyecto `SCRUM`, el tablero `SCRUM board` y tres issues genéricos preexistentes; no se modificaron.
- Se revisaron los tipos disponibles (`Epic`, `Task`, `Feature`, `Subtask`) y se preparará una propuesta de tickets para revisar antes de crearla.
- El tablero existente tiene un Sprint 0 activo y un Sprint 1 futuro. No se añadieron tareas a esos Sprints.
- Se preparó `docs/backlog_jira_propuesta.md` con un Epic y ocho tareas para revisión; no se creó ningún issue ni Sprint.

## Pendiente para la próxima sesión

1. Responder Q01–Q03 en `SCRUM-6` y actualizar análisis, catálogo, diagrama y trazabilidad.
2. Completar HU-09: vincular el repositorio en Jira, activar la regla commit→`En curso` y verificarla con un commit que lleve una clave Jira.
3. Aprobar el cierre conceptual antes de iniciar `SCRUM-7` (modelo lógico).
4. Elegir el motor solo tras aprobar el modelo lógico (`SCRUM-8`).
5. Acordar duración de próximos Sprints, integrantes y estimaciones.

## Punto de reanudación

Iniciar por [SCRUM-6](https://jjjk3022.atlassian.net/browse/SCRUM-6) en Sprint 0. Registrar las respuestas Q01–Q03 en `docs/01_analisis_semantico.md`, actualizar las evidencias conceptuales y pedir aprobación antes de trabajar el diseño lógico.
