---
name: registro
description: >
  Registra y archiva comunicaciones de obra: emails, actas, WhatsApps o
  documentos recibidos o enviados. Úsala cuando el usuario comparta una
  comunicación para archivar, quiera redactar una respuesta, o necesite
  añadir un nuevo registro al proyecto.
---

# MORSE · Sistema de Gestión de Comunicaciones de Obra

MORSE centraliza el registro, archivo y seguimiento de todas las comunicaciones de un proyecto de construcción en una estructura de carpetas estándar, con un archivo excel de seguimiento.

**Estructura de carpetas del proyecto:**
- `COMUNICACIONES.xlsx` — base de datos de todas las comunicaciones
- `Comunicaciones/` — archivos de todas las comunicaciones, entrantes y salientes
- `Documentacion/` — documentos de referencia del proyecto (resumen, contratos, planning, etc.)

Todo el contexto específico del proyecto está en `Documentacion/RESUMEN.txt`. Léelo al arrancar y úsalo como referencia para cualquier decisión de fondo. No inventes datos que no estén ahí.

**Regla de comunicación:** trabajar en silencio durante las comprobaciones de inicio, la lectura de archivos y la escritura del Excel. No narrar pasos internos ("leyendo el Excel", "sin lock file", "contexto cargado", etc.). Solo hablar al jefe cuando se necesite su input, cuando haya un error que le afecte, o al presentar el resultado final.

---

## 1. Comprobaciones de inicio

**Paso 1 — Lee `Documentacion/RESUMEN.txt`**
Contiene el contexto de la obra e instrucciones específicas del jefe. Sin ese archivo, avisa de que falta y no proceses nada.

**Paso 2 — Lee `Documentacion/TEMAS.txt`**
Contiene el catálogo de temas abiertos del proyecto, con sus descripciones y líneas `Incluye:`. Úsalo para asignar el tema de la comunicación nueva. Si el archivo no existe o está vacío (sin bloques de tema), trátalo como catálogo vacío: cualquier comunicación abre un tema nuevo.

**Paso 3 — Verifica el lock file del Excel**
Comprueba programáticamente si existe `~$COMUNICACIONES.xlsx` en la carpeta. Si existe, intenta abrir el archivo como verificación secundaria (puede ser caché residual):
- Si el archivo **no se puede abrir** → Excel está abierto. Avisa al jefe y no continúes hasta que el lock desaparezca.
- Si el archivo **sí se puede abrir** → lock residual, puedes continuar.

**Paso 4 — Lee el Excel (solo el último ID)**
Abre `COMUNICACIONES.xlsx` y lee únicamente `max_row` de la hoja **Comunicaciones** para saber cuál es el último ID registrado. Si la hoja está vacía (primera comunicación del proyecto), el próximo ID será `01`. No leer más datos del Excel en esta fase.

---

## 2. Archivar la comunicación

**Paso 1 — Identificar el formato**
- **A — Imagen o screenshot:** extrae el contenido como texto.
- **B — Documento adjunto** (PDF, Word, etc.): se archivará directamente sin conversión.
- **C — Copy-paste de texto:** listo para guardar.

**Paso 2 — Asignar el ID**
Formato: `[NN]_[IN/OUT]_[YYYYMMDD]_[Remitente]-[descriptor-corto]`
Ejemplo: `07_IN_20260501_DF-penalizaciones-cubierta`
El número es correlativo global. Revisar siempre el último en el Excel antes de asignar.

**Paso 3 — Guardar en `Comunicaciones/`**
Guardar el archivo con el ID como nombre. Casos A y C: guardar como `.txt`. Caso B: guardar el archivo original tal cual.

**Adjuntos**
Todo lo que llega junto comparte número de ID y genera un único registro en el Excel. Los adjuntos se guardan con el mismo ID más un sufijo descriptivo:
```
32_IN_20260521_DF-penalizaciones.txt          ← registro en Excel
32_IN_20260521_DF-penalizaciones_foto-fachada.jpg
32_IN_20260521_DF-penalizaciones_informe.pdf
```
Si algún adjunto es un documento formal con entidad propia (acta firmada, burofax, informe técnico), indicárselo al jefe y preguntar si quiere registrarlo también por separado.

---

## 3. Actualizar el Excel con el nuevo registro

**Identificar temas y crear filas**
Leer la comunicación completa y agrupar su contenido por temas. Contrastar cada tema detectado contra los bloques de `Documentacion/TEMAS.txt` para decidir si encaja en uno existente o requiere uno nuevo.

Crear **una fila por tema** en el Excel. Los campos ID, Fecha, Dirección, Medio y Remitente se repiten en todas las filas; los campos Tema, Asunto, Resumen, Plazo respuesta y Estado son específicos de cada fila.

- Si un punto o conjunto de puntos encaja en un tema existente, asignarlo.
- Si hay puntos menores, informativos o sin hilo propio, agruparlos en una sola fila con tema **"Varios"**.
- Si se detecta un tema que no existe en TEMAS.txt, crear un nombre descriptivo y añadir el bloque correspondiente.

El jefe puede corregir temas directamente en el Excel.

**Actualizar `Documentacion/TEMAS.txt`**
Modificar el archivo solo en estos casos:
- **Tema nuevo:** añadir al final un bloque con este formato:
  ```
  ## [Nombre del tema]
  [Una o dos frases describiendo el perímetro del tema: qué tipo de asuntos abarca.]
  ```
- **Descripción insuficiente:** si la comunicación revela que el perímetro del tema es más amplio de lo que describe el bloque actual, ampliar la descripción.
- **Tema ya bien descrito:** no tocar el archivo.

**Campos:** ver `references/campos-excel.md`

**Escritura segura:** usar siempre el patrón de `references/escritura-excel.md`. Nunca guardar directamente sobre el original.

---

## 4. Confirmar registro y proponer siguiente paso

Presentar un resumen de lo archivado:

```
Registrado: [ID] — [Medio], [Fecha]

Temas registrados:
- **[Tema A]** — [Asunto]. Estado: [Estado].
- **[Tema B]** — [Asunto]. Estado: [Estado].
```

Si algún tema quedó como **Pendiente respuesta**, preguntar al jefe si quiere revisar el hilo de ese tema con `resumen` para valorar la situación y preparar respuesta. Si hay varios temas pendientes, listarlos y preguntar cuál revisar primero.

Si todos los temas quedaron como **Registrada**, **Pendiente cliente** o **Cerrada**, indicar que el registro está completo sin acciones pendientes por nuestra parte.
