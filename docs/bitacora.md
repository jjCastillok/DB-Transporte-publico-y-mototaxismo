# Bitácora de trabajo

Registro cronológico para saber qué se hizo, qué falta y cómo retomar el proyecto.

## Solicitud P11-CR-01 · tickets/tokens de acceso vehicular (2026-11-14)

- El usuario pidió gestionar tickets y tokens para registrar entrada/salida de vehículos y documentar la gestión del código SQL para MariaDB/Workbench.
- Se registró la solicitud como cambio de alcance porque E5 solo modelaba confirmaciones genéricas de servicio por Sector y no definía eventos explícitos de entrada/salida.
- Se creó `docs/solicitud_cambio_01_tickets_tokens.md` con proceso propuesto, alternativas, privacidad y decisiones pendientes; `docs/gestion_codigo_mariadb.md` documenta estructura y manejo futuro de SQL.
- Se preguntó si entrada/salida significa inicio y fin de asignación, control en terminal/patio o ambos. La definición queda pendiente; el modelo y el DDL final no se modifican hasta incorporar esa respuesta y aprobación.
- No se emitieron tickets ni tokens reales, no se guardó secreto y no se generó DDL, trigger ni procedimiento.

## Aprobación de Fase 2 y preparación de Fase 3 (2026-11-14)

- El usuario aprobó la Fase 2 y eligió **MariaDB**. Mencionó que dispone de varios entornos de desarrollo, así que no se fija Workbench como herramienta exclusiva.
- Se revisó la puerta de control del Prompt Maestro: no producir el DDL hasta cerrar y aprobar el diseño físico E9–E11.
- Se preparan E9 (diccionario de tipos y columnas), E10 (especificación física) y E11 (verificación previa). La versión instalada no se conoce; MariaDB 11.8.9 LTS queda como objetivo recomendado, no como instalación observada.
- Los artefactos y estados fueron publicados con commits separados: [E5 aprobado](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/c5b7ac80e60b2e2352c7cf15674b30d460a275cf), [E6 3FN](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/64dd28780f22e88ab0f49cb7387876c1c0749b14), [E7](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/8de185057e8d774053df5a6a8173295242defdea), [E9](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/48bffd58c0167cad02028cd4a01dac2929de158a), [E10](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/d21520d131a73d05a611057e104e8422461e62f1) y [E11](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/12fef86ee66df948611885cb9281e224d93259a3).
- Se consultó documentación oficial de MariaDB para restricciones, collation, fechas, comentarios y política LTS; se consultó el manual de MySQL Workbench para dejar clara la limitación de compatibilidad de cliente.
- Se escogieron propuestas físicas conservadoras: InnoDB, `BIGINT UNSIGNED` en identificadores técnicos, `utf8mb4`, hora local de Bogotá en `DATETIME`, FKs restrictivas para conservar historial y sin valores por defecto de estado pendientes.
- No se creó esquema, tabla, trigger, DDL, DML ni código Java y no se ejecutó SQL en servidor. El estado es **PROPUESTO / REVISADO ESTÁTICAMENTE**, no **PROBADO EN SGBD**.
- Jira queda en pausa por decisión del usuario; esta bitácora y los commits del repositorio continúan como seguimiento activo.
- Retomar leyendo E9–E11 y revisando los dominios/decisiones pendientes antes de aprobar Fase 3. Tras esa aprobación comienza la entrega DDL documentada; no cargar datos aún.

## Retoma de Fase 2 y publicación de E5–E8 (2026-11-14)

- El usuario decidió dejar Jira en pausa y continuar con la fase lógica; no se modificaron boards ni tickets en esta sesión.
- Se revisaron los artefactos E5–E8: modelo relacional, justificación estática de 1FN–3FN, diagrama lógico y trazabilidad de requisitos.
- Se confirmó que E8 contiene 18 registros de trazabilidad, que el diagrama conceptual editable conserva XML válido y que no quedan nombres de relaciones de inspección obsoletos en E5–E8.
- Se publicaron los cambios de fase lógica en cuatro commits pequeños: [E5](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/ddc4335d1a87d0be47c7d1cd509edbee0951a019), [E6](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/b819435d224fc2542a9000e1a8bc00b8bd8ae5a9), [E7](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/70e1193dc3bb80a86b9de6e3164030ae3645dafa) y [E8](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/cd79122e8da70d245d525b3a7f900b9fb07c1a53).
- La 3FN se revisó de forma lógica y estática; no existe todavía una implementación ni una prueba contra SGBD. No se generaron DDL, SQL ni código Java.
- Fase 2 sigue como propuesta: falta la revisión y aprobación del usuario y cerrar las decisiones abiertas listadas en E5/E6. Después de aprobar, se preguntará por el SGBD antes de pasar al diseño físico.
- Punto de reanudación: revisar `docs/05_modelo_logico.md` y `docs/06_normalizacion.md`; responder los pendientes del bloque “Pendientes que afectan aprobación” y revisar E7/E8 como soporte visual y trazable.

