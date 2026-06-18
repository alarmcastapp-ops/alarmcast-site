# Alarmcast one-page website

The marketing one-pager for **Alarmcast**, live at **https://alarmcast.app**.

Everything is static (no build step) — HTML + inline CSS + a few SVGs/PNGs.

## Contents

```
website/                          (this folder == the deployed repo root)
├── index.html                    ← the whole page (inline CSS in a :root block)
├── favicon.svg                   ← browser tab icon (Wren)
├── og-image.svg                  ← social / link-preview card (Open Graph tags)
├── CNAME                         ← custom domain for GitHub Pages (alarmcast.app)
├── .nojekyll                     ← tells GitHub Pages to serve files as-is
├── .gitignore                    ← ignores OS cruft (.DS_Store, Thumbs.db, …)
└── assets/
    ├── alarmcast-icon.png / .svg ← app icon (hero, header, footer)
    ├── wren.svg                  ← the singing-mascot Wren
    └── screens/                  ← app screenshots: alarm, sound, gentle-wake, snooze
```

## Live deployment (already set up)

| | |
|---|---|
| **Repo** | [`alarmcastapp-ops/alarmcast-site`](https://github.com/alarmcastapp-ops/alarmcast-site) — branch `main`, repo root = these files |
| **Host** | GitHub Pages — *Settings → Pages → Deploy from a branch* `main` / `/ (root)` |
| **Domain** | `alarmcast.app` (declared in `CNAME`), registrar **Porkbun** |
| **www** | `www.alarmcast.app` → CNAME → `alarmcastapp-ops.github.io` |
| **HTTPS** | Auto Let's Encrypt cert via GitHub Pages (`.app` is HTTPS-only) |

This folder is its **own git repo**, initialised in place and separate from the parent
`alarmcast` monorepo (which gitignores `website/`). Push as the **alarmcastapp-ops**
GitHub account — a personal account without write access gets a `403`.

### Update the live site
Edit files here, then **from inside `website/`**:

```bash
git add -A
git commit -m "your message"
git push
```

GitHub Pages redeploys within a minute or two. (From the monorepo root you can also run
the same commands prefixed with `git -C website …`.)

### Preview locally
```bash
# from inside website/
python -m http.server 8000   # then visit http://localhost:8000
```

## First-time setup (reference — already done)

<details>
<summary>How this was wired up, in case it ever needs re-creating</summary>

1. **Repo** — created an empty GitHub repo `alarmcast-site` under the **alarmcastapp-ops**
   account (no README/license/.gitignore).
2. **Push** — from inside `website/`:
   ```bash
   git init -b main
   git add -A && git commit -m "Alarmcast landing page"
   git remote add origin https://github.com/alarmcastapp-ops/alarmcast-site.git
   git push -u origin main          # authenticate as alarmcastapp-ops
   ```
3. **Pages** — *Settings → Pages → Source: Deploy from a branch → `main` / `/ (root)`*.
   The `CNAME` file auto-fills the custom domain as `alarmcast.app`.
4. **DNS (Porkbun)** — on the apex `alarmcast.app` (Host field left **blank**):
   - **A** (IPv4): `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - **AAAA** (IPv6): `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`
   - **CNAME**: `www` → `alarmcastapp-ops.github.io`

   Delete any default `pixie.porkbun.com` parking records (incl. the wildcard `*` CNAME).
5. **HTTPS** — once the DNS check goes green in *Settings → Pages*, tick **Enforce HTTPS**.

> A freshly-added `www` record can read *"improperly configured (InvalidDNSError)"* in
> GitHub for ~15–60 min because of negative DNS caching. The apex still works; the warning
> clears on its own.
</details>

## Editing copy

All text and styles live in `index.html`. The colour palette is defined once in the
`:root` block at the top (navy background, `#7C5CFF` purple, `#3DD6C0` teal,
`#EBC14D` Wren yellow). Change it there and the whole page follows.

The launch CTA currently points to `mailto:tarlmb@gmail.com`. Swap it for the real
Google Play link once the app is published (search `#get` and the `mailto:` links).
