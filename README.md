# JAL — Sale Order to CSV

Turn photos of handwritten sale orders into an import-ready CSV.

Upload a photo → an AI vision model reads the item codes and quantities → you check and fix anything in an editable table → download a CSV whose columns match JAL's import format.

**Live app:** `https://<your-username>.github.io/jal-order-to-csv/`
_(update this link after you enable GitHub Pages — see below)_

---

## What it does

- Reads **handwritten** sale-order photos (product code + quantity).
- Shows every row in an **editable table**; low-confidence reads are highlighted so they get checked before export.
- Fills the fixed order fields (`vrtype`, `series_name`, `div_name`, etc.) from **editable defaults**, prefilled with the `JAL(MOHALI)` values.
- Exports a CSV in the exact column order JAL's system expects:
  `vrtype, dated, series_name, div_name, party_code, party_name, price_list_grp_name, emp_ucode, party_po_no, party_po_date, delivery_to_code, pucode, product, qty`

## How the API key is handled (important)

This app has **no backend and no stored key**. The person using it pastes their **own OpenRouter key** into the page. That key:

- lives only in **their browser** (optionally remembered on that device via `localStorage`),
- goes **straight from the browser to OpenAI**,
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

- **HEIC photos**: iPhone HEIC files can't be sent to the vision API by the browser. Export them as **JPG** on the phone first (the app shows a reminder if it sees a HEIC).
- Handwriting is hard. Always eyeball the highlighted rows before downloading.
- The `product` column carries the **item code** (matching JAL's sample, where the code sits in the `product` field).

## Files

```
index.html            the whole app (UI + logic)
README.md             this file
docs/USER_GUIDE.md    step-by-step guide for JAL
docs/DEPLOY.md        how to publish on GitHub Pages
docs/GET_API_KEY.md   how JAL creates an OpenRouter key
LICENSE               MIT
```
