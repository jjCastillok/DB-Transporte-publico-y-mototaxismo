# Análisis semántico

**Proyecto:** Transporte público y mototaxismo: rutas, flota y cumplimiento  
**Versión:** 0.1 — propuesta para revisión  
**Estado:** PROPUESTO; aún no aprobado ni probado en un SGBD.

## 1. Situación, objetivo y alcance

El enunciado presenta una reorganización del transporte público mediante operadores, rutas, paraderos y terminales de transferencia, y la necesidad de verificar condiciones de operación. El sistema debe conservar información de flota, accesibilidad, secuencia de paraderos, asignaciones por turno, inspecciones y reportes agregados de transporte ilegal.

El objetivo declarado es organizar flota, rutas, turnos, paraderos y verificación de condiciones de operación.

### Incluido según el enunciado

- Empresas operadoras, vehículos, capacidad y características de accesibilidad.
- Rutas, paraderos habilitados, terminales de transferencia y orden de paraderos por ruta.
- Asignación de vehículos a rutas por fecha y turno, con horas de inicio y fin.
- Inspecciones de condiciones laborales, uso de paradas autorizadas y tecnología de recaudo, con fecha, responsable y resultado.
- Reportes de transporte ilegal guardados únicamente como totales por zona y periodo.
- Conservación del historial de asignaciones e inspecciones.

### Fuera de alcance por ahora

El enunciado no solicita identificar conductores, pasajeros ni personas vinculadas a reportes de transporte ilegal. Tampoco especifica geolocalización en tiempo real, pagos individuales, recaudo transaccional, despacho en vivo ni nómina. No se proponen esos módulos en esta fase.

## 2. Actores y unidades de observación

| Actor confirmado | Interacción que sugiere el enunciado |
|---|---|
| Operador de transporte público | Mantener información de la empresa, flota y operación |
| Responsable de flota | Mantener vehículos y asignaciones |
| Autoridad de movilidad | Registrar o consultar inspecciones y cumplimiento |

No se infiere que sean las únicas personas usuarias del sistema. El responsable de una inspección se conserva como referencia de responsabilidad; su nombre u otros datos personales no se definen aquí.

Unidades de información propuestas: vehículo, punto de servicio, paso de una ruta por un punto, asignación de servicio, inspección, resultado por condición revisada y total reportado por zona y periodo.

## 3. Hechos confirmados por el enunciado

- Se registran empresas operadoras, vehículos, capacidad y accesibilidad.
- Una ruta contiene paraderos habilitados en un orden determinado; existen terminales de transferencia.
- Las asignaciones identifican vehículo, ruta, fecha, turno, hora de inicio y hora de fin.
- Las inspecciones registran fecha, responsable, resultado y revisión de las condiciones nombradas.
- Los reportes de transporte ilegal se almacenan como totales por zona y periodo y no identifican personas.
- Se conserva el historial de asignaciones e inspecciones.
- El sistema debe responder las cuatro consultas listadas en el enunciado: operación por ruta y turno, proporción accesible, cobertura y cumplimiento/puntos de transbordo.

## 4. Inferencias y supuestos por validar

| ID | Propuesta o supuesto | Motivo | Riesgo si no se confirma |
|---|---|---|---|
| S01 | Cada vehículo se relaciona con una empresa operadora a la vez. | Facilita atribuir la flota y el cumplimiento a una empresa. | Si la propiedad, afiliación o administración puede ser múltiple o cambiar históricamente, la relación debe representarlo. |
| S02 | Un punto de servicio tendrá un tipo: paradero o terminal de transferencia. | El mismo recorrido puede expresar su orden y los puntos de transbordo sin duplicar el concepto de ubicación. | Un terminal podría no ser una parada de ruta o tener datos propios no incluidos en el enunciado. |
| S03 | El orden de los puntos de una ruta representa cobertura planificada. | El enunciado pide el orden de paraderos y consulta por cobertura. | No acredita frecuencia ni que el servicio haya ocurrido efectivamente. |
| S04 | La inspección puede asociarse a una asignación vehículo-ruta-turno. | Así se puede derivar qué empresa, vehículo y ruta fueron inspeccionados. | El enunciado no identifica el objeto inspeccionado; puede ser solo la empresa o más de un objeto. |
| S05 | La cobertura de un sector se obtiene por los puntos de servicio de rutas que caen dentro de ese sector. | Permite responder qué sectores y paraderos carecen de cobertura. | Requiere definir el catálogo geográfico y qué significa “tener cobertura”. |
| S06 | La proporción accesible se calcula sobre la flota registrada en una fecha de corte. | La consulta pide una proporción, pero no fija denominador ni periodo. | Sin estado o vigencia del vehículo no se distingue flota activa de retirada. |
| S07 | Un turno puede tratarse como una etiqueta de la asignación con su intervalo horario registrado allí. | El enunciado requiere fecha, turno y horas de inicio/fin, sin catálogo de turnos descrito. | Si hay turnos predefinidos con horarios oficiales, convendrá modelarlos aparte. |

