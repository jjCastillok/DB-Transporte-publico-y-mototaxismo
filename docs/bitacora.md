# Bitácora de trabajo

Registro cronológico para saber qué se hizo, qué falta y cómo retomar el proyecto.

## Auditoría de Jira boards y organización del flujo (2026-11-14)

- Se revisó el listado de boards visibles en el sitio Jira y los vinculados al proyecto SCRUM. Resultado: un solo board visible asociado al proyecto, `SCRUM board` (ID 1), dentro del proyecto SCRUM.
- El board tiene cuatro columnas/estados: `BD-TPM` (pendiente/To Do), `En curso`, `Pruebas` y `Finalizado`. `BD-TPM` es el estado inicial y no un board adicional.
- Se consultaron los tickets P11 y el Sprint 0 activo (ID 2). Los tickets estaban inicialmente en BD-TPM; SCRUM-6 ya estaba en el sprint.
- Se movió `SCRUM-6` / HU-01 a `Finalizado`: respuestas del usuario y actualización E1–E4 completadas.
- Se movió `SCRUM-7` / HU-02 a `En curso` y se agregó al Sprint 0: trabajo de E5–E8 iniciado, todavía propuesto y pendiente de aprobación.
- `SCRUM-8`–`SCRUM-14` permanecen en BD-TPM por dependencias. Ningún P11 está en `Pruebas`: no existe SGBD/DDL aprobado ni evidencia de prueba real.
- Se actualizaron el Epic y las descripciones/comentarios de SCRUM-6/SCRUM-7. No se tocaron tickets genéricos.
- Para retomar: abrir SCRUM-7 en En curso y revisar E5–E8. No avanzar tickets a Pruebas sin una implementación aprobada y evidencia verificable.

## Actualización — aclaraciones y diseño lógico (2026-11-14)

- El usuario confirmó que las inspecciones se realizan principalmente a vehículos y empresas; una inspección de servicio/asignación también se puede almacenar, pero es secundaria.
- Se adopta **Sector** como nombre territorial. Un sector se considera cubierto si se confirma un servicio del vehículo, con evidencia token/ticket de aplicación o ticket impreso conservado. Se propone guardar tipo de evidencia y una referencia no personal, no el token/ticket completo.
- El usuario permite simultaneidad cuando el vehículo está finalizando o ya finalizó una asignación. La frontera temporal exacta sigue abierta; no se codifica como restricción física hasta que se defina.
- Se atendió la pregunta de 3FN: es un requisito y se preparó una justificación estática con dependencias funcionales en E6. No se afirma prueba de base de datos.
- Se corrigieron E1–E4 y se redactaron E5–E8 (modelo lógico, normalización, diagrama y trazabilidad) como **PROPUESTO / REVISADO ESTÁTICAMENTE**. El usuario debe revisar y aprobar la fase lógica.
- Publicación GitHub: [SCRUM-7 docs: draft normalized logical model](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/4145c1820fede67ecce237eb9749fbb57bd55ede), con análisis, modelo lógico, evidencias, Scrum y README.
- No se eligió SGBD y no se generó DDL, DML, SQL de scripts ni código Java. La fase física/implementación no ha comenzado.
- Preguntas que podrían cambiar el modelo antes de aprobarlo: habilitación global o por ruta/vigencia; significado de estado `FINALIZANDO`; cobertura individual por paradero además de sector; activo de flota y regla de cumplimiento empresarial; referencia de evidencia sin dato personal; historia del cambio de empresa del vehículo.
- Jira se actualizó para quitar el mensaje obsoleto de “esperando Q01–Q03” y enlazar el borrador lógico.
- Próximo paso: revisar E5–E8 y responder aprobación o ajustes. Después de aprobar, la siguiente pregunta será únicamente la selección del SGBD, según `Text_Prompt_Maestro.txt`.

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
- La sincronización del plan/bitácora con ese ajuste quedó publicada en [SCRUM-5 docs: sync Sprint dates and commit history](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/f22bf6a7ff1337baf65da106c90c72281d24ddc7).
- Se ampliaron las descripciones del Epic y de `SCRUM-6`–`SCRUM-14` con identificadores P11, propósito, trabajo realizado, estado, siguiente paso y dependencias. Se agregó un comentario de actualización a cada ticket para que la próxima sesión pueda retomarse sin revisar toda la conversación.
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

1. Revisar y aprobar/ajustar la propuesta E5–E8, especialmente los pendientes S01–S08 y la regla temporal D03.
2. Actualizar Jira SCRUM-6 y SCRUM-7 con estado y comentario de esta sesión.
3. Tras aprobación lógica, elegir SGBD antes de cualquier DDL (`SCRUM-8`).
4. Completar HU-09 desde Jira UI: verificar conexión del repositorio, crear regla commit→En curso y probarla.
5. Acordar capacidad/duración del próximo Sprint antes de planificar más trabajo.

## Punto de reanudación

Abrir `docs/05_modelo_logico.md`, `docs/06_normalizacion.md`, `docs/07_diagrama_logico.md` y `docs/08_trazabilidad_logica.csv`; revisar la propuesta antes de la aprobación de Fase 2. `docs/01_analisis_semantico.md` conserva las decisiones y pendientes. No iniciar DDL antes de que el usuario apruebe el modelo lógico y elija SGBD.
