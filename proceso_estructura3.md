```mermaid
erDiagram

    %% ============================
    %% TABLA DE USUARIOS Y ROLES
    %% ============================

    roles {
        int id PK
        text rol
        text descripcion
        int created_by FK
        timestamp created_at
        int updated_by FK
        timestamp updated_at
    }

    usuarios {
        int id PK
        text nombre
        text email
        text password_hash
        int id_rol FK
        int created_by FK
        timestamp created_at
        int updated_by FK
        timestamp updated_at
    }

    %% ============================
    %% TABLAS PRINCIPALES
    %% ============================

    cat_campos {
        int id PK
        text campo
        text tipo
        text descripcion
        int created_by FK
        timestamp created_at
        int updated_by FK
        timestamp updated_at
    }

    cat_plantillas {
        int id PK
        text plantilla
        text descripcion
        text empresas
        text usuarios
        int created_by FK
        timestamp created_at
        int updated_by FK
        timestamp updated_at
    }

    rel_campos_plantilla {
        int id PK
        int id_cat_campos FK
        int id_cat_plantillas FK
        int orden
        boolean obligatorio
        int created_by FK
        timestamp created_at
        int updated_by FK
        timestamp updated_at
    }

    kardex {
        int id PK
        date fecha
        text titulo
        text ubicacion
        int id_cat_plantilla FK
        int padre
        int created_by FK
        timestamp created_at
        int updated_by FK
        timestamp updated_at
    }

    data {
        int id PK
        int kardex FK
        int id_rel_campo_plantilla FK
        text valor
        int created_by FK
        timestamp created_at
        int updated_by FK
        timestamp updated_at
    }

    %% ============================
    %% RELACIONES
    %% ============================

    usuarios ||--o{ roles : "tiene rol"
    roles ||--o{ usuarios : "asignado a"

    cat_plantillas ||--o{ rel_campos_plantilla : "define campos"
    cat_campos ||--o{ rel_campos_plantilla : "forma parte de"

    cat_plantillas ||--o{ kardex : "plantilla usada"

    kardex ||--o{ data : "contiene valores"

    rel_campos_plantilla ||--o{ data : "campo llenado"
    
    kardex ||--o{ kardex : "padre (jerarquía)"

    %% Auditoría: todas las tablas tienen created_by y updated_by
    usuarios ||--o{ cat_campos : "creado/actualizado por"
    usuarios ||--o{ cat_plantillas : "creado/actualizado por"
    usuarios ||--o{ rel_campos_plantilla : "creado/actualizado por"
    usuarios ||--o{ kardex : "creado/actualizado por"
    usuarios ||--o{ data : "creado/actualizado por"
    usuarios ||--o{ roles : "creado/actualizado por"


