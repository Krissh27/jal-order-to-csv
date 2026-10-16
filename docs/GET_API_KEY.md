# Getting an OpenRouter API key

The tool uses OpenRouter to read the handwriting with two models: Gemini 3.1 Flash-Lite reads each photo, and Claude Sonnet 5.5 double-checks doubtful lines. Each read costs a small amount, billed to your OpenRouter account. One OpenRouter key works with every model on the platform, so the same key covers both.

## Steps

1. Go to <https://openrouter.ai/> and sign in (or create an account).
2. Add credit under **Settings → Credits**. A key won't work without some balance.
3. Go to **Keys**: <https://openrouter.ai/keys>.
4. Click **Create Key**. Name it something like `JAL order tool`.
5. Copy the key (starts with `sk-or-...`). **Copy it now** — you may not see it in full again.
6. Paste it into the tool's **OpenRouter API key** box and tick **Remember on this device**.

## Keeping the key safe

- Treat it like a password. Anyone with the key can spend your credit.
- Don't paste it into chats, emails, or anywhere public.
- If it ever leaks, delete it on the Keys page and create a new one.
- On a shared computer, click **Clear key** in the app when done.

## Rough cost

A clean order photo costs about ₹0.09. A photo where every line gets double-checked costs about ₹0.44. The app shows the exact cost after each read. You can set a spending limit on your OpenRouter account so it never surprises you.

## Which models

- **Reading:** `google/gemini-3.1-flash-lite` (the `READ_MODEL` line near the top of `index.html`).
- **Double-checking:** `anthropic/claude-sonnet-5.5` (the `CHECK_MODEL` line).

To try different models, edit those lines and paste any model ID from https://openrouter.ai/models.
