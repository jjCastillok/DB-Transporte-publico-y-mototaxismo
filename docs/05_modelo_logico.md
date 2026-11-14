# E5 · Modelo lógico relacional P11

**Versión:** 0.1 — propuesta para revisión, 2026-11-14  
**Estado:** PROPUESTO; no aprobado por el usuario.  
**Origen:** modelo conceptual aprobado, revisado con D01–D03.  
**SGBD:** no seleccionado. Esto es diseño lógico: no define tipos SQL ni crea código de implementación.

## Decisiones y criterios

- Identificadores técnicos simples (`id_*`) propuestos para las entidades; los identificadores naturales y sus reglas de unicidad quedan por definir en Fase 3.
- Atributos descriptivos dependen de su entidad propietaria y no se repiten en tablas de relación.
- Relaciones N:M se representan con relaciones propias explícitas.
- Conservar historia significa insertar nuevas asignaciones e inspecciones en vez de sustituir las anteriores.
- Las relaciones de inspección separan objetivos vehículo, empresa y asignación; esto evita una columna polimórfica `tipo_objeto + id_objeto` sin FK verificable.
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
| **ParaderoHabilitado** | `id_punto` PK/FK; datos de vigencia por definir | Separa habilitación de inclusión y orden en ruta (RN02, S07). |
| **Ruta** | `id_ruta` PK; `codigo/nombre` por definir | Una ruta tiene pasos ordenados. |
| **PasoRuta** | `(id_ruta, orden)` PK; `id_ruta` FK; `id_punto` FK; UQ propuesta `(id_ruta, id_punto, orden)` sería redundante respecto a PK y se descarta | Cada paso identifica un punto de servicio de la ruta; la autorización se debe validar. La posibilidad de repetir punto queda abierta. |
| **Turno** | `id_turno` PK; `nombre` | Catálogo candidato; horarios no duplicados en asignación salvo que el turno signifique solo etiqueta. |
| **Asignacion** | `id_asignacion` PK; `id_vehiculo` FK; `id_ruta` FK; `id_turno` FK; `inicio`; `fin_programado`; `estado`; `fin_real`? | Historial de asignaciones. Solapamiento se valida como regla temporal, pendiente D03. |
| **Inspeccion** | `id_inspeccion` PK; `fecha_hora`; `responsable` (referencia protegida, semántica pendiente) | Encabezado común y fecha del evento. Resultado por condición, no copia resumen redundante. |
| **InspeccionVehiculo** | `(id_inspeccion, id_vehiculo)` PK; FK a Inspeccion y Vehiculo | Una inspección puede revisar uno o más vehículos. |
| **InspeccionEmpresa** | `(id_inspeccion, id_empresa)` PK; FK a Inspeccion y EmpresaOperadora | Una inspección puede revisar una o más empresas. |
| **InspeccionAsignacion** | `(id_inspeccion, id_asignacion)` PK; FK a Inspeccion y Asignacion | Opcional y secundario; permite inspección asociada a un servicio. |
| **Condicion** | `id_condicion` PK; `nombre`; `categoria` | Catálogo de condiciones revisables, como laborales, parada autorizada y recaudo. |
| **EvaluacionInspeccion** | `(id_inspeccion, id_condicion)` PK; FK a Inspeccion y Condicion; `resultado`; `observacion`? | Resultado depende de la inspección y la condición. Un resultado general puede derivarse si se acuerda una regla. |
| **ConfirmacionServicio** | `id_confirmacion` PK; `id_asignacion` FK; `id_sector` FK; `fecha_hora`; `tipo_evidencia`; `referencia_evidencia`? | Confirmación por vehículo implícito en asignación y sector servido. El contenido del token/ticket no se almacena (S03). |
| **ReporteIlegalSectorPeriodo** | `(id_sector, periodo_inicio, periodo_fin)` PK; FK a Sector; `total` | Únicamente total agregado; no conserva sucesos individuales/personas. |

