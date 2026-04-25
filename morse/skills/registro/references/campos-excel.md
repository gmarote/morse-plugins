# Campos de cada fila — TablaComunicaciones

| Campo | Descripción |
|---|---|
| ID | Código único. Formato: `[NN]_[IN/OUT]_[YYYYMMDD]_[Remitente]-[descriptor]` |
| Fecha | Fecha del documento (no la de registro). Formato DD/MM/YYYY. Si no hay fecha visible, usar la de recepción e indicarlo en el Resumen. |
| Dirección | `IN` = recibida por la empresa contratista. `OUT` = enviada por la empresa contratista. |
| Medio | `Email`, `Acta`, `Burofax`, `Carta` o `WhatsApp`. |
| Remitente | Nombre y cargo o entidad. Ej: `Arq. Rosa Fernández (DF)` |
| Asunto | Título descriptivo y específico. Evitar asuntos genéricos. |
| Tema | Agrupación temática. Claude propone; el jefe puede corregir en el Excel. |
| Resumen | 2-3 líneas: qué pide o informa, si hay plazo, qué implica. |
| Plazo respuesta | Texto libre si aplica. Ej: `5 días hábiles`, `Urgente`, `Antes del 30/04`. |
| Estado | `Registrada` · `Pendiente respuesta` · `Pendiente cliente` · `Cerrada` |

## Columna Estado

- **Registrada** — archivada, no requiere acción. Para comunicaciones informativas o OUT sin seguimiento pendiente.
- **Pendiente respuesta** — la empresa debe actuar: responder, firmar, entregar algo.
- **Pendiente cliente** — la empresa ya actuó y espera respuesta del otro lado. También para OUT iniciados por la empresa donde se espera respuesta.
- **Cerrada** — resuelta, sin acción pendiente en ningún lado.

Claude propone el estado al registrar. El jefe puede corregirlo directamente en el Excel.
