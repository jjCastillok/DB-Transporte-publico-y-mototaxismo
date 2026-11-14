# Gestión del código SQL · MariaDB

**Proyecto:** Transporte público y mototaxismo: rutas, flota y cumplimiento  
**Versión:** 0.1 — guía de organización, 2026-11-14  
**Estado:** PROPUESTO; el DDL queda pendiente de aprobación de Fase 3.  
**Herramienta de edición:** el usuario dispone de varios entornos; no se impone uno. Workbench podrá usarse como cliente después de identificar producto y versión.

## Propósito

Definir cómo se guardará, revisará y ejecutará el SQL del proyecto para que cada archivo tenga una finalidad clara, historial de cambios y criterios de seguridad. Esta guía no contiene DDL ejecutable ni credenciales.

## Estructura prevista del repositorio

```text
sql/
  12_creacion_esquema_mariadb.sql       # esquema vacío; tras aprobación del DDL
  12b_migracion_solo_comentarios_mariadb.sql  # solo si se conoce el esquema real
  13_verificaciones_estructura_mariadb.sql   # consultas de catálogo/estructura
  14_carga_datos_sinteticos_mariadb.sql       # DML aparte, con aprobación
  15_verificaciones_carga_mariadb.sql         # conteos e integridad
  16_consultas_validacion_mariadb.sql         # consultas funcionales
```

Los nombres y la secuencia siguen el Prompt Maestro. Se crearán cuando cada puerta de aprobación permita su contenido; no se crearán archivos vacíos que aparenten implementación.

## Convenciones sencillas

- Un archivo, una responsabilidad: creación de esquema, migración documental, carga sintética y consultas van separados.
- Nombre de objeto en español, sin tildes ni espacios, estable y descriptivo; por ejemplo `ConfirmacionServicio` en el modelo se convierte en `confirmacion_servicio` en MariaDB solo si el cambio de nomenclatura se aprueba y queda trazado.
- Palabras clave SQL en mayúsculas y un objeto por bloque comentado. Cada tabla y columna tendrá un `COMMENT` semántico si MariaDB lo soporta; cada restricción e índice tendrá comentario visible en el script.
- Las PK/FK y los tipos de FK deben coincidir en tipo y signo. No se desactivan controles referenciales para forzar una carga.
- Los cambios se publican en commits de alcance breve: `SCRUM-<id> docs|sql: <acción concreta>`. Jira queda pausado; la clave sirve como identificador histórico, no como sincronización automática.
- No guardar contraseñas, credenciales de conexión, tokens activos ni datos personales en el repositorio. Los archivos de entorno local, si se usan, se excluyen con `.gitignore` y nunca se comparten por chat.

## Cómo se usará un cliente SQL

1. Antes de escribir o ejecutar, conectar solo a una instancia de desarrollo que el usuario reconozca.
2. Consultar `SELECT VERSION();` y confirmar que la conexión responde a MariaDB.
3. Leer el archivo completo, confirmar su estado y revisar que el destino sea una base vacía de prueba.
4. Ejecutar únicamente el script cuya fase esté aprobada; no pegar scripts de DML dentro del DDL.
5. Guardar evidencia del resultado y cualquier error sin incluir contraseña, host privado, registros reales o tokens.
6. Comparar el catálogo del servidor con la verificación documentada. “Ejecutado” y “probado” solo se escriben después de ver evidencia real.

Workbench es una interfaz, no el motor. El diseño objetivo es MariaDB; un cliente hecho principalmente para MySQL puede no reconocer todas las funciones de MariaDB. Si alguna función visual falla, validar el dialecto con el servidor MariaDB y un cliente compatible, no cambiar silenciosamente la especificación.

## Tratamiento del ticket/token de entrada y salida

Solicitud pendiente P11-CR-01:

- el ticket/token debe corresponder a un vehículo y a una asignación, no a una persona;
- cada evento debe tener una hora y tipo definido (entrada/salida), pero el usuario debe decidir si representa inicio/fin del servicio, control en terminal/patio o ambos;
- no guardar token secreto en texto claro ni publicar tokens en GitHub; almacenar un hash y entregar el secreto una sola vez al emitirlo;
- los tickets impresos se vinculan con una referencia opaca;
- no generar todavía tokens reales, tablas, triggers o procedimientos. Primero se actualiza y aprueba el modelo lógico y físico.

## Estados de trabajo

| Entregable | Estado |
|---|---|
| E5–E8 modelo lógico | Aprobado por usuario; cambios del ciclo entrada/salida pendientes de aclaración |
| E9–E11 diseño físico MariaDB | Propuesto; pendiente de revisión/aprobación y cotejo de versión |
| DDL documentado | No creado; requiere aprobación de Fase 3 |
| Ticket/token de servicio | Cambio solicitado; semántica de entrada/salida pendiente |
| Datos sintéticos | No creados; requieren DDL aprobado y puerta de datos |

## Por qué se organiza así

Separar código ejecutable, datos de prueba, migración documental y consultas reduce la posibilidad de ejecutar una carga de datos al crear el esquema o alterar por accidente una base existente. Los commits pequeños hacen posible identificar qué cambió y por qué.

## Revisión crítica

Workbench puede conectarse a servidores de la familia MySQL, pero no se asumirá que el modelador, reverse engineering, exportación y características específicas funcionen igual con cada versión MariaDB. Se hará una prueba controlada de lectura/conexión cuando el usuario identifique su cliente y versión, sin trasladar datos reales.

## Próximo paso

Responder P11-CR-01-Q1, revisar E9–E11 y aprobar o ajustar Fase 3. Solo después se prepara `sql/12_creacion_esquema_mariadb.sql` completamente comentado, y se solicitará su aprobación antes de crear DML.

