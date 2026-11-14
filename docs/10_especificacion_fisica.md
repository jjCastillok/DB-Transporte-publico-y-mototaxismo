# E10 · Especificación física propuesta para MariaDB

**Proyecto:** Transporte público y mototaxismo: rutas, flota y cumplimiento  
**Versión:** 0.1 — diseño físico inicial, 2026-11-14  
**Estado:** PROPUESTO; pendiente de revisión/aprobación.  
**SGBD elegido:** MariaDB.  
**Versión objetivo propuesta:** MariaDB 11.8.9 LTS. No se ha verificado qué versión está instalada en el equipo. La versión debe cotejarse con `SELECT VERSION();` antes de generar o ejecutar el DDL.  
**Cliente mencionado por el usuario:** Workbench; producto y versión exactos pendientes. La definición y compatibilidad final se verifican contra MariaDB, no por el nombre del editor.  
**Ejecución:** ninguna. No hay esquema creado ni DDL probado.

## Propósito

Traducir el modelo lógico aprobado a tipos y reglas de MariaDB, conservando nombres, relaciones, privacidad e historial. Esta especificación no crea objetos. Las longitudes y varios dominios son propuestas físicas, no hechos del enunciado ni datos observados.

## Decisiones físicas propuestas

| Tema | Propuesta | Motivo y límite |
|---|---|---|
| Versión | MariaDB 11.8.9 LTS como objetivo revisable; confirmar la versión instalada con `SELECT VERSION();`. | La Fundación MariaDB lista la serie 11.8 como LTS y 11.8.9 como una actualización estable de 2026. No se infiere que esté instalada. |
| Motor de tablas | InnoDB para todas las relaciones. | Necesario para soportar claves foráneas y transacciones en las tablas del modelo. |
| Esquema | `transporte_publico_mototaxismo` como nombre de trabajo, sujeto a aprobación. | Identifica el proyecto y no modifica ninguna base existente. |
| Identificadores | `BIGINT UNSIGNED AUTO_INCREMENT` para claves técnicas simples. | Da un rango amplio y evita generar identificadores manualmente. Las FK deben repetir exactamente tipo y signo del padre. |
| Texto | `utf8mb4` y collation explícita `utf8mb4_unicode_ci` como opción conservadora. | Permite almacenar español y evita depender del valor predeterminado local, que puede variar entre instalaciones. Confirmar que la collation figure en `INFORMATION_SCHEMA.COLLATIONS` de la versión instalada. |
| Fechas y horas | `DATE` para los límites de reportes; `DATETIME` para eventos e intervalos. Convención de entrada y presentación: hora local de Bogotá. | `DATETIME` conserva el valor de pared y no convierte por zona horaria; la aplicación y las sesiones deben respetar una convención única. No utilizar la fecha del servidor como sustituto del tiempo del evento. |
| Booleanos | `BOOLEAN` para `Vehiculo.accesible`, con verificación de valores 0/1. | En MariaDB `BOOLEAN` es un alias numérico; la condición CHECK documentará el dominio binario. |
| Capacidad | `SMALLINT UNSIGNED`, mayor que cero. | Cantidad entera de plazas, no dinero. El rango máximo es una decisión técnica propuesta, no un aforo oficial. |
| Conteo agregado | `BIGINT UNSIGNED` para `total`. | Evita valores negativos y reduce el riesgo de desbordamiento en agregados. |
| Claves candidatas | No declarar UNIQUE sobre nombres/códigos todavía. | La unicidad de códigos de sector/ruta y nombres no está confirmada por el negocio. |
| Comentarios | Comentarios semánticos en tablas y columnas mediante la opción `COMMENT` del dialecto; explicación también en el DDL para claves, índices y reglas. | La forma exacta y el límite de longitud deben cotejarse en MariaDB 11.8.9; un comentario del archivo no equivale a metadato persistido. |
| Eliminaciones y actualizaciones | `ON DELETE RESTRICT` y `ON UPDATE RESTRICT` como propuesta para relaciones históricas. | Evita borrar en cascada asignaciones, inspecciones, confirmaciones y la evidencia que dependa de entidades maestras. Revisar cada FK antes del DDL final. |
| Valores por defecto | Ningún estado o resultado recibe DEFAULT implícito. | Los valores posibles de estado y resultado siguen pendientes; no inventar una condición inicial. Las claves AUTO_INCREMENT son la única generación automática propuesta. |
| Índices | PK y los índices que MariaDB requiere para FK; añadir solo los índices de consulta listados abajo. | Evita crear dos veces el índice que ya respalda una FK. El beneficio real se comprobará luego con `EXPLAIN`, no se afirma rendimiento. |

