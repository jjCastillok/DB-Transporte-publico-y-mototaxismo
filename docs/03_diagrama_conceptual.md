# E3 · Diagrama conceptual P11

**Versión:** 0.2 — conceptual revisado el 2026-11-14.  
**Notación de la especificación:** Chen textual. El archivo editable `03_diagrama_conceptual.drawio` acompaña esta descripción. La vista relacional posterior es pata de cuervo y se rotula aparte.

## Entidades principales

EMPRESA OPERADORA, VEHÍCULO, RUTA, SECTOR, PUNTO DE SERVICIO, PARADERO HABILITADO, PASO DE RUTA, TURNO, ASIGNACIÓN, INSPECCIÓN, CONDICIÓN, EVALUACIÓN DE INSPECCIÓN, CONFIRMACIÓN DE SERVICIO y REPORTE ILEGAL AGREGADO.

## Relaciones Chen, cardinalidades y atributos

1. **EMPRESA — OPERA — VEHÍCULO:** una empresa puede operar cero o muchos vehículos; cada vehículo se relaciona con una empresa operadora en un momento dado (S01, pendiente historial de cambios).
2. **RUTA — CONTIENE — PASO DE RUTA — UBICA — PUNTO:** una ruta tiene uno o más pasos y cada paso ocupa una posición ordinal única en esa ruta; un punto puede aparecer en cero o muchos pasos. El paso programado debe estar habilitado (RN02; alcance de autorización pendiente S07).
3. **SECTOR — CONTIENE — PUNTO DE SERVICIO:** un sector puede contener cero o muchos puntos y cada punto se ubica en un sector (S05, pendiente confirmar pertenencia múltiple).
4. **VEHÍCULO — PARTICIPA EN — ASIGNACIÓN — ATIENDE — RUTA:** cada asignación tiene un vehículo, ruta y turno, y conserva intervalo/estado; cada vehículo y ruta puede tener muchas asignaciones históricas.
5. **TURNO — CLASIFICA — ASIGNACIÓN:** un turno clasifica cero o muchas asignaciones; cada asignación pertenece a un turno si turno es catálogo. Si el curso lo trata solo como nombre, podría ser atributo.
6. **INSPECCIÓN — TIENE OBJETIVO — VEHÍCULO / EMPRESA:** por decisión del usuario, ambos son objetivos principales; el vínculo a ASIGNACIÓN/servicio se permite como objetivo opcional secundario. Cada inspección debe tener al menos un objetivo; validar cardinalidad final.
7. **INSPECCIÓN — EVALÚA — CONDICIÓN:** una inspección incluye evaluaciones de condiciones; cada evaluación registra su resultado. Se puede evaluar una condición en muchas inspecciones.
8. **ASIGNACIÓN — GENERA — CONFIRMACIÓN DE SERVICIO — OCURRE EN — SECTOR:** una asignación puede tener confirmaciones en cero o muchos sectores. La confirmación conserva fecha/hora y tipo de evidencia (token/ticket de aplicación o ticket impreso); no se asocia a identidad del pasajero.
9. **SECTOR — AGRUPA — REPORTE ILEGAL AGREGADO:** cada total corresponde a un sector y periodo; el sistema guarda cantidades agregadas sin individuos.

## Reglas e interpretación

- La cobertura del sector se cuenta solo con una confirmación de servicio en el periodo consultado. La ruta planeada por sí sola no confirma cobertura.
- La regla de asignación simultánea admite la excepción descrita por el usuario cuando otra asignación está finalizando o finalizó. Hace falta fijar el instante/estado que permite iniciar la siguiente.
- La habilitación de parada y la secuencia de ruta son conceptos relacionados, pero diferentes: habilitar no determina el orden.
- No se define jerarquía de clases en el conceptual. El futuro código Java usará polimorfismo solo si se valida un comportamiento distinto para tipos de punto.

## Historial de edición

v0.1 listaba Q01–Q03 abiertas y trataba asignación como posible único objeto de inspección. v0.2 incorpora las respuestas del usuario: inspección principal de empresa/vehículo, sector por servicio confirmado y solapamiento condicionado a finalización. La precisión de la regla temporal permanece pendiente.
