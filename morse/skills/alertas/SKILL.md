---
name: alertas
description: >
  Muestra el panel de alertas activas del proyecto: plazos vencidos,
  acciones inmediatas y todos los hilos pendientes agrupados por tema.
  Úsala cuando el jefe quiera ver el estado de alerta del proyecto,
  pregunte por lo que tiene pendiente, o quiera actualizar el panel.
---

# MORSE · Alertas del proyecto

Genera o actualiza el panel de alertas del proyecto en el sidebar de Cowork. Muestra solo lo que requiere acción: plazos vencidos, urgencias y hilos pendientes agrupados por tema.

---

## 1. Lectura de datos

**Lee `Documentacion/RESUMEN.txt`** para obtener contexto del proyecto (nombre, interlocutores, plazos contractuales). Si no existe, continúa solo con el Excel.

**Lee `COMUNICACIONES.xlsx`** completo. Carga todas las filas excluyendo la de ejemplo (`00_…`).

---

## 2. Detectar alertas

Revisa todas las filas con `Estado = "Pendiente respuesta"` y clasifícalas:

**Tipo `vencido`** — el plazo ha expirado:
- El campo `Plazo respuesta` contiene una fecha explícita (`antes del DD/MM`, `DD/MM/YYYY`, etc.) que ya ha pasado.
- O el campo indica un número de días hábiles desde la `Fecha` de la comunicación, y ese plazo ya ha vencido.
- Label: `"Vencido · DD/MM"`

**Tipo `inmediata`** — requiere acción urgente sin fecha concreta:
- El campo `Plazo respuesta` dice `"Inmediato"`, `"Urgente"` o equivalente.
- O el `Resumen` describe una paralización de obra, un requerimiento de Cultura/administración, o una situación de riesgo que no admite demora.
- Label: `"Acción inmediata"`

Usa tu criterio contextual: una comunicación de enero con plazo de 5 días hábiles que sigue "Pendiente respuesta" en abril es claramente vencida aunque no tenga fecha explícita.

---

## 3. Agrupar por tema

Para cada tema que aparezca en el Excel:

1. Reúne todas sus comunicaciones.
2. **Estado del tema** = el estado más urgente entre sus filas:
   `Pendiente respuesta` > `Pendiente cliente` > `Registrada` > `Cerrada`
3. **Incluir solo** temas con estado `Pendiente respuesta` o `Pendiente cliente`.
4. **Resumen del tema** = resumen de la comunicación más reciente con estado pendiente en ese tema. Una frase concisa que describa el estado actual del hilo.
5. **last_date** = fecha de la comunicación más reciente del tema (formato `DD/MM`).
6. **last_actor** = si la última comunicación es `IN`, usar la abreviatura del remitente entre paréntesis (ej: `"IN de DF"`). Si es `OUT`, usar `"OUT del JO"`.
7. **urgent** = `true` si alguna comunicación del tema aparece en la lista de alertas del paso 2.

**Ordenar los temas:** primero los `urgent`, luego los de `Pendiente respuesta`, luego los de `Pendiente cliente`.

---

## 4. Construir el JSON y generar el artifact

**Construir el objeto de datos:**

```python
import json, re
from datetime import datetime

data = {
    "project_name": project_name,     # celda A1 del Excel
    "folder": folder_name,            # nombre de la carpeta del proyecto
    "updated_at": datetime.now().strftime("%d/%m/%Y %H:%M"),
    "alerts": [
        # {"type": "vencido", "label": "Vencido · 20/04", "text": "Andamio torre norte — ..."},
        # {"type": "inmediata", "label": "Acción inmediata", "text": "Pinturas históricas — ..."},
    ],
    "pendiente_respuesta": 0,         # total de filas con ese estado
    "pendiente_cliente": 0,
    "themes": [
        # {
        #   "name": "Medios auxiliares y andamiaje",
        #   "status": "Pendiente respuesta",
        #   "summary": "DF detecta deformaciones nivel +18m...",
        #   "count": 1,
        #   "last_date": "15/04",
        #   "last_actor": "IN de DF",
        #   "urgent": True
        # }
    ]
}
new_json = json.dumps(data, ensure_ascii=False)
```

**Leer la plantilla HTML:**
Usar Read sobre `references/alertas.html` (en la misma carpeta que este SKILL.md).

**Inyectar los datos:**
```python
html_final = re.sub(
    r'/\* MORSE_DATA_START \*/.*?/\* MORSE_DATA_END \*/',
    f'/* MORSE_DATA_START */{new_json}/* MORSE_DATA_END */',
    html_template,
    flags=re.DOTALL
)
```

---

## 5. Crear o actualizar el artifact

El id del artifact es siempre `morse-[nombre-carpeta]-alertas` (minúsculas, guiones). Ejemplo: `morse-kalam-alertas`.

**Si el artifact ya existe** (comprobarlo con `mcp__cowork__list_artifacts`):
- Llamar a `mcp__cowork__update_artifact` con el HTML modificado y `update_summary`: `"Panel actualizado — [N] alertas activas, [M] hilos pendientes"`.

**Si no existe**:
- Llamar a `mcp__cowork__create_artifact`:
  - `id`: `morse-[nombre-carpeta]-alertas`
  - `html`: el HTML generado
  - `description`: `Panel de alertas — [Nombre del proyecto]`

---

## 6. Respuesta al jefe

Tras crear o actualizar el artifact, responder brevemente en el chat:
- Cuántas alertas activas hay y de qué tipo (vencidas / inmediatas).
- Cuántos hilos pendientes de respuesta y cuántos pendientes de cliente.
- Si hay cero alertas, confirmarlo con un mensaje tranquilizador.

No repetir el contenido del panel — el jefe ya lo tiene en el sidebar.
