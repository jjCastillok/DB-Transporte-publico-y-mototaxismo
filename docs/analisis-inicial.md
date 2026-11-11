# Análisis inicial

## Contexto del proyecto

**Título:** Transporte público y mototaxismo: rutas, flota y cumplimiento.

La guía del curso pide un proyecto de bases de datos que cubra análisis de requisitos, modelo entidad-relación, modelo relacional, normalización, implementación SQL y documentación técnica. Señala 3FN como requisito base y contempla vistas, procedimientos almacenados, triggers, una función definida por el usuario, índices y consultas analíticas.

El documento de ejemplo propone una estructura de cinco fases: especificación y modelo conceptual; diseño lógico y normalización; implementación física; lógica avanzada y optimización; sustentación y memoria técnica. Sus requisitos detallados describen el caso P01, Parque Tayrona. Se usarán solo como referencia de formato, no como reglas del proyecto de transporte.

`Text_Prompt_Maestro.txt` define el proceso de trabajo: partir del enunciado, distinguir hechos e inferencias, revisar el análisis semántico, avanzar por fases con aprobación y no afirmar pruebas que no se hayan ejecutado.

## Enunciado del problema

**Estado:** recibido del usuario el 10 de noviembre de 2026; se analiza en `01_analisis_semantico.md`.

El enunciado confirma actores, procesos, información requerida, reglas de negocio y consultas principales. Deja sin precisar el objeto de inspección, la definición de zona/sector y cobertura, el alcance de la regla temporal, el denominador de accesibilidad y los detalles de las fuentes de contexto. Esos vacíos se registran sin inventar respuestas.

## Preferencias del usuario

- Java para POO si se requiere código de aplicación; mantenerlo básico.
- Markdown como formato de trabajo y Word al final.

## Método de análisis

Cada hallazgo se marcará como **HECHO CONFIRMADO**, **INFERENCIA**, **SUPUESTO POR VALIDAR**, **DUDA** o **PROPUESTA** y se asociará a su fuente. Los requisitos y reglas llevarán identificadores para mantener la trazabilidad durante el diseño y las pruebas.

## Requisitos generales extraídos de la guía

- Motor relacional: PostgreSQL, MySQL o SQL Server según la guía general; confirmar la selección del equipo.
- Modelo normalizado al menos hasta 3FN.
- La guía general indica de 8 a 15 tablas, con relaciones 1:N y N:M resueltas mediante tablas intermedias. Las entidades deberán justificarse por el dominio; no se añadirán tablas artificiales para completar una cantidad.
- Definir claves, restricciones e integridad referencial justificadas.
- La guía general pide al menos 3 vistas, 2 procedimientos almacenados, 2 triggers y 1 función definida por el usuario.
- Preparar consultas y documentar optimización e índices.
- Elaborar memoria técnica con reglas, modelos, normalización, diccionario de datos, consultas y conclusiones.

Los requisitos explícitos de la guía se mantendrán trazables. Los detalles propios del caso P01 no se trasladarán al problema de transporte. Cualquier tensión entre los mínimos del curso y el modelo que resulte natural se documentará para revisión, sin inventar entidades.

## Decisiones abiertas

| Tema | Estado | Próxima decisión |
|---|---|---|
| Enunciado y ejemplos | Recibido | Revisar análisis semántico y confirmar fuentes citadas |
| Motor de base de datos | Pendiente | Elegir uno compatible con la guía y el curso |
| POO y polimorfismo | Preferencia registrada | Java si se requiere código; incorporar polimorfismo solo si resuelve una variación validada |
| Alcance funcional | En revisión | Validar propuestas y preguntas de `01_analisis_semantico.md` |
| Repositorio GitHub | Creado y público | `jjCastillok/DB-Transporte-publico-y-mototaxismo`; integración Jira pendiente de verificación |
| Scrum | Configurado | Jira, proyecto `SCRUM`; Epic `SCRUM-5`, nueve historias `SCRUM-6`–`SCRUM-14`; HU-01 añadida al Sprint 0 activo |
