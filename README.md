# JAL — Sale Order to CSV

Turn photos of handwritten sale orders into an import-ready CSV.

Upload a photo → an AI vision model reads the item codes and quantities → you check and fix anything in an editable table → download a CSV whose columns match JAL's import format.

**Live app:** `https://<your-username>.github.io/jal-order-to-csv/`
_(update this link after you enable GitHub Pages — see below)_

---

## What it does

- Reads **handwritten** sale-order photos (product code + quantity).
- **Accepts PDFs and any photo format**: JPG, PNG, HEIC/HEIF (iPhone), WebP, TIFF (incl. multi-page), BMP, GIF, AVIF. Everything is converted to JPEG in the browser (max 2000px side) before it's sent. Each PDF page is read separately (first 20 pages per PDF).
- Shows every row in an **editable table**; low-confidence reads are highlighted so they get checked before export.
- Fills the fixed order fields (`vrtype`, `series_name`, `div_name`, etc.) from **editable defaults**, prefilled with the `JAL(MOHALI)` values.
- **Validates & corrects every code against JAL's master list.** The app bundles the official product list (`vendor/cadcodes.js`, ~18,000 codes). After the AI reads a code, the app:
  - keeps it if it matches the catalogue exactly (**Matched**);
  - auto-fixes a near-miss to the closest valid code — handling classic misreads like `B`↔`8`, `O`↔`0`, `I/L`↔`1`, `S`↔`5` — and shows the original reading (**Corrected**);
  - flags anything with no close match for manual review (**Not found**).
  Spaces, dots and hyphens are ignored when matching, so `PT 12360-26`, `PT-12360-26` and `PT12360-26` all match `PT12360-26`.
- **Two models: a cheap one reads, a strong one checks.**
  - **First read: `READ_MODEL` (Gemini 3.1 Flash-Lite)** reads every full photo. It returns each line's code, quantity and unit, plus **where the line is on the photo** (a box on a 0–1000 scale).
  - **Second check: `CHECK_MODEL` (Claude Sonnet 5.5)** looks only at doubtful lines (on by default; untick "Double-check doubtful lines" to turn it off). A line is doubtful if it was corrected, not found or low-confidence, or if it matches a code with a real look-alike one character away (e.g. `PT12362-31` vs `PT12363-31`). The app **crops just those lines out of the photo** in the browser (strips at most 600px wide, about 11 KB each) and sends all of a page's strips in **one** request, each with a shortlist of up to 5 real catalogue codes. Sonnet never sees the full page. If any doubtful line on a page has no usable position, that page's check sends the full page instead.
  - Results: **Confirmed** (second check agrees), **Changed** (second check picked a different code, flagged amber), or **Not found**.
  - Cost, estimated for a 10-line order with every line checked: about ₹0.09 (Gemini) + ₹0.34 (Sonnet on strips) ≈ **₹0.44 per photo**, against about ₹1.08 when Sonnet did both steps on the full photo. A clean page with no doubtful lines costs only Gemini's ≈ ₹0.09.
  - After each read, the line next to "Read photos" shows requests and cost **per model**, the total, and whether the check used strips or the full page.
  - Each line's crop also appears under its code in the review table (click to enlarge), so staff can check the handwriting without opening the photo.
