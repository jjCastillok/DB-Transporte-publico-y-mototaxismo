# E11 · Verificación previa del diseño físico

**Proyecto:** Transporte público y mototaxismo: rutas, flota y cumplimiento  
**Versión:** 0.1 — plan estático previo, 2026-11-14  
**Estado:** REVISADO ESTÁTICAMENTE; no probado en servidor.  
**SGBD:** MariaDB, objetivo propuesto 11.8.9 LTS. La instalación real y la versión de Workbench no se han consultado.

## Alcance

Este archivo registra compatibilidad documental y los pasos que deberán ejecutarse antes del DDL aprobado. No se conectó a una instancia MariaDB, no se creó esquema ni tabla, no se ejecutó SQL contra un servidor y no se declara estado **PROBADO EN SGBD**.

## Compatibilidad revisada estáticamente

| Aspecto | Resultado documental | Evidencia que falta en el equipo |
|---|---|---|
| MariaDB y CHECK | La referencia actual de MariaDB documenta restricciones `CHECK` como restricciones evaluadas al insertar/actualizar. | Confirmar `SELECT VERSION()` y ejecutar una prueba positiva/negativa en una BD de prueba después de aprobar DDL. |
| FK e InnoDB | El motor InnoDB es candidato para las relaciones con FK e historial. | Comprobar `SHOW ENGINES` en la instancia y que InnoDB esté habilitado. |
| Unicode/collation | Se propone `utf8mb4_unicode_ci`; no depender del default de instalación. | Buscarla en `INFORMATION_SCHEMA.COLLATIONS` y revisar `@@character_set_server`/`@@collation_server`. |
| DATETIME | Convención propuesta: hora de Bogotá, `DATETIME`, misma convención para cliente y servidor. | Confirmar el flujo de la app y consultar `@@time_zone`/`@@system_time_zone`; evitar conversiones implícitas. |
| Comentarios | MariaDB documenta comentarios en tabla/columna. La persistencia y longitud deben probarse en la versión objetivo. | Comparar `SHOW CREATE TABLE` e `INFORMATION_SCHEMA.COLUMNS` después del DDL. |
| AUTO_INCREMENT | Propuesto para claves técnicas simples. | Confirmar generación y metadatos del campo, sin depender del editor visual. |
| Acciones FK | `RESTRICT` es la acción candidata para proteger referencias de historial. | Inspeccionar `INFORMATION_SCHEMA.REFERENTIAL_CONSTRAINTS` y casos de borrado/actualización con registros sintéticos aprobados. |
| Workbench | El producto no está identificado. La documentación oficial de MySQL Workbench se refiere a MySQL Server como el servidor para el que fue desarrollado y probado; no garantiza todas las funciones con MariaDB. | Revisar nombre y versión del programa. Confirmar si conexión, editor SQL, importación de modelo y reverse engineering que se necesiten funcionan. El SQL del proyecto será la referencia principal. |

## Consultas de reconocimiento para el usuario

Ejecutar manualmente en una conexión a la instancia de desarrollo, sin enviar credenciales ni datos privados:

```sql
SELECT VERSION() AS version_servidor;
SELECT @@version_comment AS producto;
SELECT @@character_set_server AS charset_servidor,
       @@collation_server AS collation_servidor,
       @@time_zone AS zona_sesion,
       @@system_time_zone AS zona_sistema,
       @@sql_mode AS modos_sql;
SHOW ENGINES;
SELECT COLLATION_NAME, CHARACTER_SET_NAME
FROM INFORMATION_SCHEMA.COLLATIONS
WHERE COLLATION_NAME = 'utf8mb4_unicode_ci';
```

Estas consultas son de lectura. El resultado requerido para el registro es únicamente la versión/producto, disponibilidad de InnoDB y disponibilidad de la collation; no compartir credenciales, nombres de usuario, direcciones de conexión ni registros reales.

## Lista para revisar antes de aprobar DDL

- [ ] Elegir si se mantiene la versión objetivo recomendada 11.8.9 o se adapta a la versión instalada.
- [ ] Confirmar que el esquema de destino es nuevo/vacío; la creación DDL posterior no debe apuntar por accidente a una base con datos.
- [ ] Confirmar engine InnoDB y collation disponible.
- [ ] Acordar si terminales pueden formar parte de los pasos de ruta o si solo los paraderos pasan por `ParaderoHabilitado`.
- [ ] Cerrar dominios de `estado`, `resultado`, y categorías de condición.
- [ ] Decidir si accesibilidad requiere varias características normalizadas y cómo se conserva la referencia protegida de responsable.
- [ ] Precisar regla de solapamiento: estado de la asignación que termina, marca `fin_real` y momento en que puede iniciar la nueva.
- [ ] Confirmar que cobertura consultada seguirá siendo por sector, sin inferir atención individual de cada paradero.
- [ ] Revisar límites físicos propuestos (`VARCHAR`, `SMALLINT`) y claves candidatas/unicidad antes del DDL.
- [ ] Revisar orden de creación por dependencias de FK y orden de índices; no ejecutar hasta aprobación de E9–E11.

