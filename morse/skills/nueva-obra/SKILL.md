---
name: nueva-obra
description: >
  Crea desde cero la estructura Morse de un nuevo proyecto de obra:
  carpetas, RESUMEN.txt y COMUNICACIONES.xlsx. Úsala cuando el jefe inicie
  un proyecto nuevo o seleccione una carpeta vacía donde montar el
  seguimiento de comunicaciones.
---

# MORSE · Nueva obra

Inicializa la estructura Morse en la carpeta seleccionada por el jefe. Una vez ejecutada, las skills `registro` y `consulta` pueden trabajar sobre ella sin más.

La carpeta raíz la elige el jefe al seleccionarla en Cowork. Esta skill no decide ubicación: trabaja siempre sobre la carpeta activa.

---

## 1. Verificaciones previas

**Paso 1 — Comprobar que la carpeta no tiene ya estructura Morse**
Si existe cualquiera de estos elementos, abortar y avisar al jefe:
- `COMUNICACIONES.xlsx`
- `Comunicaciones/`
- `Documentacion/`

No sobreescribir nunca. Si el jefe confirma que quiere reinicializar, pedirle que mueva o borre lo existente antes de continuar.

**Paso 2 — Si la carpeta tiene otros archivos (no Morse)**
Listarlos brevemente y preguntar si está bien crear la estructura conviviendo con ellos.

---

## 2. Crear estructura de carpetas

Crear en la raíz:
- `Comunicaciones/`
- `Documentacion/`

---

## 3. Conversación guiada para `RESUMEN.txt`

`RESUMEN.txt` es el archivo de contexto que las skills `registro` y `consulta` leen al arrancar. Su calidad determina lo bien que el sistema asigna temas, detecta riesgo contractual y redacta respuestas.

**Cómo conducir la conversación**
- Ir por bloques, uno cada vez. No volcar todas las preguntas a la vez.
- Después de cada bloque, preguntar al jefe si quiere seguir al siguiente o cerrar ya.
- El jefe puede saltar cualquier bloque. Lo único imprescindible es el Bloque 1.
- No inventar datos. Si el jefe no sabe algo o lo deja para después, dejarlo en blanco con una nota tipo `[pendiente]`.

**Bloques sugeridos**

1. **Identificación de la obra** — nombre del proyecto, dirección, código interno si lo hay. *(imprescindible)*
2. **Partes implicadas** — empresa contratista (la parte del jefe), promotor, dirección facultativa, subcontratas relevantes. Para cada una: nombre completo y abreviatura corta para usar en los IDs (ej. `DF`, `PROM`, `CONT`).
3. **Contrato** — fechas clave (firma, inicio, fin previsto), importe, penalizaciones por retraso, garantías, cualquier cláusula que el jefe quiera tener presente.
4. **Planning** — hitos relevantes y fechas críticas.
5. **Interlocutores habituales** — personas concretas con rol, contacto y abreviatura para los IDs (ej. `Arq. Rosa Fernández (DF)` → abreviatura `RF` o `DF`).
6. **Instrucciones del jefe** — estilo de redacción por defecto (`Formal` / `Técnico` / `Cordial`), preferencias específicas (CC habituales, nivel de formalidad con cada parte, palabras a evitar, etc.).
7. **Notas adicionales** — cualquier cosa que el jefe quiera dejar como contexto.

**Escritura del archivo**
Una vez el jefe cierre la conversación, escribir `Documentacion/RESUMEN.txt` con un bloque por sección. Estructura sugerida:

```
# [Nombre del proyecto]

## Identificación
...

## Partes implicadas
...

## Contrato
...

## Planning
...

## Interlocutores habituales
...

## Instrucciones del jefe
...

## Notas
...
```

Bloques omitidos pueden marcarse como `[pendiente]` o no incluirse, según prefiera el jefe.

---

## 4. Crear `COMUNICACIONES.xlsx`

**No regenerar el Excel por código.** La plantilla `references/COMUNICACIONES_template.xlsx` preserva la tabla `TablaComunicaciones`, su estilo (`TableStyleMedium2`), la validación de la columna Estado, los anchos de columna, el freeze pane y la fila de ejemplo `00_…` que sirve de guía visual y no interfiere con la numeración correlativa.

**Pasos:**
1. Copiar `references/COMUNICACIONES_template.xlsx` a la raíz del proyecto como `COMUNICACIONES.xlsx`.
2. Abrir el archivo recién copiado con `load_workbook()` y escribir en `A1` el nombre del proyecto recogido en el Bloque 1.
3. Guardar con `wb.save()`. No es necesario el patrón de parcheo de tabla de `registro/references/escritura-excel.md` porque no estamos añadiendo filas; solo modificando una celda existente.

---

## 5. Cierre

Resumir al jefe lo que se ha creado:
- Ruta de `COMUNICACIONES.xlsx`
- Ruta de `Documentacion/RESUMEN.txt`
- Carpetas `Comunicaciones/` y `Documentacion/`

Indicar que ya puede:
- Empezar a archivar comunicaciones con la skill `registro`.
- Hacer consultas con la skill `consulta`.

Si algún bloque del `RESUMEN.txt` quedó como `[pendiente]`, recordárselo brevemente para que pueda completarlo cuando tenga los datos.
