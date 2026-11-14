# E5 · Modelo lógico relacional P11

**Versión:** 0.3 — diseño lógico aprobado, 2026-11-14  
**Estado:** APROBADO POR EL USUARIO para Fase 2; las decisiones de implementación señaladas como pendientes se conservan para su cierre antes del DDL.  
**Origen:** modelo conceptual aprobado, revisado con D01–D03.  
**SGBD:** MariaDB seleccionado después de aprobar Fase 2. Este artefacto sigue siendo independiente del dialecto y no define tipos SQL ni crea código de implementación.

## Decisiones y criterios

- Identificadores técnicos simples (`id_*`) propuestos para las entidades; los identificadores naturales y sus reglas de unicidad quedan por definir en Fase 3.
- Atributos descriptivos dependen de su entidad propietaria y no se repiten en tablas de relación.
- Relaciones N:M se representan con relaciones propias explícitas.
- Conservar historia significa insertar nuevas asignaciones e inspecciones en vez de sustituir las anteriores.
- Cada evento de inspección se propone para un objetivo: vehículo, empresa o asignación. Tres FK reales opcionales y una regla exclusiva evitan una columna polimórfica `tipo_objeto + id_objeto`; se registra un evento separado por cada objetivo para que los resultados sean inequívocos.
- `ConfirmacionServicio` enlaza con la asignación: vehículo y ruta se derivan desde esa relación y no se copian. Guarda sector, fecha/hora, tipo de evidencia y referencia no personal opcional.
- El solapamiento de asignaciones queda como regla temporal de dominio. Su validación no se declara resuelta por una FK simple; el significado de estado `FINALIZANDO` requiere aprobación.

## Relaciones propuestas

Abreviaturas: PK = clave primaria; FK = clave foránea; UQ = candidata de unicidad. `NN` expresa obligatoriedad lógica; `?` expresa dato opcional. No se han decidido longitudes, tipos, valores codificados ni reglas de `ON DELETE`.

| Relación lógica | Atributos propuestos y claves | Cardinalidad / observación |
|---|---|---|
| **EmpresaOperadora** | `id_empresa` PK; `nombre`; otros datos operativos por definir | Empresa 0..N Vehículo. |
| **Vehiculo** | `id_vehiculo` PK; `id_empresa` FK; `capacidad`; `accesible`; características de accesibilidad | Cada vehículo se asocia a una empresa (S01). |
| **Sector** | `id_sector` PK; `nombre/codigo` por definir | Unidad territorial usada por confirmación y agregados. |
| **PuntoServicio** | `id_punto` PK; `id_sector` FK; `tipo` (paradero/terminal); `nombre/ubicacion` por definir | Cada punto pertenece a un sector (S05). |
| **ParaderoHabilitado** | `id_punto` PK/FK a PuntoServicio; datos de habilitación por definir | Conjunto de paraderos autorizados al que deben pertenecer los pasos de ruta (RN02). Se propone autorización global vigente; alcance por ruta y vigencia histórica siguen abiertos (S07). |
| **Ruta** | `id_ruta` PK; `codigo/nombre` por definir | Una ruta tiene pasos ordenados. |
| **PasoRuta** | `(id_ruta, orden)` PK; `id_ruta` FK; `id_punto` FK a `ParaderoHabilitado.id_punto` | El FK al conjunto habilitado impide programar paraderos no autorizados bajo S07. El mismo paradero puede repetirse en una ruta si usa otro ordinal. |
| **Turno** | `id_turno` PK; `nombre` | Catálogo candidato; horarios no duplicados en asignación salvo que el turno signifique solo etiqueta. |
| **Asignacion** | `id_asignacion` PK; `id_vehiculo` FK; `id_ruta` FK; `id_turno` FK; `inicio`; `fin_programado`; `estado`; `fin_real`? | Historial de asignaciones. Solapamiento se valida como regla temporal, pendiente D03. |
| **Inspeccion** | `id_inspeccion` PK; `fecha_hora`; `responsable` (referencia protegida, semántica pendiente); `id_vehiculo` FK?; `id_empresa` FK?; `id_asignacion` FK? | Exactamente una FK de objetivo debe estar informada: vehículo o empresa como foco; asignación es opcional y secundaria. |
| **Condicion** | `id_condicion` PK; `nombre`; `categoria` | Catálogo de condiciones revisables, como laborales, parada autorizada y recaudo. |
| **EvaluacionInspeccion** | `(id_inspeccion, id_condicion)` PK; FK a Inspeccion y Condicion; `resultado`; `observacion`? | Resultado depende de la inspección y la condición. Un resultado general puede derivarse si se acuerda una regla. |
| **ConfirmacionServicio** | `id_confirmacion` PK; `id_asignacion` FK; `id_sector` FK; `fecha_hora`; `tipo_evidencia`; `referencia_evidencia`? | Confirmación por vehículo implícito en asignación y sector servido. El contenido del token/ticket no se almacena (S03). |
| **ReporteIlegalSectorPeriodo** | `(id_sector, periodo_inicio, periodo_fin)` PK; FK a Sector; `total` | Únicamente total agregado; no conserva sucesos individuales/personas. |

### Nota sobre `PasoRuta` y autorización

