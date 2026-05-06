# Campos de cada fila — TablaComunicaciones

Una comunicación puede generar varias filas en el Excel, una por tema detectado. Los campos de documento se repiten en todas las filas; los campos de tema son específicos de cada una.

| Campo | Tipo | Descripción |
|---|---|---|
| ID | Documento | Identifica el archivo en `Comunicaciones/`. Formato: `[NN]_[IN/OUT]_[YYYYMMDD]_[Remitente]-[descriptor]`. Varias filas pueden compartir el mismo ID. |
| Fecha | Documento | Fecha del documento (no la de registro). Formato DD/MM/YYYY. Si no hay fecha visible, usar la de recepción e indicarlo en el Resumen. |
| Dirección | Documento | `IN` = recibida por la empresa contratista. `OUT` = enviada por la empresa contratista. |
| Medio | Documento | `Email`, `Acta`, `Burofax`, `Carta` o `WhatsApp`. |
| Remitente | Documento | Nombre y cargo o entidad. Ej: `Arq. Rosa Fernández (DF)` |
| Tema | Tema | Agrupación temática. Claude propone; el jefe puede corregir en el Excel. Usar `Varios` para puntos menores sin hilo propio. |
| Asunto | Tema | Título descriptivo y específico del asunto tratado en este tema. |
| Resumen | Tema | 2-3 líneas: qué pide o informa sobre este tema, si hay plazo, qué implica. |
| Plazo respuesta | Tema | Texto libre si aplica. Ej: `5 días hábiles`, `Urgente`, `Antes del 30/04`. |
| Estado | Tema | `Registrada` · `Pendiente respuesta` · `Pendiente cliente` · `Cerrada` |

## Columna Estado

- **Registrada** — archivada, no requiere acción. Para comunicaciones informativas o OUT sin seguimiento pendiente.
- **Pendiente respuesta** — la empresa debe actuar: responder, firmar, entregar algo.
- **Pendiente cliente** — la empresa ya actuó y espera respuesta del otro lado. También para OUT iniciados por la empresa donde se espera respuesta.
- **Cerrada** — resuelta, sin acción pendiente en ningún lado.

Claude propone el estado al registrar. El jefe puede corregirlo directamente en el Excel.