## 5. Vacíos que pueden cambiar el modelo

1. **Objeto de la inspección:** empresa, vehículo, ruta, asignación de servicio u otra combinación.
2. **Unidad geográfica:** si “zona” y “sector” son la misma división; si cada paradero pertenece a una zona o sector; y si la cobertura se define por la existencia de una ruta programada.
3. **Solapamiento temporal:** si se prohíbe la coincidencia de asignaciones del mismo vehículo solo dentro del mismo turno o en cualquier turno.
4. **Autorización de paraderos:** si la habilitación es general para la ciudad o específica para cada ruta, y si se necesita conservar su vigencia histórica.
5. **Proporción de accesibilidad:** población de vehículos y fecha de corte que forman el denominador.
6. **Fuentes de contexto:** se citan Alcaldía (condiciones de operación, nuevos buses, busetones y ruta piloto), BID y Santa Marta Cómo Vamos, pero el enunciado no incluye enlaces, títulos completos ni fechas. No se han usado como evidencia de reglas operativas.

Las primeras tres preguntas se presentan al equipo antes de aprobar el conceptual. Los demás puntos permanecen como decisiones pendientes o supuestos explícitos.

## 6. Requisitos funcionales propuestos desde el enunciado

| ID | Requisito | Criterio observable de aceptación |
|---|---|---|
| R01 | Registrar empresas operadoras y sus vehículos, incluyendo capacidad y accesibilidad. | Se puede identificar a qué operador corresponde cada vehículo y consultar su capacidad y atributos de accesibilidad, conforme a la relación aprobada. |
| R02 | Registrar rutas y sus puntos/paraderos habilitados. | Cada ruta puede listar los puntos que tiene autorizados o programados según la definición acordada. |
| R03 | Registrar terminales de transferencia. | Los puntos identificados como terminal permiten consultar dónde hay transbordo. |
| R04 | Conservar el orden de los paraderos de cada ruta. | Para una ruta se recupera una secuencia sin posiciones ambiguas. |
| R05 | Registrar asignaciones de vehículos a ruta y turno con fecha e intervalo horario. | Para una fecha/turno se listan los vehículos y rutas asignados con sus horas. |
| R06 | Registrar inspecciones de operación. | Cada inspección tiene fecha, responsable, resultado y objeto inspeccionado por confirmar. |
| R07 | Registrar el resultado de las condiciones revisadas. | Se puede identificar el resultado para cada condición laboral, de parada autorizada o de tecnología de recaudo que se haya inspeccionado. |
| R08 | Conservar historial de asignaciones e inspecciones. | Los cambios no sobrescriben ni eliminan el registro histórico de esas operaciones. |
| R09 | Registrar reportes de transporte ilegal solo como totales agregados por zona y periodo. | Se consulta un total por zona y periodo sin almacenar persona identificable ni detalle individual. |
| R10 | Consultar flota por ruta y turno. | Una consulta devuelve vehículos asignados al filtro solicitado. |
| R11 | Consultar proporción de flota accesible y cobertura de sectores/paraderos. | Se explicitan corte, población y criterio de cobertura antes de aprobar los resultados. |
| R12 | Consultar cumplimiento por empresa y puntos de transbordo. | El cumplimiento se deriva de inspecciones y los puntos de transbordo se distinguen en el modelo. |

## 7. Reglas de negocio confirmadas

