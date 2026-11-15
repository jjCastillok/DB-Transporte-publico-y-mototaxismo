# Solicitud de cambio P11-CR-01 · Tickets y tokens de entrada/salida

**Fecha:** 2026-11-14  
**Solicitó:** usuario del proyecto  
**Versión:** 0.3 — eventos y lugares de control definidos, 2026-11-14  
**Estado:** PROPUESTO; eventos/lugares confirmados, modalidad de emisión pendiente.  
**Alcance:** solo control de vehículos y servicios del proyecto de transporte.

## Solicitud recibida

“Hazme los Tickets y tokens para que los vehículos entren y salgan” y entrega un documento sobre la gestión del código para MariaDB/Workbench.

## Por qué se registra como cambio

El modelo aprobado ya conserva confirmaciones de servicio basadas en token o ticket, pero todavía no define un ciclo de acceso con eventos explícitos de entrada y salida. La ubicación y significado de esos eventos cambian las relaciones y reglas de unicidad; no se debe crear SQL antes de confirmar esa semántica y aprobar el diseño físico.

## Propuesta de proceso, pendiente de validación

1. Asociar la credencial con una asignación de vehículo, no con el conductor ni una persona.
2. Aceptar como tipo de evidencia token de aplicación, ticket de aplicación o ticket impreso.
3. Registrar cada evento de entrada/salida con tipo, hora y referencia al punto o lugar operativo, sujeto a que el usuario defina qué representa “entrar” y “salir”.
4. No guardar el token secreto ni el contenido íntegro del ticket. Para validar un token, conservar un resumen criptográfico (hash) y mostrar el secreto solo en el momento de emisión; para ticket impreso, conservar una referencia no personal.
5. Mantener eventos históricos y rechazar credenciales inexistentes, expiradas o incompatibles con la asignación mediante reglas aprobadas.

## Alternativas de modelado

### A. Credencial y eventos separados — propuesta preferida

- `CredencialServicio`: identidad, asignación, tipo de credencial, referencia/hash, vigencia y estado.
- `EventoAccesoServicio`: eventos individuales de ENTRADA y SALIDA asociados a la credencial/asignación y al lugar de control.

Ventaja: cada token/ticket se emite una vez y se pueden conservar varias acciones sin repetir datos de credencial. Requiere acordar cuántos eventos se permiten por asignación y qué punto deben registrar.

### Alcance confirmado por el usuario

La solución debe admitir **ambos grupos de eventos**:

1. Inicio y fin de la asignación/servicio del vehículo.
2. Entrada y salida de un lugar operativo.

Los lugares de control serán **cualquier punto habilitado que forme parte de la ruta**. El evento de entrada/salida debe enlazarse a un paso concreto de esa ruta, no a un punto ajeno a la asignación. La confirmación de servicio por Sector sigue siendo distinta: una entrada o salida física no cuenta por sí sola como cobertura territorial.

### B. Un registro por evento

- Extender `ConfirmacionServicio` para añadir el tipo de evento y datos de acceso.

Ventaja: menos relaciones. Riesgo: mezcla la confirmación territorial de servicio con el control de acceso y repite datos del ticket/token por cada entrada y salida.

No se ha elegido ni implementado ninguna alternativa.

## Decisiones que faltan

- **P11-CR-01-Q1 (respondida):** ambos grupos de eventos: inicio/fin de asignación e ingreso/salida de lugar operativo.
- **P11-CR-01-Q2 (en espera):** ¿un código emitido por asignación sirve para varios eventos, o se emite uno diferente por evento?
- **P11-CR-01-Q3 (respondida):** se controlará cualquier punto habilitado de la ruta.
- Definir expiración, reuso y anulación del código; el sistema no debe emitir un token que autentique a una persona.

## Tratamiento de seguridad

- La credencial identifica una asignación/vehículo y no una persona.
- No se publicarán tokens activos en GitHub ni se incluirán tokens reales en documentación, ejemplos o commits.
- En una futura prueba se usarán exclusivamente valores sintéticos etiquetados `SINTÉTICO — SOLO USO ACADÉMICO`.
- Si se usa token secreto, persistir solo un hash no reversible con aleatoriedad apropiada. La propuesta concreta deberá cotejarse contra la versión MariaDB aprobada.
- Los tickets impresos se representan mediante identificadores opacos; no guardar nombre, documento o datos del conductor.

## Próximo paso

Al recibir respuesta a Q2, actualizar el análisis, modelo lógico E5, normalización E6, diagrama E7, trazabilidad E8 y los artefactos físicos E9–E11 que dependan del alcance. Después pedir aprobación de esos cambios. No crear aún tablas, triggers, procedimientos, tickets reales ni tokens reales.

