# Análisis semántico P11

**Proyecto:** Transporte público y mototaxismo: rutas, flota y cumplimiento  
**Versión:** 0.2 — conceptual revisado con respuestas del usuario, 2026-11-14  
**Estado:** Fase 1 aprobada anteriormente; decisiones Q01–Q03 incorporadas. El modelo lógico E5–E8 está en revisión y no aprobado.  
**SGBD:** aún no elegido. **No se ha generado ni probado SQL.**

## 1. Diagnóstico del problema

El sistema conservará información de empresas operadoras, vehículos y accesibilidad; rutas y puntos habilitados (paraderos y terminales); asignaciones históricas por ruta, vehículo y turno; inspecciones de empresas y vehículos; confirmaciones de servicios por sector; y totales agregados de reportes de transporte ilegal. Debe permitir consultas operativas y de cobertura sin identificar personas en reportes de transporte ilegal.

**Dentro del alcance:** flota, rutas, secuencia de puntos, asignaciones, inspecciones, condiciones verificadas, servicios confirmados por sector y agregados territoriales.  
**Fuera de alcance:** identificación de conductores, pasajeros o personas vinculadas a reportes de transporte ilegal; recaudo financiero detallado; almacenamiento del contenido de tokens o tickets.

## 2. Hechos confirmados por el enunciado y usuario

- Las empresas operadoras y los vehículos se registran con capacidad y características de accesibilidad.
- Las rutas incluyen paraderos habilitados y terminales de transferencia; el orden de los paraderos debe conservarse.
- Se guardan asignaciones a ruta por fecha y turno, con horas de inicio y fin, y su historial.
- Se registran inspecciones con fecha, responsable y resultado.
- Por aclaración del usuario, el objeto principal de inspección es **vehículo y empresa**. Se podrán guardar inspecciones asociadas a servicios/asignaciones, pero no son prioritarias.
- Se conservan condiciones laborales, uso de paradas autorizadas y tecnología de recaudo revisadas.
- Los reportes de transporte ilegal se almacenan solamente como totales agregados por **sector** y periodo, sin identificar personas.
- La cobertura de un sector se evalúa cuando un vehículo confirma allí la prestación del servicio. La evidencia de confirmación puede provenir de token o ticket de aplicación, o ticket impreso conservado.
- Una asignación simultánea puede permitirse si el vehículo está finalizando o ya finalizó otra asignación. La interpretación exacta del solapamiento y del estado al que aplica debe quedar validada antes de restricciones físicas.
- El usuario eligió Java para POO y solicita estilo básico, con polimorfismo solo si está justificado. Markdown durante el trabajo y Word al final.
- La guía exige justificar normalización hasta 3FN.

## 3. Inferencias, supuestos y dudas aún vigentes

| ID | Tipo | Tema | Propuesta de trabajo | Riesgo / decisión pendiente |
|---|---|---|---|---|
| S01 | Supuesto | Empresa–vehículo | Un vehículo pertenece a una empresa operadora en un momento dado. | Aclarar si el cambio de empresa requiere historial de propiedad/operación. |
| S02 | Supuesto | Terminales | Paradero y terminal son tipos de punto de servicio; un terminal puede servir como punto de transferencia dentro de una ruta. | Confirmar si todo terminal puede estar en la secuencia de ruta. |
| S03 | Supuesto | Confirmación y evidencia | Guardar tipo de evidencia y una referencia no personal; no guardar el token/ticket completo ni datos del usuario. | Confirmar si la referencia debe ser auditable o basta tipo y fecha. |
| S04 | Supuesto | Inspección | Usar vínculos diferenciados para vehículo, empresa y, opcionalmente, asignación. Una inspección puede tener uno o más objetivos. | Validar cómo se exigirá que cada inspección tenga objetivo y cómo se representa responsable. |
| S05 | Supuesto | Sector de un punto | Cada punto de servicio se ubica en un sector. | Confirmar si un punto puede pertenecer a varios sectores. |
| S06 | Supuesto | Cobertura | Un sector tiene cobertura dentro del periodo consultado si existe al menos una confirmación de servicio. | La ventana temporal debe ser indicada en cada consulta. |
| S07 | Supuesto | Autorización | Un paradero habilitado se modela como dato de autorización distinto del orden en que aparece en una ruta. | Falta precisar si la habilitación es global, por ruta y/o temporal. |
| S08 | Supuesto | Flota accesible | Calcular proporción respecto a la flota activa en una fecha de corte. | Definir cuándo un vehículo cuenta como activo. |
| D01 | Decisión usuario | Inspecciones | Inspecciones dirigidas a empresas y vehículos; las de servicios/asignaciones son posibles, secundarias. | Incorporada al lógico propuesto. |
| D02 | Decisión usuario | Territorio y cobertura | Usar “sector”; contar cobertura mediante confirmación de servicio por vehículo y evidencia tipo token/ticket. | Incorporada al lógico propuesto. |
| D03 | Decisión usuario | Solapamiento | Permitir simultaneidad bajo condición de finalización/finalizada. | Redacción temporal exacta por validar; no asumir regla física aún. |