## Auditoría de Jira boards y organización del flujo (2026-11-14)

- Publicación de la organización Jira y del historial: [commit a4c926d](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo/commit/a4c926df13ba713cbc3ded2ab00b1f1edcff8c10).
- Se revisó el listado de boards visibles en el sitio Jira y los vinculados al proyecto SCRUM. Resultado: un solo board visible asociado al proyecto, `SCRUM board` (ID 1), dentro del proyecto SCRUM.
- El board tiene cuatro columnas/estados: `BD-TPM` (pendiente/To Do), `En curso`, `Pruebas` y `Finalizado`. `BD-TPM` es el estado inicial y no un board adicional.
- Se consultaron los tickets P11 y el Sprint 0 activo (ID 2). Los tickets estaban inicialmente en BD-TPM; SCRUM-6 ya estaba en el sprint.
- Se movió `SCRUM-6` / HU-01 a `Finalizado`: respuestas del usuario y actualización E1–E4 completadas.
- Se movió `SCRUM-7` / HU-02 a `En curso` y se agregó al Sprint 0: trabajo de E5–E8 iniciado, todavía propuesto y pendiente de aprobación.
- `SCRUM-8`–`SCRUM-14` permanecen en BD-TPM por dependencias. Ningún P11 está en `Pruebas`: no existe SGBD/DDL aprobado ni evidencia de prueba real.
- Se actualizaron el Epic y las descripciones/comentarios de SCRUM-6/SCRUM-7. No se tocaron tickets genéricos.
- Para retomar: abrir SCRUM-7 en En curso y revisar E5–E8. No avanzar tickets a Pruebas sin una implementación aprobada y evidencia verificable.

## Diseño lógico E5–E8 · revisión 0.2 (2026-11-14)

- Se hizo explícita la relación `PasoRuta.id_punto → ParaderoHabilitado.id_punto`, para que el diseño lógico represente la regla de que los paraderos programados estén autorizados, bajo el supuesto de habilitación global vigente.
- Se actualizó el análisis de dependencias funcionales y el diagrama lógico; E3 textual y E4 conceptual trazan ahora la relación con autorización.
- La nueva definición sigue **PROPUESTA / REVISADA ESTÁTICAMENTE**. Falta confirmar autorización global o por ruta y su vigencia, si terminales pueden ser pasos de ruta, cobertura por paradero, granularidad de inspección y semántica de solapamiento.
- No se eligió SGBD y no se generó SQL/DDL ni código. Tampoco se afirma ejecución en servidor.
- Próximo paso: revisar E5–E8 v0.2, ajustar los supuestos indicados y aprobar Fase 2. Después de aprobarla, corresponderá preguntar el SGBD.

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

1. Revisar y aprobar/ajustar E9–E11 para MariaDB; leer los pendientes de tipos y dominios físicos.
2. Responder P11-CR-01-Q1 para definir si los eventos corresponden a servicio, terminal/patio o ambos; luego cerrar Q2/Q3.
3. Antes de escribir DDL, cotejar versión del servidor (`SELECT VERSION()`), disponibilidad de InnoDB y collation; no compartir credenciales.
4. Tras aprobación de Fase 3, preparar DDL documentado y los artefactos de inventario, migración exclusivamente documental y mejoras no aplicadas.
5. No generar datos sintéticos hasta que el DDL propuesto y documentado haya recibido su puerta de aprobación.
6. Jira está en pausa por decisión del usuario; no requiere acción para continuar el proyecto.

## Punto de reanudación

Abrir `docs/09_diccionario_fisico.csv`, `docs/10_especificacion_fisica.md` y `docs/11_verificacion_previa.md`; Fase 2 ya fue aprobada y MariaDB ya fue elegido. Revisar/aprobar Fase 3 y cotejar la versión instalada antes de DDL. `docs/05_modelo_logico.md`–`docs/08_trazabilidad_logica.csv` conservan el modelo aprobado y sus pendientes de implementación.

