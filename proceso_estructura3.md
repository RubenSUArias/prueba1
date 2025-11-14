'''mermaid
erDiagram

    CAT_PLANTILLAS ||--o{ REL_PLANTILLA_CAMPOS : contiene
    CAT_CAMPOS ||--o{ REL_PLANTILLA_CAMPOS : pertenece
    CAT_PLANTILLAS ||--o{ KARDEX : usa

    CAT_PLANTILLAS {
        int id PK
        varchar nombre
        text descripcion
    }

    CAT_CAMPOS {
        int id PK
        varchar nombre_campo
        varchar tipo_campo
        text descripcion
    }

    REL_PLANTILLA_CAMPOS {
        int id PK
        int id_plantilla FK
        int id_campo FK
        int orden
        bool obligatorio
    }

    KARDEX {
        int id PK
        date fecha
        varchar titulo
        text descripcion
        int id_plantilla FK
        jsonb campos_valores
        timestamp creado_en
    }
