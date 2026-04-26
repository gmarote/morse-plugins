# MORSE · Plugin de Gestión de Comunicaciones de Obra

MORSE centraliza el registro, archivo y seguimiento de todas las comunicaciones de un proyecto de construcción.

## Skills disponibles

- **nueva-obra** — inicializa un proyecto desde cero: crea las carpetas, genera el `RESUMEN.txt` mediante una conversación guiada, monta el `COMUNICACIONES.xlsx` y crea el dashboard en el sidebar de Cowork
- **registro** — archiva comunicaciones entrantes y salientes, redacta respuestas, actualiza el Excel y refresca el dashboard automáticamente
- **consulta** — consulta el estado del proyecto, busca comunicaciones y resume hilos sin modificar nada (también refresca el dashboard)

## Estructura de proyecto

Cada obra vive en su propia carpeta con esta estructura, que `nueva-obra` crea automáticamente:

```
Mi Obra/
├── COMUNICACIONES.xlsx    ← base de datos de comunicaciones
├── Comunicaciones/        ← archivos de todas las comunicaciones
└── Documentacion/
    └── RESUMEN.txt        ← contexto del proyecto
```

## Dashboard

Al inicializar un proyecto, MORSE crea un **dashboard persistente** en el sidebar de Cowork. Muestra:

- Contadores por estado: pendiente respuesta, pendiente cliente, registrada, cerrada
- Tabla filtrable y con búsqueda libre por ID, remitente, asunto o tema
- Tooltip con el resumen de cada comunicación al pasar el cursor

El dashboard se actualiza automáticamente cada vez que el jefe usa `registro` o `consulta`. No requiere ninguna acción adicional.

## RESUMEN.txt — qué contiene

`nueva-obra` lo construye conversando con el jefe de obra. Cubre:

- Datos de la obra (nombre, dirección, referencia, fechas, importe)
- Interlocutores (jefe de obra, cliente, dirección facultativa, etc.)
- Cláusulas clave del contrato (penalizaciones, plazos, condiciones)
- Hitos del proyecto
- Instrucciones específicas para Claude (tono por interlocutor, alertas, etc.)

## Cómo usar

1. Instala el plugin en Cowork
2. Apunta Cowork a la carpeta de tu obra
   - **Obra nueva** (carpeta vacía): lanza `nueva-obra` para montar la estructura y el dashboard
   - **Obra ya inicializada**: comparte una comunicación para archivarla, o pregunta por el estado del proyecto
