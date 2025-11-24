erDiagram

    %% ============================
    %% USUARIOS, ROLES, PERMISOS
    %% ============================

    usuarios {
        int id PK
        text nombre
        text email
        text password_hash
        int id_rol FK
        int created_by
        timestamp created_at
        int updated_by
        timestamp updated_at
    }

    roles {
        int id PK
        text rol
        text descripcion
        int created_by
        timestamp created_at
        int updated_by
        timestamp updated_at
    }

    permisos {
        int id PK
        text codigo
        text descripcion
    }

    roles_permisos {
        int id_rol FK
        int id_permiso FK
    }

    user_sessions {
        int id PK
        int user_id FK
        text jwt
        timestamp issued_at
        timestamp expires_at
        text ip_address
        text user_agent
        boolean revoked
    }

    bitacora_login {
        int id PK
        int user_id FK
        timestamp fecha
        text ip
        boolean exito
        int intentos_fallidos
    }

    %% ============================
    %% AUDITORÍA
    %% ============================

    audit_log {
        bigint id PK
        text tabla
        int registro_id
        text accion
        jsonb valores_antes
        jsonb valores_despues
        int user_id FK
        timestamp fecha
    }


    %% ============================
    %% CATÁLOGOS Y PLANTILLAS
    %% ============================

    cat_campos {
        int id PK
        text campo
        text tipo
        text descripcion
        text regex_validacion
        boolean visible
        boolean solo_lectura
        jsonb opciones
        int created_by
        timestamp created_at
        int updated_by
        timestamp updated_at
    }

    cat_plantillas {
        int id PK
        text plantilla
        text descripcion
        text empresas
        text usuarios
        text estado
        int version
        int created_by
        timestamp created_at
        int updated_by
        timestamp updated_at
    }

    rel_campos_plantilla {
        int id PK
        int id_cat_campos FK
        int id_cat_plantillas FK
        int orden
        boolean obligatorio
        int created_by
        timestamp created_at
        int updated_by
        timestamp updated_at
    }


    %% ============================
    %% DOCUMENTOS (KARDEX)
    %% ============================

    kardex {
        int id PK
        date fecha
        text titulo
        text ubicacion
        int id_cat_plantilla FK
        int padre FK
        text estatus
        int nivel_acceso
        jsonb metadata
        int created_by
        timestamp created_at
        int updated_by
        timestamp updated_at
    }

    data {
        int id PK
        int kardex FK
        int id_rel_campo_plantilla FK
        text valor_texto
        int valor_numero
        date valor_fecha
        jsonb valor_json
        int created_by
        timestamp created_at
        int updated_by
        timestamp updated_at
    }


    %% ============================
    %% ARCHIVOS Y VERSIONADO
    %% ============================

    archivos {
        int id PK
        int id_kardex FK
        text nombre_archivo
        text tipo_mime
        text ruta_storage
        bigint tamaño
        jsonb metadata
        int created_by
        timestamp created_at
    }

    archivos_versiones {
        int id PK
        int id_archivo FK
        int version
        text ruta_storage
        timestamp created_at
        int created_by
    }


    %% ============================
    %% WORKFLOW (TAREAS)
    %% ============================

    tareas_pendientes {
        int id PK
        int id_kardex FK
        int asignado_a FK
        text descripcion
        text estado
        timestamp fecha_limite
    }


    %% ============================
    %% NOTIFICACIONES
    %% ============================

    notificaciones {
        int id PK
        int user_id FK
        text mensaje
        boolean leida
        timestamp created_at
    }


    %% ============================
    %% PARÁMETROS DEL SISTEMA
    %% ============================

    parametros_sistema {
        text clave PK
        text valor
        text descripcion
    }


    %% ============================
    %% RELACIONES
    %% ============================

    roles ||--o{ usuarios : "tiene"
    usuarios ||--o{ user_sessions : "sesiones"
    usuarios ||--o{ bitacora_login : "login"
    roles ||--o{ roles_permisos : "tiene permisos"
    permisos ||--o{ roles_permisos : "asignado a"

    usuarios ||--o{ audit_log : "realizó cambio"

    cat_plantillas ||--o{ rel_campos_plantilla : "define"
    cat_campos ||--o{ rel_campos_plantilla : "incluye"

    cat_plantillas ||--o{ kardex : "plantilla base"
    kardex ||--o{ kardex : "jerarquía padre"

    kardex ||--o{ data : "campos llenados"
    rel_campos_plantilla ||--o{ data : "valor para"

    kardex ||--o{ archivos : "adjunta"
    archivos ||--o{ archivos_versiones : "versiones"

    kardex ||--o{ tareas_pendientes : "requiere"
    usuarios ||--o{ tareas_pendientes : "asignado"

    usuarios ||--o{ notificaciones : "recibe"
