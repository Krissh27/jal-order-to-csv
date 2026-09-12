# Deploy to GitHub Pages

You can do this entirely from your college laptop. ~5 minutes.

## 1. Create the repo on GitHub

1. Go to <https://github.com/new>.
2. Repository name: `jal-order-to-csv`
3. Set it to **Public** (Pages is free for public repos).
4. Don't add a README (this folder already has one).
5. Click **Create repository**.

## 2. Push this folder

In a terminal, inside this project folder:

```bash
git init
git add .
git commit -m "JAL sale order to CSV converter"
git branch -M main
git remote add origin https://github.com/<your-username>/jal-order-to-csv.git
git push -u origin main
```

Replace `<your-username>` with your GitHub username.

## 3. Turn on Pages

1. On the repo page, go to **Settings → Pages**.
2. Under **Build and deployment → Source**, choose **Deploy from a branch**.
3. Branch: **main**, folder: **/ (root)**. Click **Save**.
4. Wait ~1 minute. GitHub shows the live URL, which will be:

```
https://<your-username>.github.io/jal-order-to-csv/
```

## 4. Test it

Open that URL. Click **Load sample rows** → **Download CSV**. If you get a CSV, it's live.

## 5. Update the link in the README

Edit `README.md`, put the real URL under **Live app**, then:

```bash
git add README.md
git commit -m "Add live link"
git push
```

## Updating later

Any time you change a file:

```bash
git add .
git commit -m "describe the change"
git push
```

Pages redeploys automatically in about a minute.
