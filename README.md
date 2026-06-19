# OxynX website

Modern, static marketing site for **OxynX** (`oxynx.com`) — built as plain HTML/CSS/JS so it
deploys to **GitHub Pages** with zero build step.

```
oxynx-site/
├─ index.html            # Landing page
├─ privacy/index.html    # Privacy Policy (verbatim)
├─ terms/index.html      # Software License & Beta Testing Agreement (verbatim)
├─ 404.html              # Branded not-found page
├─ favicon.svg
├─ CNAME                 # oxynx.com  (custom domain)
├─ robots.txt, sitemap.xml
└─ .nojekyll             # serve folders as-is on GitHub Pages
```

> **Each page is fully self-contained:** its CSS lives in an inline `<style>` block and its
> small JS in an inline `<script>`. There is no external stylesheet to fetch, so every page
> renders correctly in any context — a preview pane, a `file://` open, or GitHub Pages — with
> no broken-styling window while assets load.

## Preview locally

Just open `index.html` in a browser (double-click works — pages are self-contained), or serve
the folder:

```bash
# from this directory
python -m http.server 8080
# then visit http://localhost:8080
```

> Internal links are **relative** (`privacy/`, `../terms/`), so the site works the same whether
> opened from disk, served locally, or deployed to the `oxynx.com` root on GitHub Pages.
> (`404.html` is the one exception: it links to `/` and `/favicon.svg` with root-absolute paths,
> which is correct because GitHub serves it for missing URLs at any depth.)

## Deploy to GitHub Pages

**1. Create the repo and push** (run from this folder):

```bash
git init
git add -A
git commit -m "OxynX website"
git branch -M main
# create the repo on github.com first (e.g. oxynx/oxynx-website), then:
git remote add origin https://github.com/<org-or-user>/<repo>.git
git push -u origin main
```

(Or with the GitHub CLI: `gh repo create <org>/<repo> --public --source=. --push`.)

**2. Turn on Pages:** GitHub repo → **Settings → Pages** → *Build and deployment* →
Source: **Deploy from a branch** → Branch: **main** / **/(root)** → Save.

**3. Custom domain:** the included `CNAME` file already sets **oxynx.com**. In
Settings → Pages → *Custom domain*, confirm `oxynx.com` and tick **Enforce HTTPS**.

## DNS (at your domain registrar for oxynx.com)

Point the domain to GitHub Pages:

| Type  | Host  | Value                          |
|-------|-------|--------------------------------|
| CNAME | `www` | `<org-or-user>.github.io`      |
| A     | `@`   | `185.199.108.153`              |
| A     | `@`   | `185.199.109.153`              |
| A     | `@`   | `185.199.110.153`              |
| A     | `@`   | `185.199.111.153`              |

(Optionally add the matching `AAAA` records: `2606:50c0:8000::153`, `…8001::153`, `…8002::153`,
`…8003::153`.) GitHub then auto-redirects `www.oxynx.com` → the apex `oxynx.com`.

> Switching the domain from the old Google Site to GitHub Pages = just updating these DNS records.
> Prefer the **apex** `oxynx.com` as primary instead of `www`? Change `CNAME` to `oxynx.com`,
> keep the four `A` records on `@`, and add a `CNAME www → <user>.github.io`.

DNS changes can take from a few minutes up to ~24h to propagate; HTTPS certificate issuance on
GitHub follows shortly after.

## Editing

- **Content & copy:** edit the `.html` files directly.
- **Colors / spacing / type:** brand tokens live in the `:root { … }` block at the top of each
  page's inline `<style>` (start with `index.html`). Keep them in sync across pages if you
  rebrand — the landing page carries the full design system; the legal pages and 404 carry the
  subset they use.
- **Legal pages** (`privacy/`, `terms/`) carry the agreement text **verbatim** — only the design
  changed. Update wording there if Legal revises it.
- Brand: deep purple `#291735` / accent `#4b1e78` / gold `#ffd56b`. Fonts: Sora (display) + Inter.

Contact: admin@oxynx.com · © OxynX, LLC