## Mapeo de relaciones y restricciones

`09_diccionario_fisico.csv` contiene una fila por columna, tipo, nulabilidad, valor predeterminado, clave/restricción, significado, trazabilidad y estado. El resumen de claves físicas propuesto es:

| Tabla | PK | Restricciones adicionales principales |
|---|---|---|
| EmpresaOperadora | `id_empresa` | — |
| Vehiculo | `id_vehiculo` | FK empresa; capacidad positiva; accesible binario |
| Sector | `id_sector` | Códigos y unicidad pendientes |
| PuntoServicio | `id_punto` | FK sector; tipo PARADERO/TERMINAL |
| ParaderoHabilitado | `id_punto` | PK también FK a PuntoServicio |
| Ruta | `id_ruta` | Código/uniqueness pendientes |
| PasoRuta | (`id_ruta`, `orden`) | FK Ruta y FK `id_punto` a ParaderoHabilitado |
| Turno | `id_turno` | — |
| Asignacion | `id_asignacion` | FK vehículo/ruta/turno; inicio < fin_programado; `fin_real` NULL o posterior al inicio |
| Inspeccion | `id_inspeccion` | Tres FK objetivo nullable; CHECK lógico de exactamente una referencia no nula |
| Condicion | `id_condicion` | Dominio de categoría pendiente |
| EvaluacionInspeccion | (`id_inspeccion`, `id_condicion`) | FKs; dominio de resultado pendiente |
| ConfirmacionServicio | `id_confirmacion` | FKs a asignación y sector; tipo de evidencia limitado; referencia opcional no personal |
| ReporteIlegalSectorPeriodo | (`id_sector`, `periodo_inicio`, `periodo_fin`) | Total no negativo; fin no anterior al inicio |

Las expresiones `CHECK` para capacidad, valores binarios, orden de fechas, exclusividad del objetivo de inspección, tipo de punto, tipo de evidencia y periodo son propuestas. MariaDB documenta restricciones `CHECK` aplicadas; aun así, se validará la versión real y el modo de servidor antes de usarla. La regla temporal de asignación requiere comparar otras filas y no se puede sustituir por un CHECK de fila.

## Índices secundarios propuestos

- `Asignacion(id_ruta, id_turno, inicio)`: consulta de vehículos por ruta/turno/fecha. La PK e índices de FK conservan las búsquedas por identificadores individuales.
- `Asignacion(id_vehiculo, inicio, fin_programado)`: localizar intervalos candidatos al validar solapamientos de un vehículo.
- `Inspeccion(id_vehiculo, fecha_hora)` y `Inspeccion(id_empresa, fecha_hora)`: historial por objetivo y fecha; revisar si el índice simple de cada FK se vuelve redundante al crear el compuesto.
- `ConfirmacionServicio(id_sector, fecha_hora)`: consulta de cobertura de sectores en una ventana de tiempo.
- `EvaluacionInspeccion(id_condicion)`: búsquedas desde una condición hacia sus evaluaciones; la PK compuesta ya inicia por inspección.

El índice secundario de FK no se agregará duplicado si un índice compuesto ya comienza por la misma columna. En la fase de consultas se medirá si estos índices ayudan; quitar o añadir índices después requerirá revisión del diseño y aprobación.

## Reglas que requieren lógica fuera de un CHECK sencillo

