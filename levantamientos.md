'''mermaid
erDiagram

USUARIOS {
    INT id PK
    TEXT nombre
    TEXT email
    TEXT password_hash
    BOOLEAN activo
    BOOLEAN deleted
    TIMESTAMP created_at
}

ROLES {
    INT id PK
    TEXT rol
}

PERMISOS {
    INT id PK
    TEXT codigo
}

USUARIOS_ROLES {
    INT usuario_id FK
    INT rol_id FK
}

ROLES_PERMISOS {
    INT id PK
    INT id_rol FK
    INT id_permiso FK
}

USER_SESSIONS {
    INT id PK
    INT user_id FK
    TEXT jwt
    TIMESTAMP expires_at
    BOOLEAN revoked
}

BITACORA_LOGIN {
    INT id PK
    INT user_id FK
    BOOLEAN exito
    TEXT ip
}

KARDEX {
    INT id PK
    TEXT titulo
    TEXT descripcion
    DATE fecha
    INT padre FK
    LTREE path
    DOUBLE latitud
    DOUBLE longitud
    JSONB metadata
    INT created_by FK
    INT updated_by FK
}

ARCHIVOS {
    INT id PK
    INT id_kardex FK
    TEXT nombre_archivo
    TEXT ruta_storage
    JSONB metadata
    INT created_by FK
}

DATA {
    INT id PK
    INT kardex FK
    TEXT clave
    TEXT valor_texto
    JSONB valor_json
    INT created_by FK
}

AUDIT_LOG {
    BIGINT id PK
    TEXT tabla
    INT registro_id
    TEXT accion
    JSONB valores_antes
    JSONB valores_despues
    INT user_id FK
}

USUARIOS ||--o{ USUARIOS_ROLES : tiene
ROLES ||--o{ USUARIOS_ROLES : asigna

ROLES ||--o{ ROLES_PERMISOS : contiene
PERMISOS ||--o{ ROLES_PERMISOS : define

USUARIOS ||--o{ USER_SESSIONS : genera
USUARIOS ||--o{ BITACORA_LOGIN : registra

USUARIOS ||--o{ KARDEX : crea
USUARIOS ||--o{ KARDEX : actualiza

USUARIOS ||--o{ ARCHIVOS : sube
USUARIOS ||--o{ DATA : registra

USUARIOS ||--o{ AUDIT_LOG : ejecuta

KARDEX ||--o{ KARDEX : jerarquia
KARDEX ||--o{ ARCHIVOS : contiene
KARDEX ||--o{ DATA : almacena
