# Vinculación GitHub–Jira y automatización de commits

## Estado actual

- Repositorio público: [DB-Transporte-publico-y-mototaxismo](https://github.com/jjCastillok/DB-Transporte-publico-y-mototaxismo).
- Jira: proyecto `SCRUM`, tablero [SCRUM board](https://jjjk3022.atlassian.net/jira/software/c/projects/SCRUM/boards/1).
- El Epic `SCRUM-5` tiene un enlace web al repositorio.
- Jira notificó que GitHub for Atlassian está instalado. Esto no confirma que el repo esté conectado ni que Jira reciba eventos de commits.
- HU-09: [SCRUM-14](https://jjjk3022.atlassian.net/browse/SCRUM-14).
- Pendiente: conectar/autorizar el repo en Atlassian, crear y habilitar regla, y probar transición. No se afirma sincronización automática hasta verificar ejecución.

## Convención de referencias

Escribir la clave Jira en el nombre de la rama y en el mensaje del commit. Ejemplo:

```text
SCRUM-6 docs: close conceptual questions
```

Esto facilita que GitHub for Atlassian asocie la actividad al ticket.

## Regla objetivo

Nombre: `P11 — Commit referenciado → En curso`.

1. Disparador: commit nuevo recibido desde el repositorio GitHub conectado.
2. Condición: el commit referencia una clave de incidencia Jira del proyecto `SCRUM`.
3. Acción: transicionar esa incidencia a `En curso` si el flujo permite esa transición.
4. Guardar y habilitar la regla; revisar el historial/audit log tras la primera prueba.

La opción exacta de disparador depende de los eventos que muestre la instancia Jira y del estado de GitHub for Atlassian. No crear una regla basada en cualquier edición de incidencia, porque eso podría generar ciclos o iniciar incidencias no relacionadas.

## Verificación requerida

1. Confirmar que el repositorio exacto figura como conectado a Jira.
2. Guardar captura o evidencia de la regla habilitada.
3. Crear un commit de prueba que incluya una clave de issue del proyecto.
4. Confirmar que el ticket aparece en `En curso` y que la ejecución consta en el historial de automatización.
5. Registrar fecha, commit, ticket y resultado observado en `docs/bitacora.md`.

## Límite de esta sesión

La API Jira disponible aquí permite crear/consultar tickets y Sprint, pero no administrar reglas de automatización. La nueva sesión de navegador Jira solicita autenticación. No se ingresaron credenciales. Completar la configuración desde una sesión autenticada y verificar antes de cerrar HU-09.
