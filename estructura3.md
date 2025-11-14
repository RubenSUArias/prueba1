```mermaid
erDiagram

    %% ============================
    %% TABLAS PRINCIPALES
    %% ============================

    cat_campos {
        int id PK
        text campo
        text tipo
        text descripcion
    }

    cat_plantillas {
        int id PK
        text plantilla
        text descripcion
        text empresas
        text usuarios
    }

    rel_campos_plantilla {
        int id PK
        int id_cat_campos FK
        int id_cat_plantillas FK
        int orden
        boolean obligatorio
    }

    kardex {
        int id PK
        date fecha
        text titulo
        text ubicacion
        int id_cat_plantilla FK
        int padre
    }

    data {
        int id PK
        int kardex FK
        int id_rel_campo_plantilla FK
        text valor
    }

    %% ============================
    %% RELACIONES
    %% ============================

    cat_plantillas ||--o{ rel_campos_plantilla : "define campos"
    cat_campos ||--o{ rel_campos_plantilla : "forma parte de"

    cat_plantillas ||--o{ kardex : "plantilla usada"

    kardex ||--o{ data : "contiene valores"

    rel_campos_plantilla ||--o{ data : "campo llenado"
    
    kardex ||--o{ kardex : "padre (jerarquía)"
