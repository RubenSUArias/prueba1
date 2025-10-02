
```mermaid
erDiagram
    USUARIOS {
        BIGINT id_usuario PK
        VARCHAR login
        VARCHAR email
        VARCHAR clave
        ENUM rol
        TIMESTAMP fecha_registro
    }
    PROYECTOS {
        BIGINT id_proyecto PK
        VARCHAR nombre
        TEXT descripcion
        DATE fecha_inicio
        DATE fecha_fin
        VARCHAR responsable
        BIGINT created_by FK
        BIGINT updated_by FK
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }
    SECCIONES {
        BIGINT id_seccion PK
        VARCHAR nombre
        BIGINT created_by FK
        BIGINT updated_by FK
    }
    PROYECTOS_SECCIONES {
        BIGINT id_proyecto_seccion PK
        BIGINT id_proyecto FK
        BIGINT id_seccion FK
        BIGINT created_by FK
        BIGINT updated_by FK
    }
    CONCEPTOS {
        BIGINT id_concepto PK
        BIGINT id_proyecto_seccion FK
        INT numero
        TEXT descripcion
        BIGINT id_padre FK
    }
    ARCHIVOS {
        BIGINT id_archivo PK
        BIGINT id_concepto FK
        VARCHAR nombre_archivo
        VARCHAR ruta_archivo
        VARCHAR tipo
        INT version
    }
    ESTATUS_CONCEPTOS {
        BIGINT id_estatus PK
        BIGINT id_concepto FK
        ENUM estatus
        TEXT observaciones
    }

    USUARIOS ||--o{ PROYECTOS : "crea"
    USUARIOS ||--o{ SECCIONES : "crea"
    PROYECTOS ||--o{ PROYECTOS_SECCIONES : "tiene"
    SECCIONES ||--o{ PROYECTOS_SECCIONES : "pertenece"
    PROYECTOS_SECCIONES ||--o{ CONCEPTOS : "contiene"
    CONCEPTOS ||--o{ ARCHIVOS : "adjunta"
    CONCEPTOS ||--o{ ESTATUS_CONCEPTOS : "estatus"
