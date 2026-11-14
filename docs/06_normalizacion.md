# E6 · Normalización hasta tercera forma normal

**Versión:** 0.1 — revisión estática del modelo lógico propuesto.  
**Estado:** REVISADO ESTÁTICAMENTE; sujeto a aprobación del modelo. No probado en SGBD.

## Respuesta a la pregunta de 3FN

Sí, la 3FN es un criterio obligatorio del diseño. En esta fase se revisa relación por relación y se dejan explícitas las dependencias funcionales. La afirmación solo se aplica al modelo lógico propuesto después de revisar sus pendientes; no significa que exista una base implementada o probada.

## 1FN

Se propone que cada atributo contenga un solo valor lógico. La secuencia de puntos no se guarda en una lista dentro de `Ruta`: cada punto ocupa una fila de `PasoRuta`. Los resultados de inspección tampoco se agrupan en una lista textual: cada condición evaluada ocupa una fila de `EvaluacionInspeccion`. Las características de accesibilidad deberán definirse como atributo simple o, si son múltiples categorías independientes, normalizarse como asociación antes de aprobar tipos físicos.

## 2FN

En relaciones con clave compuesta, los datos no clave dependen de toda la clave:

| Relación | Clave | Dependencia relevante |
|---|---|---|
| PasoRuta | `(id_ruta, orden)` | La clave completa determina `id_punto`. Ni ruta ni orden aisladamente determinan todos los datos del paso. |
| InspeccionVehiculo | `(id_inspeccion, id_vehiculo)` | El vínculo existe por el par; no se copian nombres ni atributos del vehículo/inspección. |
| InspeccionEmpresa | `(id_inspeccion, id_empresa)` | El vínculo existe por el par; no se copian nombre de empresa o datos de inspección. |
| InspeccionAsignacion | `(id_inspeccion, id_asignacion)` | El vínculo existe por el par; no se copian ruta, vehículo ni fecha. |
| EvaluacionInspeccion | `(id_inspeccion, id_condicion)` | El resultado depende de la condición evaluada en esa inspección, no solo de uno de los componentes. |
| ReporteIlegalSectorPeriodo | `(id_sector, periodo_inicio, periodo_fin)` | `total` depende del sector y del periodo completo. |

Las relaciones con clave simple cumplen 2FN al no existir subconjunto propio de su clave primaria.

## 3FN y dependencias funcionales

Se usan identificadores conceptuales como claves primarias. Atributos naturales (códigos o nombres) no se declaran únicos todavía; la elección de claves candidatas queda pendiente de conocer reglas de negocio.

| Relación | Dependencia funcional propuesta | Revisión 3FN |
|---|---|---|
| EmpresaOperadora | `id_empresa → nombre, datos_operativos` | Los datos propios dependen de la clave de empresa. |
| Vehiculo | `id_vehiculo → id_empresa, capacidad, accesible, caracteristicas` | La empresa se guarda como FK; no se repiten nombre/datos de empresa. |
| Sector | `id_sector → nombre/codigo` | Denominación depende de la clave del sector. |
| PuntoServicio | `id_punto → id_sector, tipo, nombre, ubicacion` | Se almacena FK sector; no se duplica nombre del sector. |
| ParaderoHabilitado | `id_punto → datos_habilitacion` | La habilitación depende del punto referenciado, bajo el supuesto global pendiente. |
| Ruta | `id_ruta → codigo/nombre, descripcion` | Datos descriptivos dependen de ruta. |
| PasoRuta | `(id_ruta, orden) → id_punto` | No se copian descripción de ruta, ubicación o atributos del punto. |
| Turno | `id_turno → nombre` | El nombre depende del identificador de turno. |
| Asignacion | `id_asignacion → id_vehiculo, id_ruta, id_turno, inicio, fin_programado, estado, fin_real` | No se repiten nombres/códigos de sus entidades relacionadas. |
| Inspeccion | `id_inspeccion → fecha_hora, responsable` | Encabezado común; no repite atributos del objetivo ni un resumen redundante de evaluaciones. |
| InspeccionVehiculo | `(id_inspeccion, id_vehiculo) → —` | Asociación sin atributos transitivos duplicados. |
| InspeccionEmpresa | `(id_inspeccion, id_empresa) → —` | Asociación sin atributos transitivos duplicados. |
| InspeccionAsignacion | `(id_inspeccion, id_asignacion) → —` | Asociación sin atributos transitivos duplicados. |
| Condicion | `id_condicion → nombre, categoria` | La descripción de condición vive solo en catálogo. |
| EvaluacionInspeccion | `(id_inspeccion, id_condicion) → resultado, observacion` | El resultado depende de la evaluación completa; no se copia nombre de condición. |
| ConfirmacionServicio | `id_confirmacion → id_asignacion, id_sector, fecha_hora, tipo_evidencia, referencia` | Empresa/vehículo/ruta se obtienen por asignación; no se almacenan como copias. |
| ReporteIlegalSectorPeriodo | `(id_sector, periodo_inicio, periodo_fin) → total` | El total depende de sector y periodo; no se repite el nombre del sector. |

En cada fila, los atributos no clave dependen de la clave, de toda la clave cuando es compuesta y no de otro atributo no clave. La 3FN es por tanto defendible bajo las dependencias propuestas.

## Anomalías que se previenen

- **Inserción:** se puede registrar una ruta o empresa sin duplicar esos datos por cada asignación.
- **Actualización:** cambiar el nombre de un sector no obliga a editarlo en cada punto y reporte.
- **Eliminación:** quitar una asignación futura no elimina el registro maestro del vehículo o de la ruta; el historial se conserva.
- **Transitiva:** empresa y sector se referencian por ID; sus nombres no se duplican en vehículos, puntos, confirmaciones o reportes.

## Límites de esta comprobación

1. “Características de accesibilidad” puede ser multivaluada. Si el usuario necesita registrar más de una característica independiente por vehículo, se propondrá un catálogo y una relación M:N; no se comprimirá en texto delimitado.
2. El responsable de inspección puede requerir una entidad/identificador con acceso protegido, pero no se registrarán datos personales de conductores.
3. La vigencia de habilitación y los cambios de operador pueden añadir historial y nuevas claves; deben quedar definidos antes de cerrar Fase 2.
4. La regla de solapamiento es temporal y no se demuestra por normalización; exige una restricción o validación física posterior.
5. No se proponen desnormalizaciones.

## Checklist E6

- [x] 1FN, 2FN y 3FN explicadas sobre las relaciones propuestas.
- [x] Dependencias funcionales relevantes anotadas.
- [x] Atributos descriptivos no se duplican en relaciones.
- [x] Limitaciones y decisiones que podrían cambiar la forma del modelo declaradas.
- [ ] Revisión y aprobación del usuario.
- [ ] No existe prueba en servidor ni evidencia de ejecución SQL.