## 4. Requisitos identificados

| ID | Requisito | Criterio observable |
|---|---|---|
| R01 | Registrar empresa, vehículo, capacidad y accesibilidad. | Una consulta enlaza cada vehículo con la empresa y muestra su accesibilidad. |
| R02 | Registrar rutas, puntos habilitados y terminales de transferencia. | Se identifican rutas, paraderos y terminales. |
| R03 | Conservar el orden de los puntos de cada ruta. | Cada ruta se muestra en secuencia sin ambigüedad. |
| R04 | Registrar asignaciones de vehículo a ruta, turno e intervalo. | Se recuperan asignaciones actuales e históricas. |
| R05 | Registrar inspecciones de empresa y vehículo; permitir inspección de servicio como extensión. | Se consultan fecha, responsable, objeto y resultados históricos. |
| R06 | Registrar resultados de condiciones inspeccionadas. | Se distingue el resultado por condición y por inspección. |
| R07 | Registrar confirmaciones de servicios por sector y evidencia. | La cobertura se deriva de confirmaciones válidas dentro de un periodo. |
| R08 | Guardar reportes ilegales solo como total por sector y periodo. | No hay registro de persona ni caso individual en el conjunto agregado. |
| R09 | Consultar vehículos por ruta y turno. | La consulta identifica las unidades asignadas según intervalo/fecha. |
| R10 | Calcular proporción de flota accesible. | Se informa numerador, denominador y fecha de corte. |
| R11 | Encontrar sectores/paraderos sin cobertura. | Se detectan sectores sin confirmación en el periodo indicado. |
| R12 | Consultar empresas cumplidoras y puntos de transbordo. | El cumplimiento se deriva de inspecciones y los terminales se reconocen como transbordos. |

## 5. Reglas de negocio

| ID | Regla | Estado / interpretación |
|---|---|---|
| RN01 | Una unidad no puede figurar en asignaciones temporalmente incompatibles salvo la excepción permitida por el usuario. | Revisada con D03; “finalizando” y “finalizó” deben especificarse temporalmente antes de Fase 3. |
| RN02 | Una parada programada debe estar habilitada. | Confirmada; alcance territorial y vigencia de la habilitación siguen pendientes. |
| RN03 | Proteger datos de conductores y mantener reportes ilegales sin identificar personas. | Confirmada; minimizar también referencias de tickets/tokens. |
| RN04 | Conservar historial de asignaciones e inspecciones. | Confirmada; corregir registros sin borrar el historial se detallará después. |
| RN05 | Un servicio solo acredita cobertura territorial cuando el vehículo lo confirma con evidencia admitida. | Nueva regla derivada directamente de la aclaración D02; no equivale a medir pasajeros. |

## 6. Revisión crítica de la propuesta previa

La propuesta inicial describía las inspecciones solo como posibles inspecciones de asignación, y sugería medir cobertura por presencia de puntos de ruta. Ambas opciones podían contradecir la aclaración del usuario: el objetivo principal de inspección es empresa/vehículo y la cobertura se basa en servicio confirmado por sector. Se corrigen en el catálogo, el diagrama, el modelo lógico y la matriz. Esta revisión evita afirmar cobertura solo porque un vehículo tenía un recorrido planeado.

## 7. Fuentes

La fuente primaria del alcance es el enunciado P11 pegado por el usuario y sus aclaraciones en esta conversación. Las referencias contextuales listadas en el enunciado (Alcaldía, BID y Santa Marta Cómo Vamos) no se han consultado ni se usan como fuente de cifras o reglas. No se incorporan estadísticas externas.

## 8. Checklist de fase

- [x] Enunciado recibido y problema separado del ejemplo P01.
- [x] Actores, alcance, reglas y consultas principales identificados.
- [x] Decisiones Q01–Q03 recibidas del usuario e incorporadas.
- [x] Evidencias conceptuales actualizadas como revisión, sin alterar el hecho histórico de que Fase 1 fue aprobada.
- [ ] Aprobación del modelo lógico E5–E8.
- [ ] Elegir SGBD solo después de la aprobación lógica.
- [ ] No se ha creado DDL, DML ni código Java.

**Historial:** v0.1 conceptual aprobado; v0.2 (2026-11-14), incorpora D01–D03 y revisa la cobertura de planificada a confirmada. Consecuencia: se agrega confirmación de servicio por sector y las inspecciones se enlazan a objetivos identificados.
