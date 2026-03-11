
```mermaid
flowchart TD

A[Usuario<br>Dispositivo móvil o PC] --> B[Frontend<br>HTML + JavaScript]

B --> C[Captura de datos<br>
- 7 respuestas<br>
- Fecha y hora<br>
- Ubicación]

C --> D[Solicitud HTTP POST<br>JSON]

D --> E[Backend<br>PHP API]

E --> F[Procesamiento<br>
Validación de datos]

F --> G[(Base de datos SQL<br>
Respuestas de encuesta)]

G --> H[Almacenamiento<br>
Registros históricos]
