# Using the Sale Order to CSV tool

A short guide for the JAL team. No installation — it runs in any web browser.

## One-time setup: your OpenRouter key

The tool uses OpenAI to read the handwriting. You need your own key (billed to your OpenAI account). See **GET_API_KEY.md** for how to create one. It looks like `sk-...`.

- Open the tool link.
- Paste the key into the **OpenAI API key** box at the top.
- Tick **Remember on this device** so you don't have to paste it again on this computer.

The key stays only in your browser. It is never uploaded or shared.

## Reading an order

1. **Take a clear photo** of the handwritten order. Good light, flat page, whole sheet in frame. Any photo format works, including iPhone HEIC, and you can also add a **PDF** (each page is read separately).
2. **Add the files** — drag onto the box, or click to choose. You can add several at once.
3. Click **Read photos**. Wait a few seconds per page. Doubtful lines get a second look automatically, which adds a few seconds.
4. The rows appear in the table, each with a status:
   - **✓ matched** / **✓✓ confirmed** (white or blue row) — the code is in the product list. Confirmed means it also passed the second check.
   - **✎ corrected** / **↺ changed** (amber row) — the code was fixed to a real one, or the second check picked a different code. The note under it shows what was first read. Check these against the photo.
   - **! not found** (red row) — not in the product list. Check it by hand.
   - **alphanumeric** (blue badge and blue row) — the code has letters in it, which JAL's system can mishandle. Make sure it imports correctly.
   - **box** (badge) — the order was in boxes. The qty column shows **pieces**, with the working underneath (e.g. "½ box × 24 pcs"). A plain "box" means the small box. If it says **estimated**, the box size was guessed from the product type, so confirm it. If it says **box size not known**, type the pieces yourself.

## Checking and fixing

- Every code and quantity is editable — just click a cell and type. The app re-checks the code against the product list as you type.
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
