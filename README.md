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
- [Diccionario físico MariaDB — E9](docs/09_diccionario_fisico.csv)
- [Especificación física MariaDB — E10](docs/10_especificacion_fisica.md)
- [Verificación previa MariaDB — E11](docs/11_verificacion_previa.md)
- [Gestión del código SQL MariaDB](docs/gestion_codigo_mariadb.md)
- [Solicitud P11-CR-01 — tickets/tokens de entrada/salida](docs/solicitud_cambio_01_tickets_tokens.md)
- [Plan de commits](docs/plan_commits.md)
- [Propuesta de backlog Jira](docs/backlog_jira_propuesta.md)
- [Vinculación y automatización Jira–GitHub](docs/automatizacion_jira.md)
- [Prompt Maestro](Text_Prompt_Maestro.txt)

## Decisiones y pendientes

- La Fase 2 lógica fue aprobada por el usuario el 14 nov 2026.
- MariaDB fue elegido; la versión instalada no se ha cotejado. MariaDB 11.8.9 LTS es objetivo propuesto, no una versión confirmada en el equipo.
- Revisar y aprobar la Fase 3 física E9–E11, incluidos pendientes de estados, terminales, accesibilidad, referencias protegidas y concurrencia.
- Precisar P11-CR-01: qué representan entrada/salida del vehículo, para adaptar E5–E11 antes de DDL.
- Integrantes, duración de Sprint y fechas de entrega.
- Jira queda en pausa por decisión del usuario; sus referencias conservan el último estado observado y no representan cambios posteriores.

## Preferencias acordadas

- Java para POO si el alcance requiere código de aplicación.
- Markdown durante el trabajo y Word al final.
- Scrum documentado en `docs/scrum.md` y commits de GitHub. Jira se conserva como historial y queda en pausa por decisión del usuario.
- Repositorio GitHub público: [DB-Transporte-publico-y-mototaxismo](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo).
- Último estado Jira observado: Sprint 0 con HU-01 Finalizada y HU-02 En curso. No se actualizaron tickets después de la decisión de pausar Jira; ver [Scrum](docs/scrum.md).

## Estado del diseño

Las decisiones sobre inspecciones, cobertura por Sector y simultaneidad fueron recibidas el 14 nov 2026. La Fase 2 lógica fue aprobada; la revisión estática de 3FN se documenta en E6. La Fase 3 se prepara para MariaDB. Todavía no se han generado scripts SQL ni código Java.

## Criterio de implementación

Se preferirán nombres claros, clases y métodos pequeños, SQL legible y funciones que se puedan explicar. Se incorporará polimorfismo donde resuelva una variación real del problema, sin añadir capas ni patrones innecesarios. No se escribirá código definitivo antes de revisar el enunciado y aprobar el diseño correspondiente.

