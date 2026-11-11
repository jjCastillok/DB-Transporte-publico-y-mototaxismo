# Transporte público y mototaxismo: rutas, flota y cumplimiento

## Propósito

Este repositorio organizará el análisis, diseño e implementación de una base de datos para el proyecto **Transporte público y mototaxismo: rutas, flota y cumplimiento**. El código y los documentos se construirán paso a paso, con soluciones sencillas que puedan explicarse en la sustentación.

## Punto de partida

1. Revisar el enunciado completo del problema de transporte y confirmar su alcance.
2. Separar hechos, inferencias, supuestos y dudas.
3. Proponer el modelo conceptual y solicitar aprobación antes de continuar.
4. Traducir el modelo aprobado a diseño lógico y solicitar su aprobación.
5. Elegir el motor, elaborar el diseño físico y continuar con las fases del Prompt Maestro.

## Referencias de trabajo

La guía del curso y el ejemplo del caso P01 se conservan localmente como referencias; el ejemplo no define requisitos del proyecto de transporte. No se incluyen en este repositorio público.

## Documentos del proyecto

- [Análisis inicial](docs/analisis-inicial.md)
- [Scrum y backlog](docs/scrum.md)
- [Bitácora de trabajo](docs/bitacora.md)
- [Análisis semántico — Fase 1](docs/01_analisis_semantico.md)
- [Catálogo conceptual — Fase 1](docs/02_catalogo_conceptual.md)
- [Diagrama conceptual Chen — texto](docs/03_diagrama_conceptual.md)
- [Trazabilidad conceptual — CSV](docs/04_trazabilidad_conceptual.csv)
- [Diagrama Chen editable](docs/03_diagrama_conceptual.drawio)
- [Plan de commits](docs/plan_commits.md)
- [Propuesta de backlog Jira](docs/backlog_jira_propuesta.md)
- [Prompt Maestro](Text_Prompt_Maestro.txt)

## Decisiones por confirmar

- Respuestas a Q01–Q03 para cerrar decisiones conceptuales pendientes.
- Motor y versión del sistema gestor.
- Integrantes, duración de Sprint y fechas de entrega.
- Confirmar acceso del conector GitHub for Atlassian al repositorio e implementar la regla de automatización en Jira.

## Preferencias acordadas

- Java para POO si el alcance requiere código de aplicación.
- Markdown durante el trabajo y Word al final.
- Scrum en Jira, en el tablero existente `SCRUM board` del proyecto `SCRUM`; épico e historias P11 ya creados.
- Repositorio GitHub público: [DB-Transporte-publico-y-mototaxismo](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo).
- Sprint 0 ya activo; solo HU-01 se añadió para atender primero las decisiones abiertas.

## Criterio de implementación

Se preferirán nombres claros, clases y métodos pequeños, SQL legible y funciones que se puedan explicar. Se incorporará polimorfismo donde resuelva una variación real del problema, sin añadir capas ni patrones innecesarios. No se escribirá código definitivo antes de revisar el enunciado y aprobar el diseño correspondiente.
