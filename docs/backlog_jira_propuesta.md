# Backlog Jira de P11

Este documento conserva la propuesta original y el mapeo real a Jira. Los tickets ya fueron creados en el proyecto `SCRUM` luego de la confirmación del usuario.

## Épico

- `SCRUM-5` — P11 · Sistema de gestión del transporte público y mototaxismo.
- [Abrir Epic en Jira](https://jjjk3022.atlassian.net/browse/SCRUM-5)
- Repositorio enlazado: https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo

## Historias de usuario

| ID | Jira | Resumen |
|---|---|---|
| HU-01 | `SCRUM-6` | Aclarar decisiones abiertas del alcance conceptual |
| HU-02 | `SCRUM-7` | Diseñar el modelo lógico relacional normalizado |
| HU-03 | `SCRUM-8` | Seleccionar SGBD y documentar el diseño físico |
| HU-04 | `SCRUM-9` | Preparar DDL básico y documentado |
| HU-05 | `SCRUM-10` | Preparar datos sintéticos y controles de privacidad |
| HU-06 | `SCRUM-11` | Documentar consultas de operación y cobertura |
| HU-07 | `SCRUM-12` | Modelar conceptos seleccionados en Java con POO básica |
| HU-08 | `SCRUM-13` | Auditar trazabilidad y preparar entrega final |
| HU-09 | `SCRUM-14` | Vincular commits GitHub con incidencias Jira |

Los títulos de Jira incluyen el identificador `P11 HU-nn`; las descripciones contienen formato de historia, criterios de aceptación y dependencias. Cada historia tiene comentario inicial. El proyecto Jira no ofrece el tipo `Story`, por lo que se utilizó `Feature` para representar historias de usuario.

## Sprint activo

`SCRUM Sprint 0` (ID 2) ya estaba activo cuando se confirmó el trabajo. Se agregó `SCRUM-6` para cerrar decisiones pendientes; no se reinició el ciclo, no se ajustaron fechas y ninguna tarea fue cambiada a `En curso` antes de tener trabajo iniciado.

## Integración GitHub / Jira

El repositorio es público y se agregó como enlace web al Epic. Jira muestra instalado GitHub for Atlassian, pero aún no se ha verificado que el repositorio esté conectado al proyecto ni que exista la automatización de commits. `SCRUM-14` queda en backlog para completar esa configuración y comprobarla con una clave Jira en un mensaje de commit.

Convención prevista: `SCRUM-6 docs: close conceptual questions`. La clave del issue permite rastreo; el cambio automático de estado solo se afirmará después de activar y probar la regla.
