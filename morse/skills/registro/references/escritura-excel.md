# Escritura segura al Excel — patrón obligatorio

Usar siempre este patrón al modificar `COMUNICACIONES_REGISTRO.xlsx`. La hoja `Comunicaciones` contiene una tabla Excel llamada **TablaComunicaciones**; el patrón parchea el XML del ZIP para extender el ref de la tabla a la nueva última fila.

```python
import shutil, os, tempfile, zipfile, re, io
from openpyxl import load_workbook
from copy import copy

EXCEL_PATH = "COMUNICACIONES_REGISTRO.xlsx"
BACKUP_PATH = "COMUNICACIONES_REGISTRO.backup.xlsx"

# 0. Comprobar lock file de Excel
lock_path = os.path.join(os.path.dirname(os.path.abspath(EXCEL_PATH)), "~$" + os.path.basename(EXCEL_PATH))
if os.path.exists(lock_path):
    try:
        load_workbook(EXCEL_PATH).close()
        # Lock residual (caché): se puede continuar
    except Exception:
        raise RuntimeError("El archivo está abierto en Excel. Ciérralo antes de continuar.")

# 1. Copia de seguridad
shutil.copy2(EXCEL_PATH, BACKUP_PATH)

# 2. Cargar y escribir datos
wb = load_workbook(EXCEL_PATH)
ws = wb["Comunicaciones"]
next_row = ws.max_row + 1
# ... escribir datos en next_row ...

# 3. Copiar formato de la fila anterior (fuente, alineación, relleno, bordes, formato de número)
for col in range(1, 11):
    src = ws.cell(next_row - 1, col)
    dst = ws.cell(next_row, col)
    dst.font = copy(src.font)
    dst.alignment = copy(src.alignment)
    dst.fill = copy(src.fill)
    dst.border = copy(src.border)
    dst.number_format = src.number_format

# 4. Guardar en archivo temporal
tmp_fd, tmp_path = tempfile.mkstemp(suffix=".xlsx", dir=os.path.dirname(os.path.abspath(EXCEL_PATH)))
os.close(tmp_fd)

try:
    wb.save(tmp_path)

    # 5. Extender el ref de TablaComunicaciones para incluir la nueva fila
    buf = io.BytesIO()
    with zipfile.ZipFile(tmp_path, 'r') as zin:
        with zipfile.ZipFile(buf, 'w', zipfile.ZIP_DEFLATED) as zout:
            for item in zin.infolist():
                data = zin.read(item.filename)
                if re.match(r'xl/tables/table\d+\.xml', item.filename):
                    content = data.decode('utf-8')
                    content = re.sub(
                        r'(ref="[A-Z]+\d+:[A-Z]+)\d+"',
                        lambda m: m.group(1) + str(next_row) + '"',
                        content
                    )
                    data = content.encode('utf-8')
                zout.writestr(item, data)
    with open(tmp_path, 'wb') as f:
        f.write(buf.getvalue())

    # 6. Verificar que el temporal es válido
    load_workbook(tmp_path).close()
    # 7. Reemplazar el original
    shutil.move(tmp_path, EXCEL_PATH)
except Exception as e:
    if os.path.exists(tmp_path):
        os.remove(tmp_path)
    raise RuntimeError(f"Error al guardar. El original está intacto. Detalle: {e}")
```

**Reglas:**
- Nunca usar `wb.save(EXCEL_PATH)` directamente.
- El backup `.backup.xlsx` se sobreescribe en cada operación — es solo un salvavidas de la última sesión.
- El parcheo del XML aplica solo al añadir filas. Para modificar celdas existentes (cambio de Estado, etc.) no es necesario.
