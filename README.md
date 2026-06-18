# Alarmcast one-page website

The marketing one-pager for **Alarmcast**, served at **https://alarmcast.app**.

Everything is static (no build step). Just HTML + inline CSS + a couple of SVGs.

```
website/
├── index.html      ← the whole page (inline CSS)
├── favicon.svg     ← browser tab icon (Wren)
├── og-image.svg    ← social/preview card (referenced by Open Graph tags)
├── assets/
│   └── wren.svg    ← the animated singing mascot (used in hero + footer)
├── CNAME           ← custom domain for GitHub Pages (alarmcast.app)
└── .nojekyll       ← tells GitHub Pages to serve files as-is
```

## Preview locally

Open `index.html` directly in a browser, or serve the folder:

```bash
# from inside website/
python -m http.server 8000
# then visit http://localhost:8000
```

## Deploy to GitHub Pages (with the alarmcast.app domain)

GitHub Pages serves a site from the **root** of a repo (or a `/docs` folder), so the
cleanest setup is a **dedicated repo** whose root is the contents of this `website/`
folder.

### 1. Create the repo
Create a new GitHub repo named **`alarmcast-site`** (empty, no README).

### 2. Push the contents of this folder to it
From inside `website/`:

```bash
git init
git add .
git commit -m "Alarmcast landing page"
git branch -M main
git remote add origin https://github.com/<your-username>/alarmcast-site.git
git push -u origin main
```

### 3. Turn on Pages
Repo → **Settings → Pages** → *Build and deployment* → Source: **Deploy from a branch**
→ Branch: **main**, folder: **/ (root)** → Save.

### 4. Point the domain at GitHub
The `CNAME` file in this folder already declares `alarmcast.app`. At your domain
registrar, add these DNS records for the apex domain:

**A records** (IPv4):
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```
**AAAA records** (IPv6):
```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```
Optionally add a **CNAME record** for `www` → `<your-username>.github.io` so
`www.alarmcast.app` redirects too.

### 5. Enforce HTTPS
Back in **Settings → Pages**, confirm the custom domain shows `alarmcast.app`,
wait for the DNS check to go green, then tick **Enforce HTTPS**.

> Note: `.app` is an HSTS-preloaded TLD, so HTTPS is *mandatory*. GitHub provisions a
> free Let's Encrypt certificate automatically. It can take a few minutes to an hour
> after DNS resolves.

## Editing copy

All text and styles live in `index.html`. The colour palette is defined once in the
`:root` block at the top (navy background, `#7C5CFF` purple, `#3DD6C0` teal,
`#EBC14D` Wren yellow). Change it there and the whole page follows.

The launch CTA currently points to `mailto:tarlmb@gmail.com`. Swap it for the real
Google Play link once the app is published (search `#get` and the `mailto:` links).