### Nota sobre `PasoRuta` y autorización

La PK `(id_ruta, orden)` permite modelar la secuencia aun cuando un mismo punto aparezca más de una vez. El punto del paso debe formar parte de los habilitados. La tabla `ParaderoHabilitado` representa habilitación global como propuesta inicial; si la habilitación es por ruta o tiene vigencia evaluada en fecha de operación, cambiará esta relación antes de aprobar el diseño lógico. No se debe presentar la vigencia como aplicada hasta resolver S07.

### Nota sobre asignaciones simultáneas

Las relaciones no prohíben por sí solas dos intervalos del mismo vehículo. La validación propuesta compara intervalos y el estado de la asignación que estaba finalizando o finalizada. Falta acordar si `FINALIZANDO` permite que la nueva asignación comience antes del `fin_real` anterior, y qué marca el cierre real. La base física tendrá que implementar la regla aprobada con el mecanismo que soporte el SGBD; no se sustituye por una unicidad simple de vehículo+fecha+turno.

## Integridad lógica adicional

1. `capacidad` debe ser positiva y accesibilidad debe representar una condición definida, no texto libre ambiguo.
2. `inicio < fin_programado`; cuando exista `fin_real`, debe ser posterior al inicio.
3. La secuencia por ruta no debe tener dos pasos con el mismo ordinal.
4. Cada paso debe referenciar punto habilitado en el alcance/vigencia aplicable (RN02).
5. Una inspección debe tener al menos un objetivo en `InspeccionVehiculo`, `InspeccionEmpresa` o `InspeccionAsignacion`. Se requiere validación transaccional o regla de aplicación porque los objetivos están en relaciones separadas.
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
| Empresas cumplidoras | EmpresaOperadora, InspeccionEmpresa, EvaluacionInspeccion, Condicion | Regla agregada de cumplimiento aún por definir; no se asume “todas las inspecciones aprobadas”. |
| Puntos de transbordo | PuntoServicio | Filtrar tipo terminal. |

## Pendientes que afectan aprobación

- S01: asignación histórica/cambios de operador.
- S02: terminal como elemento de ruta.
- S03: contenido mínimo de referencia de evidencia sin almacenar token/ticket bruto.
- S04: objetivo múltiple y responsable de inspección.
- S05: sector único o múltiple por punto.
- S06/S08: periodo de cobertura y denominador activo accesible.
- S07: alcance/vigencia de habilitación.
- D03: semántica temporal exacta de solapamiento y finalización.
- Consulta de “paradero sin cobertura”: confirmar si basta cobertura por sector o si se necesita confirmación individual por punto.
- Definir cómo se deriva el cumplimiento empresarial a partir de inspecciones.

## Por qué se modeló así

Una confirmación es un evento separado de la programación: conservarla permite distinguir un sector previsto de un sector efectivamente servido. La inspección tiene tablas de destino específicas para mantener FK claras y evitar el patrón frágil de una referencia genérica. El total agregado tiene su propio grano sector-periodo, sin detalle personal. Las relaciones de historial guardan eventos anteriores y no reemplazan su estado pasado.

## Alternativa considerada

Se consideró una inspección con columnas opcionales para `id_vehiculo`, `id_empresa` y `id_asignacion`. Se prefiere separar los vínculos porque admite varios objetivos del mismo tipo, evita filas con columnas vacías y expresa la integridad con claves foráneas. La condición “al menos un objetivo” no se puede asegurar con un `NOT NULL` único y se registra como regla de validación posterior.

## Checklist E5

- [x] Relaciones lógicas, PK/FK propuestas y cardinalidades esenciales descritas.
- [x] Relaciones N:M resueltas con asociaciones.
- [x] Historial, privacidad, confirmaciones y reglas temporales tratados.
- [ ] Decisiones pendientes revisadas por el usuario.
- [ ] Aprobación de fase lógica.
- [ ] Ningún SGBD ni DDL definidos.

