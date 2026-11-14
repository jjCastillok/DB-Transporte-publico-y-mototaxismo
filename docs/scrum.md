# Scrum del proyecto P11

## Herramienta y enlaces

- Jira: proyecto `SCRUM`; [tablero](https://jjjk3022.atlassian.net/jira/software/c/projects/SCRUM/boards/1).
- Repositorio público: [jjCastillok/DB-Transporte-publico-y-mototaxismo](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo).
- Épico: [SCRUM-5 — P11](https://jjjk3022.atlassian.net/browse/SCRUM-5).
- Auditoría 2026-11-14: un board visible asociado al proyecto, `SCRUM board` (ID 1); columnas reales: `BD-TPM`, `En curso`, `Pruebas`, `Finalizado`. `BD-TPM` es el estado pendiente (categoría To Do), no un board adicional.
- Conexión GitHub/Jira y regla de automatización de commits siguen sin verificación efectiva; no afirmar sincronización automática activa.

## Backlog trazable

| ID | Ticket | Alcance | Estado documentado / siguiente paso |
|---|---|---|---|
| HU-01 | [SCRUM-6](https://jjjk3022.atlassian.net/browse/SCRUM-6) | Cerrar decisiones conceptuales | **Finalizado**; aclaraciones recibidas y evidencias E1–E4 actualizadas. |
| HU-02 | [SCRUM-7](https://jjjk3022.atlassian.net/browse/SCRUM-7) | Modelo lógico, integridad y demostración 3FN | **En curso; Sprint 0**. E5–E8 propuestos, pendientes revisión y aprobación. |
| HU-03 | [SCRUM-8](https://jjjk3022.atlassian.net/browse/SCRUM-8) | Elegir SGBD y diseño físico | Solo después de aprobar Fase 2. |
| HU-04 | [SCRUM-9](https://jjjk3022.atlassian.net/browse/SCRUM-9) | DDL documentado | Bloqueada por Fase 3, SGBD y aprobación. |
| HU-05 | [SCRUM-10](https://jjjk3022.atlassian.net/browse/SCRUM-10) | DML sintético y privacidad | Después de aprobar DDL. |
| HU-06 | [SCRUM-11](https://jjjk3022.atlassian.net/browse/SCRUM-11) | Consultas de operación y cobertura | Requiere modelo y datos de prueba aprobados. |
| HU-07 | [SCRUM-12](https://jjjk3022.atlassian.net/browse/SCRUM-12) | POO básica en Java | Mantener simple; polimorfismo solo con variación real. |
| HU-08 | [SCRUM-13](https://jjjk3022.atlassian.net/browse/SCRUM-13) | Auditoría y sustentación | Cierre final. |
| HU-09 | [SCRUM-14](https://jjjk3022.atlassian.net/browse/SCRUM-14) | Conexión y automatización GitHub/Jira | Jira UI debe confirmar conexión y regla de commit→En curso. |

Cada ticket usa identificador `P11-HU-nn`, describe propósito, hecho, estado, siguiente paso, criterios y dependencias. Los comentarios Jira conservan el historial de decisiones.

## Sprint y capacidad

`SCRUM Sprint 0` (ID 2) está activo del 10 al 24 nov 2026, hora Bogotá. HU-01 y HU-02 están asociadas; HU-01 está finalizada y HU-02 en curso. Se agregó HU-02 al ciclo para que el ticket represente el trabajo actual. No se cambiaron fechas ni se asignaron estimaciones.

## Uso de las columnas del board

| Columna Jira | Tickets P11 al 2026-11-14 | Criterio |
|---|---|---|
| BD-TPM | SCRUM-8–SCRUM-14 | Pendientes por dependencia. |
| En curso | SCRUM-7 | Preparación y revisión de E5–E8; no significa diseño aprobado. |
| Pruebas | Ninguno | Sin SGBD ni DDL aprobados; no hay prueba real que registrar. |
| Finalizado | SCRUM-6 | Aclaraciones y revisión conceptual completadas. |

Se usa el único board disponible de principio a fin. Sus columnas son estados de tickets; no se crearán boards duplicados para representar el flujo.

## Update del 2026-11-14

- El usuario precisó inspecciones a empresas/vehículos, permitiendo como opción secundaria inspección de servicio.
- Se usará **Sector**. Cobertura se deriva de confirmación de servicios por vehículo con evidencia tipo token/ticket (app o impreso).
- Se permite simultaneidad bajo condición de finalizando/finalizada; la regla exacta de intervalo aún requiere precisión para implementación.
- Se generaron borradores lógicos E5–E8 con análisis explícito de 1FN, 2FN y 3FN.
- Jira se auditó y actualizó. SCRUM-6 pasó a Finalizado; SCRUM-7 pasó a En curso y Sprint 0. E5–E8 siguen pendientes de aprobación.
- La columna Pruebas queda sin tickets P11 hasta que exista una implementación y evidencia real.

## Integración de commits

Convención: `SCRUM-7 docs: draft logical model`. La clave permite vincular el commit manualmente. El cambio automático de estado solo se considera activo tras conectar el repo, crear la regla y observar una prueba real. Jira UI es necesaria para configurar la automatización; la API conectada no expone la administración de reglas.

## Retomar el trabajo

1. Revisar E5–E8 en el repositorio y aprobar o ajustar pendientes.
2. Si se aprueba Fase 2, responder la pregunta obligatoria del SGBD; no crear antes SQL.
3. Registrar la decisión en bitácora, actualizar SCRUM-6/7 y conservar un commit por alcance.
4. Más adelante, retomar SCRUM-14 desde Jira UI para completar integración y regla automática.
