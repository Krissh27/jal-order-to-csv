# Using the Sale Order to CSV tool

A short guide for the JAL team. No installation — it runs in any web browser.

## One-time setup: your OpenRouter key

The tool uses OpenAI to read the handwriting. You need your own key (billed to your OpenAI account). See **GET_API_KEY.md** for how to create one. It looks like `sk-...`.

- Open the tool link.
- Paste the key into the **OpenAI API key** box at the top.
- Tick **Remember on this device** so you don't have to paste it again on this computer.

The key stays only in your browser. It is never uploaded or shared.

## Reading an order

1. **Take a clear photo** of the handwritten order. Good light, flat page, whole sheet in frame. If your phone saves photos as **HEIC**, export/share them as **JPG** first.
2. **Add the photo(s)** — drag onto the box, or click to choose. You can add several at once.
3. Click **Read photos**. Wait a few seconds per photo.
4. The rows appear in the table. **Pink rows** are ones the AI wasn't sure about — check those first.

## Checking and fixing

- Every code and quantity is editable — just click a cell and type.
- Fixing a pink row clears the highlight.
- **+ Add empty row** adds a line the AI missed.
- The 🗑 button deletes a row.

## Order defaults

Click **Order defaults** to set the fields that are the same for the whole order — party code, date, series, etc. These are prefilled for JAL(MOHALI) and remembered on your device. They fill every row of the CSV.

## Download

Click **Download CSV**. The file has the exact columns your system imports. Open it to double-check, then import it as usual.

## Tips for better reads

- One order sheet per photo reads better than a photo of several sheets.
- Straight-on, not at an angle. Avoid shadows across the writing.
- If a code is genuinely unreadable in the photo, it will be unreadable to the AI too — fix it by hand in the table.

## Trying it without a key

Leave the key box blank and click **Load sample rows** to see the whole flow with example data (no cost). Good for a first look.