| ID | Regla | Criterio observable |
|---|---|---|
| RN01 | Un vehículo no puede estar asignado a dos rutas simultáneamente durante un mismo turno. | Al validar la regla acordada, una asignación temporal incompatible no puede considerarse válida. La extensión entre turnos queda por confirmar. |
| RN02 | Las paradas programadas deben pertenecer al conjunto de paraderos autorizados. | Cada punto programado en la ruta debe estar autorizado según el alcance temporal y geográfico por definir. |
| RN03 | Los datos de conductores deben protegerse; los reportes de transporte ilegal no identifican personas. | El diseño minimiza datos personales y el registro de transporte ilegal solo conserva totales agregados por zona y periodo. |
| RN04 | Se conserva el historial de asignaciones e inspecciones. | Los hechos anteriores permanecen consultables después de registrar cambios posteriores. |

## 8. Riesgos y protección de información

- No se almacenarán datos de conductores que el enunciado no requiera; si más adelante se vuelven necesarios, se deberá definir finalidad, acceso y retención antes de incorporarlos.
- Los reportes de transporte ilegal no tendrán filas a nivel de persona, conductor, vehículo observado ni evento individual; solo agregados por zona y periodo.
- La agregación puede permitir inferencias si los grupos son muy pequeños. El enunciado no fija umbral de supresión; queda pendiente de análisis antes de publicar reportes.
- La identidad y el control de acceso del responsable de inspección deben limitarse a lo necesario para la trazabilidad.
- Las fuentes de contexto no se han verificado y no se usarán para convertir cifras externas en capacidades o reglas actuales.

## 9. Redacción precisa propuesta

**Redacción aclarada, sin agregar reglas:** El sistema conservará los datos de operadores, vehículos, rutas, paraderos habilitados, terminales de transferencia y asignaciones de vehículos a rutas por fecha y turno, incluyendo sus intervalos horarios. También conservará inspecciones que documenten fecha, responsable, resultado y las condiciones de operación revisadas. Los reportes de transporte ilegal se almacenarán únicamente como cantidades agregadas por zona y periodo, sin identificación de personas. El sistema mantendrá los historiales de asignaciones e inspecciones y permitirá consultar operación por ruta/turno, accesibilidad de la flota, cobertura y cumplimiento empresarial. Las definiciones de zona/sector, alcance de autorización, objeto de inspección, regla de solapamiento y denominador de accesibilidad quedan pendientes de validación.

## 10. Decisiones registradas

| ID | Decisión o propuesta | Estado |
|---|---|---|
| D01 | Mantener separados los hechos del enunciado y las inferencias/supuestos. | Aplicada al documento; pendiente de revisión del equipo. |
| D02 | No almacenar identificadores de conductores ni detalles individuales de transporte ilegal en el conceptual inicial. | Propuesta derivada de RN03; pendiente de aprobación. |
| D03 | Proponer un punto de servicio con tipo paradero/terminal para representar secuencias y transbordos. | Propuesta conceptual pendiente. |
| D04 | Representar las inspecciones con resultados por condición revisada, para permitir revisar varias condiciones en una inspección. | Inferencia pendiente de aprobación. |
| D05 | No fijar motor, DDL, lenguaje de aplicación ni datos de prueba en la Fase 1. | Conforme al orden del Prompt Maestro. |

## 11. Preguntas de comprensión

1. ¿Por qué la asignación debe tener intervalo horario además de fecha y turno?
2. ¿Qué información se pierde y qué riesgo se evita al guardar transporte ilegal solo como total por zona y periodo?

## 12. Evidencias mínimas para aceptar la Fase 1

- Confirmar objeto inspeccionado, definición geográfica de cobertura y alcance de la regla de solapamiento.
- Revisar el catálogo conceptual y las cardinalidades propuestas.
- Aceptar, corregir o descartar los supuestos S01–S07 y las decisiones D02–D04.
- Completar enlaces y fechas de las fuentes antes de emplearlas para agregar contexto factual.
- Registrar aprobación o cambios en `docs/bitacora.md`.

## 13. Lista de comprobación

- [x] El objetivo y alcance se derivan del enunciado.
- [x] Hechos, inferencias, dudas y propuestas se distinguen.
- [x] Los reportes de transporte ilegal se limitan a agregados, sin individuos.
- [x] Se identifican vacíos con impacto conceptual.
- [x] Se proponen criterios de aceptación y trazabilidad.
- [ ] El usuario valida las preguntas abiertas y el catálogo.
- [ ] El usuario aprueba la Fase 1.

## Cambios de trazabilidad

Las relaciones conceptuales iniciales que corresponden a R01–R12 y RN01–RN04 se enumeran en `04_trazabilidad_conceptual.csv`. Todos los elementos están en estado **PROPUESTO**, no aprobado.
