# Scrum del proyecto P11

## Herramienta y enlaces

- Jira: proyecto `SCRUM`; [tablero](https://jjjk3022.atlassian.net/jira/software/c/projects/SCRUM/boards/1).
- Repositorio público: [jjCastillok/DB-Transporte-publico-y-mototaxismo](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo).
- Épico: [SCRUM-5 — P11](https://jjjk3022.atlassian.net/browse/SCRUM-5).
- Estados del tablero observados: Idea, En curso, Pruebas, Finalizado.
- Conexión GitHub/Jira y regla de automatización de commits siguen sin verificación efectiva; no afirmar sincronización automática activa.

## Backlog trazable

| ID | Ticket | Alcance | Estado documentado / siguiente paso |
|---|---|---|---|
| HU-01 | [SCRUM-6](https://jjjk3022.atlassian.net/browse/SCRUM-6) | Cerrar decisiones conceptuales | Respuestas Q01–Q03 recibidas; evidencias revisadas; pedir visto bueno conceptual/lógico. Jira aún muestra Idea hasta actualizarlo. |
| HU-02 | [SCRUM-7](https://jjjk3022.atlassian.net/browse/SCRUM-7) | Modelo lógico, integridad y demostración 3FN | E5–E8 propuestos; pendientes decisiones de frontera y aprobación. |
| HU-03 | [SCRUM-8](https://jjjk3022.atlassian.net/browse/SCRUM-8) | Elegir SGBD y diseño físico | Solo después de aprobar Fase 2. |
| HU-04 | [SCRUM-9](https://jjjk3022.atlassian.net/browse/SCRUM-9) | DDL documentado | Bloqueada por Fase 3, SGBD y aprobación. |
| HU-05 | [SCRUM-10](https://jjjk3022.atlassian.net/browse/SCRUM-10) | DML sintético y privacidad | Después de aprobar DDL. |
| HU-06 | [SCRUM-11](https://jjjk3022.atlassian.net/browse/SCRUM-11) | Consultas de operación y cobertura | Requiere modelo y datos de prueba aprobados. |
| HU-07 | [SCRUM-12](https://jjjk3022.atlassian.net/browse/SCRUM-12) | POO básica en Java | Mantener simple; polimorfismo solo con variación real. |
| HU-08 | [SCRUM-13](https://jjjk3022.atlassian.net/browse/SCRUM-13) | Auditoría y sustentación | Cierre final. |
| HU-09 | [SCRUM-14](https://jjjk3022.atlassian.net/browse/SCRUM-14) | Conexión y automatización GitHub/Jira | Jira UI debe confirmar conexión y regla de commit→En curso. |

Cada ticket usa identificador `P11-HU-nn`, describe propósito, hecho, estado, siguiente paso, criterios y dependencias. Los comentarios Jira conservan el historial de decisiones.

## Sprint y capacidad

`SCRUM Sprint 0` (ID 2) fue observado activo con fechas 10–24 nov 2026, hora Bogotá. HU-01 ya estaba asociada. No se cambian sprint ni estimaciones sin conocer la disponibilidad del equipo. La configuración real del tablero y su estado actual deben volver a verificarse al iniciar trabajo.

## Update del 2026-11-14

- El usuario precisó inspecciones a empresas/vehículos, permitiendo como opción secundaria inspección de servicio.
- Se usará **Sector**. Cobertura se deriva de confirmación de servicios por vehículo con evidencia tipo token/ticket (app o impreso).
- Se permite simultaneidad bajo condición de finalizando/finalizada; la regla exacta de intervalo aún requiere precisión para implementación.
- Se generaron borradores lógicos E5–E8 con análisis explícito de 1FN, 2FN y 3FN.
- Jira debe actualizarse con las respuestas y enlazarse a este estado; no se movieron HU-02 al sprint activo ni a En curso porque falta la aprobación de fase y no se conoce capacidad.

## Integración de commits

Convención: `SCRUM-7 docs: draft logical model`. La clave permite vincular el commit manualmente. El cambio automático de estado solo se considera activo tras conectar el repo, crear la regla y observar una prueba real. Jira UI es necesaria para configurar la automatización; la API conectada no expone la administración de reglas.

## Retomar el trabajo

1. Revisar E5–E8 en el repositorio y aprobar o ajustar pendientes.
2. Si se aprueba Fase 2, responder la pregunta obligatoria del SGBD; no crear antes SQL.
3. Registrar la decisión en bitácora, actualizar SCRUM-6/7 y conservar un commit por alcance.
4. Más adelante, retomar SCRUM-14 desde Jira UI para completar integración y regla automática.
