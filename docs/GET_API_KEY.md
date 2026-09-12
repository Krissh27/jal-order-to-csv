# Getting an OpenRouter API key

The tool uses OpenRouter (running OpenAI's GPT-4o vision model) to read the handwriting. Each read costs a small amount, billed to your OpenRouter account.

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

Reading a typical order photo through `openai/gpt-4o` on OpenRouter costs a few US cents. You can set a spending limit on your OpenRouter account so it never surprises you.

## Which model

The tool uses **openai/gpt-4o** via OpenRouter. No configuration needed — just the key.
