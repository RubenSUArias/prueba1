```mermaid
erDiagram
    PLANTILLA {
        int id
        string nombre
        string descripcion
    }

    CAMPOS {
        int id
        string nombre
        string tipo
        string descripcion
    }

    REL_PLANTILLA_CAMPOS {
        int id
        int id_plantillas
        int id_campos
        int orden
        boolean obligatorio
    }

    KARDEX {
        int id
        date fecha
        string titulo
        int id_rel_campos_plantilla
    }

    DATA {
        int id
        int id_kardex
        int id_campos
        string valor
    }

    %% Relaciones
    PLANTILLA ||--o{ REL_PLANTILLA_CAMPOS : "usa"
    CAMPOS ||--o{ REL_PLANTILLA_CAMPOS : "define"
    REL_PLANTILLA_CAMPOS }o--o{ KARDEX : "aplica a"
    KARDEX ||--o{ DATA : "contiene valores"
    CAMPOS ||--o{ DATA : "define valor"