## Matriz breve de trazabilidad física

| Requisito/regla | Relaciones | Elemento físico propuesto | Revisión / evidencia por obtener |
|---|---|---|---|
| R01 flota, empresa, capacidad y accesibilidad | EmpresaOperadora, Vehiculo | BIGINT, FK, capacidad positiva, booleano; características pendientes | Versión, CHECK y acceso; después, prueba en servidor |
| R02 puntos y sectores | Sector, PuntoServicio | FK de sector; tipo controlado | Revisar dominio, collation y alcance de terminal |
| R03–R04 ruta y secuencia | Ruta, PasoRuta, ParaderoHabilitado | PK compuesta por ruta/orden y FK hacia el conjunto habilitado | Confirmar si terminal está incluido; revisar que no haya FK duplicada |
| R05/RN01 asignación e historial | Asignacion, Vehiculo, Ruta, Turno | DATETIME y FK; índices de consulta/intervalo | Transacción/solapamiento necesita decisión y prueba concurrente posterior |
| R06–R07 inspección/condiciones | Inspeccion, Condicion, EvaluacionInspeccion | CHECK de exactamente un objetivo; dominios pendientes; referencia protegida | Validar enforcement y política de acceso |
| R08 confirmación real | ConfirmacionServicio, Asignacion, Sector | FK, DATETIME, tipo evidencia permitido, sin ticket/token bruto | Confirmar integridad temporal y no guardar secreto |
| R09 privacidad del transporte ilegal | ReporteIlegalSectorPeriodo | Clave compuesta sector-periodo, total entero no negativo | Verificar que el modelo no agregue personas ni detalle individual |
| Consultas de cobertura | Sector, ConfirmacionServicio | Índice candidato sector-fecha | No se deduce cobertura por paradero con una confirmación sectorial |

## Riesgos/decisiones sin aplicar

- La versión de MariaDB no viene confirmada por el usuario ni por conexión local.
- `ParaderoHabilitado` no aclara completamente si un terminal puede ser un paso de ruta.
- Una lista textual de características accesibles puede romper atomicidad si el contenido representa varias categorías; no usar valores separados por comas.
- El estado `FINALIZANDO` y la comprobación de intervalos requieren definición de negocio y lógica transaccional; no crear un trigger antes de decidir cómo manejar concurrencia.
- La referencia del responsable y de la evidencia debe ser opaca y no contener nombre, token, ticket íntegro ni datos de conductor.
- Los índices son candidatos derivados de consultas previstas. Sin volumen de datos y `EXPLAIN` no se afirma que mejoren rendimiento.

## Evidencia y checklist E11

- [x] Matriz de verificación de motor/versión, engine, collation, fecha/hora, comentarios y Workbench.
- [x] Consultas de reconocimiento de solo lectura documentadas.
- [x] Compatibilidad normativa revisada en documentación oficial y estado estático separado de prueba real.
- [x] Reglas que no se resuelven con CHECK anotadas.
- [ ] Versión de la instancia cotejada por el usuario.
- [ ] Decisiones pendientes de modelo cerradas o aceptadas como PENDIENTE DE CONFIRMAR.
- [ ] Diseño físico aprobado.
- [ ] DDL creado/probado — reservado para fase posterior a la aprobación de E9–E11.

## Fuentes técnicas

- [MariaDB: restricciones y enforcement](https://mariadb.com/docs/server/reference/sql-statements/data-definition/constraint).
- [MariaDB Foundation: series LTS y mantenimiento](https://mariadb.org/about/).
- [MariaDB: collations disponibles](https://mariadb.com/docs/server/reference/data-types/string-data-types/character-sets/supported-character-sets).
- [MariaDB: comportamiento de fecha y hora](https://mariadb.com/docs/server/mariadb-quickstart-guides/mariadb-datetime-guide).
- [MySQL Workbench Manual: requisitos y servidor MySQL](https://dev.mysql.com/doc/workbench/en/wb-requirements.html).

## Por qué se modeló así

Separar el plan de reconocimiento de un script DDL permite identificar la versión y capacidades reales antes de crear objetos. Las consultas aquí son solo lectura y no necesitan una base del proyecto existente.

## Alternativa descartada

No se propone usar funciones propias de MySQL como si fueran automáticamente compatibles con MariaDB, ni aceptar el resultado visual de Workbench como prueba de integridad. Se validará el dialecto en el servidor MariaDB elegido.