- **Box orders are converted to pieces.** Orders can say "1 BOX", "½ BOX" or "2 master boxes". The model reports the number and unit separately. The app multiplies by the pieces per box: a plain "box" is the **small box**, and "master box" / "carton" / "MB" is the master box. The CSV's `qty` is always **pieces**, so JAL's import format doesn't change. Box sizes come from `vendor/boxsizes.js`, in this order:
  1. **box sheet**: the code is on the PTMT MOQ sheet;
  2. **other colour**: the same tap in another colour is on the sheet (`PT12362-31` uses `PT12362-26`'s box size);
  3. **estimate**: a PTMT faucet whose description matches one of the sheet's product-type rows ("All Bib Swivel / Pillar Swivel" = 12 per box). These rows are **amber**. When tested against codes that *are* on the sheet, the type rows agreed for all 6 older `PT12xxx` taps but none of the 9 newer `PT14xxx` taps, so treat estimates as guesses;
  4. **unknown**: the row is **red** with an empty quantity, and staff type the pieces. The CSV download warns if any are still empty.
- **Alphanumeric codes are highlighted** — any code containing a letter (`PT12360-26`, `J-301`, `EL11220-CH`) gets a blue row and an "alphanumeric" badge in the table, because JAL's system can mishandle these.
- **Outputs the code only, never the description.** `SINGLE LIVER BASIN MIXER (EL11220-CH)` becomes `EL11220-CH`; `81390 TWO WAY TAPS` becomes `81390`.
- **Flagged Excel export** — the "Export flagged Excel" button downloads a colour-highlighted `.xlsx`: red for not-found codes and unknown box sizes, amber for corrected/changed rows and estimated box sizes, blue for alphanumeric codes. Extra columns: `ordered_as` (e.g. "0.5 box"), `pcs_per_box`, `box_size_from`, `check_status`, `alphanumeric`, `note`, plus a legend sheet. Uses `vendor/exceljs.min.js` (bundled, no internet needed).
- Exports a CSV in the exact column order JAL's system expects:
  `vrtype, dated, series_name, div_name, party_code, party_name, price_list_grp_name, emp_ucode, party_po_no, party_po_date, delivery_to_code, pucode, product, qty`

## How the API key is handled (important)

This app has **no backend and no stored key**. The person using it pastes their **own OpenRouter key** into the page. That key:

- lives only in **their browser** (optionally remembered on that device via `localStorage`),
- goes **straight from the browser to OpenRouter**,
- is **never** committed to this repo, uploaded, or logged anywhere.

Because the key is never in the code, it is safe to host this repo publicly on GitHub Pages.

> **Never paste an API key into the source files or commit one.** If you ever do, revoke it immediately at openrouter.ai/keys.

## Mock mode (no key needed)

Leave the key field blank, or click **Load sample rows**, and the app runs in **mock mode** with example data. Use this to demo the whole flow without spending anything.

## Running locally

It's a single static file. Just open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Deploying to GitHub Pages

See [`docs/DEPLOY.md`](docs/DEPLOY.md) for copy-paste commands.

## Handing it to JAL

Send them the live link plus [`docs/USER_GUIDE.md`](docs/USER_GUIDE.md) (or the PDF version). That's the whole handover — no install on their side.

## Notes & limits

- **HEIC photos** are converted in the browser (Safari natively; other browsers use the bundled `heic-to` decoder, loaded only when a HEIC file is added).
- The models are set by the `READ_MODEL` and `CHECK_MODEL` lines near the top of the script in `index.html`. Any OpenRouter model ID works; set both to the same ID to use one model for everything. `READ_MODEL` must be able to give line positions (Gemini models are trained for this); if it can't, every check falls back to the full page.
- If the cheap first read misreads a code into a *different real code* that has no look-alike, nothing flags it and the second check never sees it. Staff should still glance over the table, using the line previews.
- Handwriting is hard. Always eyeball the highlighted rows before downloading. The second check can't fix codes that aren't in the master list, or writing a person couldn't read either.
- Fractions of a box ("½ box") are converted (½ × 24 = 12 pcs). If the result isn't a whole number, the row is flagged.

## Updating box sizes

Box sizes live in two CSVs you can open in Excel:

- `data/ptmt_box_sizes.csv`: one row per code (typed up from the PTMT MOQ sheet photo; **check these numbers against the original sheet**).
- `data/ptmt_box_types.csv`: the "All Bib Tap Long Body…" product-type rows.

After editing them (or adding another brand's codes to the first file), rebuild the app's lookup:

```bash
python3 tools/build_boxsizes.py path/to/Product_List_Detail_Report.xlsx
```

It prints how many codes each source covers and where the type rows disagree with the sheet.
- The `product` column carries the **item code** (matching JAL's sample, where the code sits in the `product` field).

## Files

```
index.html            the whole app (UI + logic)
README.md             this file
vendor/cadcodes.js    JAL's master code list (from Product_List_Detail_Report.xlsx)
vendor/boxsizes.js    pieces per box (generated by tools/build_boxsizes.py)
data/ptmt_box_sizes.csv, data/ptmt_box_types.csv   box sizes to edit
tools/build_boxsizes.py   rebuilds vendor/boxsizes.js from the CSVs
vendor/exceljs.min.js Excel export
vendor/pdf.min.js, pdf.worker.min.js   PDF pages -> images (pdf.js 3.11)
vendor/heic-to.js     iPhone HEIC -> JPEG (heic-to 1.6.6)
vendor/UTIF.js, pako_inflate.min.js    TIFF decoding
docs/USER_GUIDE.md    step-by-step guide for JAL
docs/DEPLOY.md        how to publish on GitHub Pages
docs/GET_API_KEY.md   how JAL creates an OpenRouter key
LICENSE               MIT
```
