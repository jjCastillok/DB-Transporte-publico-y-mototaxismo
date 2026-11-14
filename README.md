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
- [Modelo lógico relacional — E5](docs/05_modelo_logico.md)
- [Normalización hasta 3FN — E6](docs/06_normalizacion.md)
- [Diagrama lógico — E7](docs/07_diagrama_logico.md)
- [Trazabilidad lógica — E8](docs/08_trazabilidad_logica.csv)
- [Plan de commits](docs/plan_commits.md)
- [Propuesta de backlog Jira](docs/backlog_jira_propuesta.md)
- [Vinculación y automatización Jira–GitHub](docs/automatizacion_jira.md)
- [Prompt Maestro](Text_Prompt_Maestro.txt)

## Decisiones por confirmar

- Revisión y aprobación del diseño lógico E5–E8, incluidos los supuestos temporales y de habilitación aún listados en `docs/01_analisis_semantico.md`.
- Motor y versión del sistema gestor.
- Integrantes, duración de Sprint y fechas de entrega.
- Confirmar acceso del conector GitHub for Atlassian al repositorio e implementar la regla de automatización en Jira.

## Preferencias acordadas

- Java para POO si el alcance requiere código de aplicación.
- Markdown durante el trabajo y Word al final.
- Scrum en Jira, en el tablero existente `SCRUM board` del proyecto `SCRUM`; épico e historias P11 ya creados.
- Repositorio GitHub público: [DB-Transporte-publico-y-mototaxismo](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo).
- Sprint 0 activo: HU-01 está Finalizada y HU-02 está En curso. Jira tiene un board visible con columnas BD-TPM, En curso, Pruebas y Finalizado; ver [Scrum](docs/scrum.md).

## Estado del diseño

Las decisiones sobre inspecciones, cobertura por Sector y simultaneidad fueron recibidas el 14 nov 2026. La revisión de 3FN estática se documenta en E6. El modelo lógico está propuesto y aún requiere aprobación; no se ha elegido SGBD y no se han generado scripts SQL ni código Java.

## Criterio de implementación

Se preferirán nombres claros, clases y métodos pequeños, SQL legible y funciones que se puedan explicar. Se incorporará polimorfismo donde resuelva una variación real del problema, sin añadir capas ni patrones innecesarios. No se escribirá código definitivo antes de revisar el enunciado y aprobar el diseño correspondiente.