1. **Solapamiento de asignaciones (RN01):** la decisión depende de intervalos, estado `FINALIZANDO`/`FINALIZADA` y otras filas. Se propone validar en una transacción de aplicación que serialice las asignaciones del mismo vehículo (por ejemplo, bloqueo de la fila Vehiculo y búsqueda de intervalos candidatos) antes de guardar una nueva asignación. No se declara suficiente un trigger sin resolver concurrencia. Falta fijar catálogo de estados, el evento que define “finaliza” y la frontera horaria.
2. **Confirmación dentro del intervalo real del servicio:** validar que la fecha de `ConfirmacionServicio` corresponda al intervalo/estado de la asignación; la regla está pendiente de precisión.
3. **Cobertura:** el modelo responde cobertura por Sector y ventana temporal a partir de `ConfirmacionServicio`. No permite asegurar cobertura por cada paradero individual.
4. **Cumplimiento empresarial:** requiere una regla agregada acordada sobre condiciones y resultados; no se codifica un umbral inventado.
5. **Privacidad:** acceso a la referencia protegida de responsable y evidencia debe limitarse en aplicación/roles. No registrar nombres o datos de conductores, tokens completos ni información personal en reportes agregados.

## Pendientes de negocio/compatibilidad antes del DDL

- Obtener la versión instalada con `SELECT VERSION();`; comparar collations, CHECK, comentarios, AUTO_INCREMENT y metadatos disponibles.
- Confirmar nombre final del esquema y producto/versión de Workbench si se quiere documentar el cliente específico.
- Cerrar si los terminales también pueden ocupar pasos de ruta; si sí, la relación llamada ParaderoHabilitado puede no representar todos los puntos autorizados.
- Definir cómo expresar varias características de accesibilidad sin guardar listas en una sola columna.
- Precisar estado de asignación y la simultaneidad durante FINALIZANDO, manteniendo la regla aprobada por el usuario.
- Definir referencia protegida del responsable, niveles de resultado/condición, alcance temporal de reportes y si códigos tienen unicidad.

## Referencias técnicas revisadas

- [MariaDB Foundation: mantenimiento y versiones LTS](https://mariadb.org/about/).
- [MariaDB Server: restricciones CHECK, PK, FK y UNIQUE](https://mariadb.com/docs/server/reference/sql-statements/data-definition/constraint).
- [MariaDB Server: claves foráneas y soporte InnoDB](https://mariadb.com/docs/server/architecture/server-constraints/foreign-key-constraints).
- [MariaDB Server: AUTO_INCREMENT](https://mariadb.com/docs/server/reference/data-types/auto_increment).
- [MariaDB Server: CREATE TABLE y opción COMMENT](https://mariadb.com/docs/server/server-usage/tables/create-table).
- [MariaDB Server: conjuntos de caracteres y collations](https://mariadb.com/docs/server/reference/data-types/string-data-types/character-sets/supported-character-sets).
- [MariaDB Server: DATETIME y TIMESTAMP](https://mariadb.com/docs/server/mariadb-quickstart-guides/mariadb-datetime-guide).
- [MySQL Workbench Manual: alcance del cliente](https://dev.mysql.com/doc/workbench/en/); su documentación lo describe como diseñado y probado con MySQL Server. No se toma eso como garantía de que todas las funciones del cliente operen con MariaDB.

## Por qué se modeló así

Se eligen tipos numéricos exactos para identificadores y conteos, fechas explícitas para intervalos, y claves foráneas con restricciones para preservar el historial. Las reglas que dependen de filas existentes se dejan como lógica transaccional identificada, no se simulan como restricciones que el motor no puede evaluar por fila.

## Alternativa descartada

No se propone `ENUM` para estados ni `SET` para características: los valores de negocio aún no están cerrados, y ampliar un catálogo real debe ser trazable. Se propone `VARCHAR` con catálogo/regla por confirmar o una relación normalizada cuando el atributo sea multivaluado.

## Checklist E10

- [x] Motor elegido y versión objetivo propuesta.
- [x] Tipos, tamaños, codificación, collation, nulabilidad y generación de claves propuestos.
- [x] FK, acciones referenciales, restricciones e índices candidatos identificados.
- [x] Reglas fuera de CHECK y limitaciones de privacidad documentadas.
- [x] Referencias técnicas oficiales revisadas.
- [ ] Versión instalada y collation verificadas en servidor.
- [ ] Decisiones pendientes de negocio cerradas o anotadas como PENDIENTE DE CONFIRMAR en DDL.
- [ ] Aprobación de la Fase 3.
- [ ] Sin DDL, DML ni ejecución en servidor en esta fase.

