# Scrum del proyecto P11

## Herramienta y enlaces

- Jira: proyecto `SCRUM`; [tablero](https://jjjk3022.atlassian.net/jira/software/c/projects/SCRUM/boards/1).
- Repositorio público: [jjCastillok/DB-Transporte-publico-y-mototaxismo](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo).
- Épico: [SCRUM-5 — P11](https://jjjk3022.atlassian.net/browse/SCRUM-5).
- Auditoría 2026-11-14: un board visible asociado al proyecto, `SCRUM board` (ID 1); columnas reales: `BD-TPM`, `En curso`, `Pruebas`, `Finalizado`. `BD-TPM` es el estado pendiente (categoría To Do), no un board adicional.
- Conexión GitHub/Jira y regla de automatización de commits siguen sin verificación efectiva; no afirmar sincronización automática activa.
- Decisión del usuario 2026-11-14: Jira queda en pausa y no se harán más cambios en tickets/tableros por ahora. La información previa se conserva como historial; el seguimiento activo se lleva en los documentos y commits del repositorio.
- Solicitud nueva P11-CR-01: gestión de tickets/tokens para entrada y salida de vehículos y guía del código SQL MariaDB. La definición del evento está pendiente; no se creó SQL ni credenciales.

## Backlog trazable

| ID | Ticket | Alcance | Estado documentado / siguiente paso |
|---|---|---|---|
| HU-01 | [SCRUM-6](https://jjjk3022.atlassian.net/browse/SCRUM-6) | Cerrar decisiones conceptuales | **Finalizado**; aclaraciones recibidas y evidencias E1–E4 actualizadas. |
| HU-02 | [SCRUM-7](https://jjjk3022.atlassian.net/browse/SCRUM-7) | Modelo lógico, integridad y demostración 3FN | **Fase 2 aprobada por el usuario el 2026-11-14**. Jira conserva su último estado observado; no se actualizó. |
| HU-03 | [SCRUM-8](https://jjjk3022.atlassian.net/browse/SCRUM-8) | Elegir SGBD y diseño físico | MariaDB elegido; E9–E11 en propuesta. Versión instalada pendiente de cotejo y aprobación de Fase 3. |
| HU-04 | [SCRUM-9](https://jjjk3022.atlassian.net/browse/SCRUM-9) | DDL documentado | Fase 3 y DDL pendientes de aprobación; el cambio P11-CR-01 debe incorporarse antes de congelar el diseño. |
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
| En curso | SCRUM-7 (último estado de Jira observado) | Fase 2 aprobada; diseño físico E9–E11 en elaboración fuera de Jira. |
| Pruebas | Ninguno | Sin SGBD ni DDL aprobados; no hay prueba real que registrar. |
| Finalizado | SCRUM-6 | Aclaraciones y revisión conceptual completadas. |

Se usa el único board disponible de principio a fin. Sus columnas son estados de tickets; no se crearán boards duplicados para representar el flujo.

## Update del 2026-11-14

- El usuario precisó inspecciones a empresas/vehículos, permitiendo como opción secundaria inspección de servicio.
- Se usará **Sector**. Cobertura se deriva de confirmación de servicios por vehículo con evidencia tipo token/ticket (app o impreso).
- Se permite simultaneidad bajo condición de finalizando/finalizada; la regla exacta de intervalo aún requiere precisión para implementación.
- Se generaron borradores lógicos E5–E8 con análisis explícito de 1FN, 2FN y 3FN.
- La Fase 2 fue aprobada por el usuario. E5–E8 se publicaron en commits individuales; el usuario eligió MariaDB para Fase 3.
- Jira queda en pausa por decisión del usuario. El último estado observado fue SCRUM-6 Finalizado y SCRUM-7 En curso/Sprint 0; no se modificó ni se presume sincronización automática.
- La columna Pruebas queda sin tickets P11 hasta que exista una implementación y evidencia real.

## Integración de commits

Convención: `SCRUM-7 docs: draft logical model`. La clave permite vincular el commit manualmente. El cambio automático de estado solo se considera activo tras conectar el repo, crear la regla y observar una prueba real. Jira UI es necesaria para configurar la automatización; la API conectada no expone la administración de reglas.

## Retomar el trabajo

1. Revisar E9–E11 y aprobar o ajustar el diseño físico propuesto para MariaDB.
2. Antes del DDL, cotejar la versión real con `SELECT VERSION();` y confirmar que el esquema destino esté vacío.
3. Solo tras aprobar Fase 3, preparar DDL documentado, inventario documental, migración de comentarios pendiente de metadatos reales y reporte de mejoras no aplicadas.
4. Jira queda pausado; al reanudarlo, actualizar sus tickets desde el último estado documentado.