La PK `(id_ruta, orden)` permite modelar la secuencia aun cuando un mismo paradero aparezca más de una vez. El FK de `PasoRuta.id_punto` apunta a `ParaderoHabilitado`, y esta relación a su vez referencia `PuntoServicio`; así el paso solo puede seleccionar un paradero del conjunto habilitado, sin copiar sus datos. Esta propuesta interpreta la autorización como global y vigente. Si es por ruta o tiene vigencias históricas, la relación requiere ajuste antes de aprobar Fase 2. Un terminal solo entra en la secuencia como paradero habilitado; confirmar si esa interpretación es correcta.

### Nota sobre asignaciones simultáneas

Las relaciones no prohíben por sí solas dos intervalos del mismo vehículo. La validación propuesta compara intervalos y el estado de la asignación que estaba finalizando o finalizada. Falta acordar si `FINALIZANDO` permite que la nueva asignación comience antes del `fin_real` anterior, y qué marca el cierre real. La base física tendrá que implementar la regla aprobada con el mecanismo que soporte el SGBD; no se sustituye por una unicidad simple de vehículo+fecha+turno.

## Integridad lógica adicional

1. `capacidad` debe ser positiva y accesibilidad debe representar una condición definida, no texto libre ambiguo.
2. `inicio < fin_programado`; cuando exista `fin_real`, debe ser posterior al inicio.
3. La secuencia por ruta no debe tener dos pasos con el mismo ordinal.
4. Cada paso debe referenciar punto habilitado en el alcance/vigencia aplicable (RN02).
5. Una inspección debe tener exactamente un objetivo entre las tres FK opcionales. Es una regla de exclusividad que se puede expresar luego con una comprobación sobre las FK, según soporte del SGBD.
6. Cada evaluación refiere exactamente una inspección y una condición.
7. Una confirmación debe referir una asignación existente y un sector; la fecha de confirmación debe ser compatible con el periodo efectivo del servicio, regla a precisar.
8. En reportes agregados, `total` es un entero no negativo y el par sector-periodo no se duplica. La granularidad del periodo no está fijada.

## Consultas que cubre el diseño

| Consulta | Relaciones principales | Criterio |
|---|---|---|
| Vehículos por ruta y turno | Asignacion, Vehiculo, Ruta, Turno | Filtrar por turno e intervalo/fecha. |
| Proporción de flota accesible | Vehiculo | Numerador accesibles / denominador activos a fecha de corte (S08 pendiente). |
| Sectores sin cobertura | Sector, ConfirmacionServicio | Sectores sin confirmación en ventana consultada (S06). |
| Paraderos sin cobertura | PuntoServicio, ConfirmacionServicio | Depende de cómo se mapea una confirmación sectorial a un punto; actualmente no puede responderse con precisión si se requiere nivel de paradero. |
| Empresas cumplidoras | EmpresaOperadora, Inspeccion, EvaluacionInspeccion, Condicion | Regla agregada de cumplimiento aún por definir; no se asume “todas las inspecciones aprobadas”. |
| Puntos de transbordo | PuntoServicio | Filtrar tipo terminal. |

## Pendientes que afectan aprobación

- S01: asignación histórica/cambios de operador.
- S02: terminal como elemento de ruta.
- S03: contenido mínimo de referencia de evidencia sin almacenar token/ticket bruto.
- S04: confirmar si el objetivo de una inspección individual es único; definir la referencia protegida del responsable.
- S05: sector único o múltiple por punto.
- S06/S08: periodo de cobertura y denominador activo accesible.
- S07: confirmar habilitación global o por ruta, y si su vigencia debe conservarse históricamente; confirmar si un terminal puede ser un paso de ruta al habilitarlo como paradero.
- D03: semántica temporal exacta de solapamiento y finalización.
- Consulta de “paradero sin cobertura”: confirmar si basta cobertura por sector o si se necesita confirmación individual por punto.
- Definir cómo se deriva el cumplimiento empresarial a partir de inspecciones.

## Por qué se modeló así

Una confirmación es un evento separado de la programación: conservarla permite distinguir un sector previsto de un sector efectivamente servido. La inspección usa FK específicas y no una referencia genérica. El total agregado tiene su propio grano sector-periodo, sin detalle personal. Las relaciones de historial guardan eventos anteriores y no reemplazan su estado pasado.

## Alternativa considerada

Se consideró permitir varios objetivos dentro de un mismo registro de inspección. Se propone un único objetivo por evento para que cada resultado por condición se refiera a un objeto inequívoco; si el usuario necesita agrupar varios, cada evaluación también deberá indicar el objetivo específico.

## Checklist E5

- [x] Relaciones lógicas, PK/FK propuestas y cardinalidades esenciales descritas.
- [x] La relación PasoRuta referencia al conjunto de paraderos habilitados para representar RN02.
- [x] Relaciones N:M resueltas con asociaciones.
- [x] Historial, privacidad, confirmaciones y reglas temporales tratados.
- [ ] Decisiones pendientes revisadas por el usuario.
- [ ] Aprobación de fase lógica.
- [ ] Ningún SGBD ni DDL definidos.

**Historial v0.2:** se explicitó la FK de `PasoRuta` al conjunto autorizado para que RN02 forme parte de la integridad lógica. Motivo: el primer borrador solo dejaba la regla como validación pendiente. Consecuencia: se adopta autorización global vigente como supuesto temporal (S07), sujeto a decisión del usuario.

