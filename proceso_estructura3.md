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
        jsonb estructura   "Listado de IDs de cat_campos"
    }

    kardex {
        int id PK
        date fecha
        text titulo
        text ubicacion     "lat,lon"
        int id_cat_plantilla FK
        int padre          "Referencia a otro registro del kardex"
        jsonb datos        "IDs de cat_campos y documentos asociados"
    }

    %% ============================
    %% RELACIONES
    %% ============================

    %% cat_plantillas.estructura --> lista de cat_campos.id
    cat_plantillas ||--o{ cat_campos : "incluye (vía jsonB)"

    %% Kardex pertenece a una plantilla
    cat_plantillas ||--o{ kardex : "define estructura"

    %% Kardex tiene jerarquía (padre → hijo)
    kardex ||--o{ kardex : "padre-hijo"


