# MORSE · Plugin de Gestión de Comunicaciones de Obra

MORSE centraliza el registro, archivo y seguimiento de todas las comunicaciones de un proyecto de construcción.

## Estructura de proyecto requerida

Cada obra necesita su propia carpeta con esta estructura:

```
Mi Obra/
├── COMUNICACIONES_REGISTRO.xlsx   ← base de datos de comunicaciones
├── Comunicaciones/                ← archivos de todas las comunicaciones
└── Documentacion/
    └── RESUMEN.txt                ← contexto del proyecto (ver plantilla)
```

## RESUMEN.txt — qué debe incluir

- Datos de la obra (nombre, dirección, referencia, fechas, importe)
- Interlocutores (jefe de obra, cliente, dirección facultativa, etc.)
- Cláusulas clave del contrato (penalizaciones, plazos, condiciones)
- Hitos del proyecto
- Instrucciones específicas para Claude (tono por interlocutor, alertas, etc.)

## Cómo usar

1. Instala el plugin en Cowork
2. Apunta Cowork a la carpeta de tu obra
3. Comparte una comunicación (email, imagen, documento) y MORSE la archiva y registra automáticamente

## Versión

0.1.0
