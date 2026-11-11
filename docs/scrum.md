# Scrum del proyecto

## Herramienta y enlaces

- Herramienta: Jira.
- Proyecto: `SCRUM` — Administración de mis proyectos.
- Tablero: [SCRUM board](https://jjjk3022.atlassian.net/jira/software/c/projects/SCRUM/boards/1).
- Repositorio: [jjCastillok/DB-Transporte-publico-y-mototaxismo](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo).
- Columnas del tablero: `Idea`, `En curso`, `Pruebas`, `Finalizado`.
- No se modificaron tickets genéricos preexistentes.

## Épico e historias

Épico P11: [SCRUM-5](https://jjjk3022.atlassian.net/browse/SCRUM-5). Enlaza el repositorio.

| ID | Jira | Historia / resultado | Estado / ubicación |
|---|---|---|---|
| HU-01 | [SCRUM-6](https://jjjk3022.atlassian.net/browse/SCRUM-6) | Aclarar decisiones abiertas del alcance conceptual (Q01–Q03). | Sprint 0 activo; Idea |
| HU-02 | [SCRUM-7](https://jjjk3022.atlassian.net/browse/SCRUM-7) | Diseñar el modelo lógico relacional normalizado. | Backlog; depende de aprobación conceptual |
| HU-03 | [SCRUM-8](https://jjjk3022.atlassian.net/browse/SCRUM-8) | Seleccionar SGBD y documentar diseño físico. | Backlog; depende de aprobación lógica |
| HU-04 | [SCRUM-9](https://jjjk3022.atlassian.net/browse/SCRUM-9) | Preparar DDL básico y documentado. | Backlog; depende del diseño físico |
| HU-05 | [SCRUM-10](https://jjjk3022.atlassian.net/browse/SCRUM-10) | Preparar datos sintéticos y controles de privacidad. | Backlog; depende del DDL aprobado |
| HU-06 | [SCRUM-11](https://jjjk3022.atlassian.net/browse/SCRUM-11) | Documentar consultas de operación y cobertura. | Backlog; requiere esquema y datos sintéticos |
| HU-07 | [SCRUM-12](https://jjjk3022.atlassian.net/browse/SCRUM-12) | Modelar conceptos seleccionados en Java con POO básica. | Backlog; confirmar alcance académico |
| HU-08 | [SCRUM-13](https://jjjk3022.atlassian.net/browse/SCRUM-13) | Auditar trazabilidad y preparar entrega final. | Backlog |
| HU-09 | [SCRUM-14](https://jjjk3022.atlassian.net/browse/SCRUM-14) | Vincular commits GitHub con incidencias Jira. | Backlog; conexión/regla por completar |

Cada historia tiene un resumen identificable (`P11 HU-nn`), criterios de aceptación y comentario inicial de seguimiento.

## Sprint activo

`SCRUM Sprint 0` (ID 2) ya estaba activo al iniciar esta gestión, con periodo 11–25 nov 2026 según Jira. No se reinició ni se alteraron fechas. Se añadió únicamente `SCRUM-6`, para trabajar las preguntas pendientes primero. No se marcó `En curso` porque aún no hay respuestas del usuario.

**Objetivo operativo:** cerrar las decisiones conceptuales antes de iniciar el modelo lógico. Las demás historias quedan en backlog por depender de aprobaciones o entregables previos.

## Automatización y commits

- Convención de commit: referenciar una clave, por ejemplo `SCRUM-6 docs: close conceptual questions`.
- Jira muestra GitHub for Atlassian instalado; la instalación no demuestra por sí sola que este repositorio ya esté conectado.
- Falta verificar la conexión al repositorio y crear/probar una regla que, al detectar un commit con clave `SCRUM-n`, cambie la incidencia a `En curso`.
- La API Jira conectada no expone gestión de reglas de automatización. HU-09 registra esta tarea pendiente de completar en Jira UI.

## Acuerdos para continuar

- Respetar las fases y puertas de aprobación de `Text_Prompt_Maestro.txt`.
- Mantener nombres, clases y métodos sencillos; introducir polimorfismo solo si aporta variación real.
- Registrar decisiones y cambios en `docs/bitacora.md`.
- No asumir estimaciones, asignaciones, duración futura de Sprint ni respuestas a Q01–Q03.
