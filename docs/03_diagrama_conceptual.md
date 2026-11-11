# Diagrama conceptual

**Proyecto:** Transporte público y mototaxismo: rutas, flota y cumplimiento  
**Versión:** 0.1 — propuesta para revisión  
**Notación:** Chen; entidades en rectángulos, relaciones en rombos, atributos en óvalos y cardinalidades textuales.  
**Archivo editable:** `03_diagrama_conceptual.drawio`.

## Entidades y atributos principales

- **EMPRESA OPERADORA** (identificador por definir, nombre por definir)
- **VEHÍCULO** (identificador por definir, capacidad, características de accesibilidad)
- **RUTA** (identificador por definir, datos descriptivos por definir)
- **PUNTO DE SERVICIO** (identificador por definir, tipo: paradero o terminal de transferencia [supuesto], datos de ubicación por definir)
- **ZONA/SECTOR** (identificador y denominación por definir)
- **ASIGNACIÓN** (entidad asociativa: fecha, turno, hora de inicio y hora de fin)
- **INSPECCIÓN** (identificador por definir, fecha, responsable, resultado general si aplica)
- **CONDICIÓN** (catálogo conceptual: laboral, parada autorizada, tecnología de recaudo)
- **EVALUACIÓN DE CONDICIÓN** (resultado de una condición dentro de una inspección)
- **TOTAL DE REPORTES** (periodo, cantidad total; zona identificada por relación)

## Especificación Chen textual

1. **EMPRESA OPERADORA — OPERA — VEHÍCULO:** una empresa opera cero o muchos vehículos; cada vehículo se asocia a una empresa en un momento dado (supuesto S01 pendiente).
2. **RUTA — PASO DE RUTA — PUNTO DE SERVICIO:** una ruta tiene uno o más puntos ordenados; un punto puede aparecer en cero o muchas rutas. `orden` es atributo de PASO DE RUTA. Solo se programan puntos autorizados (RN02).
3. **ZONA/SECTOR — CONTIENE — PUNTO DE SERVICIO:** una zona contiene cero o muchos puntos; cada punto pertenece a una zona (supuesto S05 pendiente).
4. **VEHÍCULO — ASIGNACIÓN — RUTA:** vehículo y ruta participan en cero o muchas asignaciones; cada asignación corresponde exactamente a un vehículo y una ruta y tiene fecha, turno e intervalo horario.
5. **TURNO — CLASIFICA — ASIGNACIÓN:** si se aprueba TURNO como entidad, un turno clasifica cero o muchas asignaciones y cada asignación pertenece a un turno. Si no hay catálogo, `turno` será un atributo de ASIGNACIÓN.
6. **ASIGNACIÓN — ES OBJETO DE — INSPECCIÓN:** una asignación puede recibir cero o muchas inspecciones; una inspección se relaciona con cero o una asignación en esta propuesta. Esta relación está pendiente de Q01.
7. **INSPECCIÓN — REVISA — CONDICIÓN:** una inspección revisa una o más condiciones; una condición puede aparecer en cero o muchas inspecciones. `resultado` pertenece a EVALUACIÓN DE CONDICIÓN.
8. **ZONA/SECTOR — AGRUPA — TOTAL DE REPORTES:** una zona puede tener cero o muchos totales por periodo; cada total corresponde a una zona. `periodo` y `cantidad` son atributos del total.

## Jerarquía y polimorfismo

No se impone una jerarquía de clases en el modelo conceptual. La propuesta inicial usa un atributo `tipo` en PUNTO DE SERVICIO. Si se confirma que paradero y terminal tienen comportamiento o información distinta, se revisará la especialización. La futura implementación Java podrá aplicar polimorfismo únicamente si esa diferencia se valida; no se generará código en esta fase.

## Dudas visibles en el diagrama

- Q01: ¿la inspección corresponde a una asignación, empresa, vehículo, ruta u objeto combinado?
- Q02: ¿zona y sector son el mismo nivel geográfico, y la cobertura se determina por los puntos servidos?
- Q03: ¿la restricción de no simultaneidad aplica entre turnos o solo dentro de un mismo turno?
- S02: el terminal se considera un tipo de punto de servicio solo como propuesta.
