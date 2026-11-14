# Catálogo conceptual P11

**Versión:** 0.2 — revisada con decisiones del usuario (2026-11-14).  
**Estado:** Conceptual aprobado previamente; presente edición actualiza la interpretación y alimenta el lógico PROPUESTO.

| Concepto | Propósito | Datos conceptuales | Identificador | Estado/fundamento |
|---|---|---|---|---|
| Empresa operadora | Organización que presta el servicio | Nombre/razón, datos operativos por definir | ID conceptual por definir | Hecho del enunciado. |
| Vehículo | Unidad de flota | Capacidad, accesibilidad, características | ID conceptual por definir | Hecho. Empresa actual como relación asumida; historial por confirmar. |
| Ruta | Servicio organizado | Código/nombre/descripción por definir | ID conceptual por definir | Hecho. |
| Sector | Unidad territorial para cobertura y agregados | Código/nombre por definir | ID conceptual por definir | Decisión del usuario: usar “sector”. |
| Punto de servicio | Lugar programable de servicio | Tipo paradero/terminal; ubicación | ID conceptual por definir | Terminal como tipo de punto es supuesto pendiente. |
| Paradero habilitado | Define el conjunto en el que puede programarse una parada | Punto; vigencia/alcance por definir | Punto o clave de habilitación | La regla de habilitación está confirmada; alcance temporal/global/por ruta por acordar. |
| Paso de ruta | Ordena puntos dentro de una ruta | Orden | Ruta + orden, candidato | Asociación M:N con atributo ordinal. |
| Turno | Clasifica una franja o jornada de operación | Nombre/código | ID conceptual por definir | El enunciado requiere el turno; si es solo etiqueta podría quedar como atributo. |
| Asignación | Historial de vehículo en ruta y turno | Inicio, fin previsto/real, estado | ID conceptual por definir | Incluye regla de simultaneidad aclarada parcialmente por usuario. |
| Inspección | Encabezado del evento de revisión | Fecha/hora; responsable | ID conceptual por definir | Objetivos prioritarios: empresa y vehículo. Asignación/servicio es opcional. |
| Objetivo inspeccionado | Vincula inspección con sus objetos | Vehículo, empresa o asignación | Clave de inspección + objeto | Relaciones separadas para mantener referencias verificables. |
| Condición | Catálogo de aspectos revisables | Laboral, parada autorizada, tecnología de recaudo | Código/ID por definir | Hecho del enunciado. |
| Evaluación de inspección | Resultado por condición y evento | Resultado, observación opcional | Inspección + condición | Permite histórico por revisión. |
| Confirmación de servicio | Evidencia de servicio prestado por vehículo en sector | Fecha/hora, tipo de evidencia, referencia no personal opcional | ID conceptual por definir | Decisión usuario: token/ticket app/impreso; no presupone contenido del comprobante. |
| Reporte ilegal agregado | Guarda volumen por sector y periodo | Inicio/fin de periodo, total | Sector + periodo candidato | Nunca registra persona ni detalle individual. |

## Relaciones conceptuales

- Empresa **opera** vehículos (0..N vehículos por empresa; 1 empresa por vehículo como supuesto temporal S01).
- Ruta **contiene pasos ordenados**; cada paso refiere un punto de servicio. Un punto puede aparecer en varias rutas.
- Sector **ubica** puntos de servicio y es unidad de agregación de confirmaciones y totales (S05: pertenencia única por confirmar).
- Un vehículo recibe asignaciones a rutas/turnos, conservadas históricamente.
- Inspección **tiene objetivos** en empresa y/o vehículo; puede vincularse a asignación como opción secundaria. La cardinalidad exacta y mínimo de un objetivo quedan para validar.
- Inspección **evalúa** condiciones mediante resultados por condición.
- Una asignación **produce confirmaciones de servicio** asociadas a sectores y evidencia; vehículo/ruta se obtienen a través de la asignación.
- Sector **agrupa reportes ilegales** por periodo como total agregado.

## Atributos, catálogos y entidades

Capacidad y accesibilidad describen vehículos. Orden pertenece al paso de ruta. El estado/intervalo pertenece a asignación. Resultado pertenece a la evaluación de condición. Los tipos de evidencia y punto pueden ser catálogos simples; no se crea jerarquía de clases sin necesidad. Si accesibilidad admite varias características independientes, se revisará una asociación en lugar de guardar una lista textual.

## Privacidad

No se modelan conductores ni personas reportadas por transporte ilegal. La inspección requiere responsable, pero su representación como identificador protegido está pendiente. Para la confirmación de servicio, se propone conservar tipo y referencia técnica no personal; no almacenar tokens ni tickets completos hasta que exista una necesidad aprobada.

## Historial

Asignaciones, inspecciones y confirmaciones son eventos históricos. Sus modificaciones y retención se definirán después sin sobrescribir el historial. Los totales de transporte ilegal no permiten reconstruir casos individuales.

## Revisión

v0.1 proponía asignación como posible único objeto de inspección y cobertura por paraderos programados. v0.2 corrige esa propuesta: inspeccionar principalmente empresas/vehículos y medir cobertura con confirmaciones efectivas por sector. Motivo: decisiones explícitas del usuario; consecuencia: nuevas relaciones conceptuales `Objetivo inspeccionado` y `Confirmación de servicio`.
