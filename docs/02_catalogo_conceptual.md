# Catálogo conceptual

**Proyecto:** Transporte público y mototaxismo: rutas, flota y cumplimiento  
**Versión:** 0.1 — propuesta para revisión  
**Estado:** PROPUESTO; no aprobado.

El catálogo describe conceptos del dominio, no tablas ni columnas físicas. Los identificadores se expresan de forma conceptual; no se han elegido tipos ni nombres SQL.

## Entidades candidatas

| Concepto | Propósito | Atributos conceptuales indicados o necesarios | Identificador conceptual | Fundamento y estado |
|---|---|---|---|---|
| Empresa operadora | Identificar la organización que opera el servicio. | Nombre o razón de identificación (detalle pendiente); información adicional pendiente. | Identificador de operador, por definir. | El enunciado exige registrar empresas operadoras. |
| Vehículo | Representar cada unidad de flota. | Capacidad; características de accesibilidad; otros atributos pendientes. | Identificador de vehículo, por definir. | El enunciado exige vehículos, capacidad y accesibilidad. No se asume que la placa sea el identificador. |
| Ruta | Representar un servicio/ruta que puede recibir vehículos y tener puntos ordenados. | Nombre/código y descripción pendientes. | Identificador de ruta, por definir. | El enunciado exige gestionar rutas y consultar operación por ruta. |
| Punto de servicio | Representar ubicación programable de parada o transferencia. | Tipo de punto; nombre/ubicación pendientes. | Identificador de punto, por definir. | Propuesta S02 para evitar duplicar ubicación; tipo esperado: paradero o terminal de transferencia. Requiere aprobación. |
| Zona/sector | Ubicar puntos de servicio y agrupar totales de transporte ilegal. | Nombre/código territorial pendiente. | Identificador geográfico, por definir. | Inferencia necesaria para las consultas de cobertura y los agregados por zona. Debe aclararse si “sector” y “zona” son iguales. |
| Turno | Distinguir el turno asociado a una asignación. | Nombre/código; horario fijo si existe, pendiente. | Identificador de turno, por definir. | Propuesta S07. Si el turno solo es una etiqueta por asignación, podría no ser entidad independiente. |
| Asignación de servicio | Registrar un vehículo operando en una ruta durante fecha, turno e intervalo. | Fecha, hora de inicio, hora de fin. | Identificador de asignación; alternativa: combinación de vehículo, fecha, turno e inicio, pendiente de lógico. | Entidad transaccional necesaria para guardar historial y responder la consulta por ruta/turno. |
| Inspección | Registrar revisión de condiciones de operación. | Fecha; responsable; resultado general si aplica; referencia al objeto inspeccionado pendiente. | Identificador de inspección, por definir. | El enunciado exige fecha, responsable, resultado e historial. |
| Condición revisada | Catalogar cada clase de condición que puede verificarse. | Tipo/denominación: condiciones laborales, parada autorizada, tecnología de recaudo. | Código o denominación, por definir. | Catálogo propuesto para evitar repetir textos libres al registrar resultados. |
| Evaluación de condición | Guardar el resultado de una condición dentro de una inspección. | Resultado; comentario/evidencia solo si se aprueba. | Inspección + condición, candidato conceptual. | Asociación transaccional propuesta D04; una inspección puede revisar varias condiciones. |
| Total de reportes por zona y periodo | Conservar el volumen agregado de reportes sobre transporte ilegal. | Periodo; total; categoría del reporte solo si se requiere. | Zona + periodo como candidato, sujeto a granularidad aprobada. | Unidad agregada exigida. No almacena persona ni reporte individual. |

## Catálogos y atributos frente a entidades

- **Capacidad** y **características de accesibilidad** describen un vehículo; no se proponen como entidades separadas en esta fase.
- **Orden** describe la relación entre una ruta y uno de sus puntos; pertenece a la asociación de paso de ruta, no al punto de servicio.
- **Fecha, hora de inicio y hora de fin** describen una asignación.
- **Fecha y responsable** describen una inspección.
- **Resultado** describe la evaluación de una condición y posiblemente un resumen de inspección. Se debe confirmar si existe también un resultado general.
- **Tipo de punto** podría ser un atributo categórico del punto de servicio; no se propone una jerarquía de clases o tablas físicas en esta fase.
- **Zona/sector** se mantiene como concepto geográfico candidato debido a que el enunciado usa ambos términos.
- **Conductor** no se propone como entidad de identificación personal. El enunciado advierte que sus datos deben protegerse, pero no exige conservar nombres ni documentos.

## Roles

Los actores confirmados son operadores de transporte público, responsables de flota y autoridades de movilidad. El “responsable” que aparece en cada inspección se conserva como referencia de rendición de cuentas; no se define aún si será un código de usuario, una persona inspectora o una unidad administrativa.

## Relaciones y cardinalidades propuestas

Cardinalidades propuestas para revisión; las que dependen de supuestos están señaladas.

| Relación | Cardinalidad propuesta | Atributos de relación | Comentario |
|---|---|---|---|
| Empresa opera Vehículo | Empresa 0..N — Vehículo 1..1 | Ninguno | Supuesto S01: un vehículo se asocia a una empresa a la vez. Confirmar si la relación cambia históricamente o puede ser múltiple. |
| Ruta incluye Punto de servicio | Ruta 1..N — Punto 0..N | Orden | Relación M:N resuelta conceptualmente por “Paso de ruta”. El paso programado debe pertenecer al conjunto autorizado (RN02). |
| Zona contiene Punto de servicio | Zona 0..N — Punto 1..1 | Ninguno | Propuesta S05; se debe validar zona/sector y si un punto puede quedar en límites de más de una zona. |
| Vehículo participa en Asignación | Vehículo 0..N — Asignación 1..1 | — | La asignación registra el historial del vehículo. |
| Ruta recibe Asignación | Ruta 0..N — Asignación 1..1 | — | Cada asignación apunta a una ruta. |
| Turno clasifica Asignación | Turno 0..N — Asignación 1..1 | — | Solo si se aprueba el turno como catálogo independiente. |
| Asignación es objeto de Inspección | Asignación 0..N — Inspección 0..1 | — | Propuesta S04 y pregunta Q01; podría sustituirse por inspección de empresa, ruta o vehículo. |
| Inspección revisa Condición | Inspección 1..N — Condición 0..N | Resultado por condición | M:N resuelta por “Evaluación de condición”; propuesta D04. |
| Zona agrupa Total de reportes | Zona 0..N — Total 1..1 | Periodo y total | El periodo/granularidad debe ser consistente; no se almacena detalle individual. |

## Reglas para las relaciones

1. Cada paso de ruta conserva el orden propuesto por la ruta.
2. Un punto programado en una ruta debe ser autorizado según RN02.
3. Una asignación identifica vehículo, ruta, fecha, turno e intervalo horario.
4. Un mismo vehículo no puede cumplir asignaciones temporalmente incompatibles conforme al alcance que se apruebe para RN01.
5. Los resultados de inspección deben poder conservarse históricamente.
6. Los totales de transporte ilegal se asocian a zona y periodo, sin identificar personas.

## Alternativa conceptual considerada

Se podría mantener “Paradero” y “Terminal de transferencia” como dos entidades independientes. Se propone inicialmente un solo “Punto de servicio” con atributo de tipo para compartir ubicación y orden de recorrido. Esta alternativa solo se acepta si un terminal puede efectivamente ser un punto de parada; de lo contrario, se separarán los conceptos antes de aprobar el modelo lógico.
