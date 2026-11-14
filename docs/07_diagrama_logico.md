# E7 · Diagrama lógico relacional P11

**Versión:** 0.1 — propuesta  
**Estado:** PROPUESTO; editable como Markdown/Mermaid.  
**Notación:** representación aproximada de pata de cuervo; no es Chen ni un DDL.

```mermaid
erDiagram
    EMPRESA_OPERADORA ||--o{ VEHICULO : opera
    SECTOR ||--o{ PUNTO_SERVICIO : contiene
    PUNTO_SERVICIO ||--o| PARADERO_HABILITADO : habilita
    RUTA ||--|{ PASO_RUTA : ordena
    PUNTO_SERVICIO ||--o{ PASO_RUTA : aparece_en
    VEHICULO ||--o{ ASIGNACION : recibe
    RUTA ||--o{ ASIGNACION : atiende
    TURNO ||--o{ ASIGNACION : clasifica
    INSPECCION ||--o{ INSPECCION_VEHICULO : tiene_objetivo
    VEHICULO ||--o{ INSPECCION_VEHICULO : inspeccionado
    INSPECCION ||--o{ INSPECCION_EMPRESA : tiene_objetivo
    EMPRESA_OPERADORA ||--o{ INSPECCION_EMPRESA : inspeccionada
    INSPECCION ||--o{ INSPECCION_ASIGNACION : tiene_objetivo_opcional
    ASIGNACION ||--o{ INSPECCION_ASIGNACION : inspeccionada_opcional
    INSPECCION ||--|{ EVALUACION_INSPECCION : evalua
    CONDICION ||--o{ EVALUACION_INSPECCION : se_revisa
    ASIGNACION ||--o{ CONFIRMACION_SERVICIO : confirma
    SECTOR ||--o{ CONFIRMACION_SERVICIO : reporta_servicio
    SECTOR ||--o{ REPORTE_ILEGAL_SECTOR_PERIODO : agrupa_total

    EMPRESA_OPERADORA {
        id_empresa PK
        nombre
    }
    VEHICULO {
        id_vehiculo PK
        id_empresa FK
        capacidad
        accesible
        caracteristicas_accesibilidad
    }
    SECTOR {
        id_sector PK
        nombre_codigo
    }
    PUNTO_SERVICIO {
        id_punto PK
        id_sector FK
        tipo
        ubicacion
    }
    PARADERO_HABILITADO {
        id_punto PK_FK
        datos_habilitacion
    }
    RUTA {
        id_ruta PK
        codigo_nombre
    }
    PASO_RUTA {
        id_ruta PK_FK
        orden PK
        id_punto FK
    }
    TURNO {
        id_turno PK
        nombre
    }
    ASIGNACION {
        id_asignacion PK
        id_vehiculo FK
        id_ruta FK
        id_turno FK
        inicio
        fin_programado
        estado
        fin_real
    }
    INSPECCION {
        id_inspeccion PK
        fecha_hora
        responsable
    }
    INSPECCION_VEHICULO {
        id_inspeccion PK_FK
        id_vehiculo PK_FK
    }
    INSPECCION_EMPRESA {
        id_inspeccion PK_FK
        id_empresa PK_FK
    }
    INSPECCION_ASIGNACION {
        id_inspeccion PK_FK
        id_asignacion PK_FK
    }
    CONDICION {
        id_condicion PK
        nombre
        categoria
    }
    EVALUACION_INSPECCION {
        id_inspeccion PK_FK
        id_condicion PK_FK
        resultado
        observacion
    }
    CONFIRMACION_SERVICIO {
        id_confirmacion PK
        id_asignacion FK
        id_sector FK
        fecha_hora
        tipo_evidencia
        referencia_no_personal
    }
    REPORTE_ILEGAL_SECTOR_PERIODO {
        id_sector PK_FK
        periodo_inicio PK
        periodo_fin PK
        total
    }
```

## Lectura del modelo

- Las tablas puente muestran las claves compuestas con `PK_FK`.
- `PASO_RUTA` conserva ordinal y punto. Su FK a `ParaderoHabilitado` dependerá de resolver alcance/vigencia de habilitación; el diagrama muestra solo los enlaces esenciales.
- `ConfirmacionServicio` no repite vehículo ni ruta: ambos se derivan de `Asignacion`.
- La inspección debe tener al menos un objetivo entre sus tres relaciones objetivo. El diagrama por sí solo no expresa esa validación entre tablas.
- La compatibilidad temporal de asignaciones tampoco se expresa en una relación; queda como RN01 para diseño físico.

## Checklist E7

- [x] Relaciones, PK y FK principales visibles.
- [x] Asociaciones M:N incluidas.
- [ ] Revisar habilitación de paraderos, asignación y la opcionalidad de objetivos de inspección.
- [ ] Aprobar diagrama junto al resto de Fase 2.
