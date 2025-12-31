# Inventario - Alerta stock < 10 (Sheets -> Gmail)

Workflow de **n8n** que monitorea una planilla de **Google Sheets** y envía una alerta por **Gmail** cuando el stock de un producto baja de un umbral (por defecto `< 10`). Después marca la fila como “Alerta Enviada” para evitar duplicados.

## Archivo del workflow
- `Inventario - Alerta stock _ 10 (Sheets Hoja1 -_ Gmail).json`

## ¿Qué hace?

1. **Google Sheets Trigger**: detecta actualizaciones de filas (polling cada minuto).
2. **IF - Stock Bajo y Sin Alerta**: si `Stock < 10` (y si la alerta no fue enviada), continúa.
3. **Gmail - Enviar Alerta**: envía un correo con el producto y el stock actual.
4. **Update row in sheet**: actualiza la fila en Sheets para marcar `Alerta Enviada = Verdadero`.

## Requisitos

### En Google Sheets
Una hoja (ej: `Hoja1`) con estas columnas:
- `Producto` (texto) ✅
- `Stock` (número) ✅
- `Alerta Enviada` (texto o boolean) ✅

> Recomendación: usar valores `Verdadero/Falso` o `TRUE/FALSE`.

### En n8n
Credenciales:
- Google Sheets (Trigger + Update)
- Gmail (OAuth2)

## Configuración en n8n (pasos)

1. Importá el `.json`:
   - n8n → **Workflows** → **Import from file** → seleccionar el archivo `.json`.
2. Abrí el workflow y configurá:
   - **Google Sheets Trigger**: Spreadsheet y Sheet correctos.
   - **Gmail - Enviar Alerta**: cambiar `sendTo` al email deseado.
3. Revisá el nodo **IF**:
   - Asegurate de que además de `Stock < 10` esté chequeando que `Alerta Enviada` esté vacío o sea falso, para no repetir correos.
4. Activá el workflow.

## Notas importantes (para repos públicos)
- Revisá que no quede hardcodeado:
  - email real en el nodo de Gmail
  - IDs de spreadsheet sensibles
- Ideal: mover destinatario y umbral a variables (ej: `ALERT_TO`, `STOCK_THRESHOLD`).
