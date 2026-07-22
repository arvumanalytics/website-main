# Arvum — Waitlist / Pilot landing page

Single-file static landing page for **Arvum**, a unified retail platform with an AI dynamic-pricing engine. Visitors read the pitch and apply to the private pilot program.

The entire site is one self-contained [`index.html`](index.html): all CSS is inlined, and the only external requests are web fonts (Google Fonts + a webfont CDN) and the form submission. There is no build step.

## Run locally

Just open the file:

```sh
open index.html          # macOS
```

Or serve it over HTTP (recommended, matches production):

```sh
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploy to GitHub Pages

This repo ships with a Pages workflow at [`.github/workflows/pages.yml`](.github/workflows/pages.yml) that publishes on every push to `main`.

1. Push this repo to GitHub.
2. In the repo, go to **Settings → Pages → Build and deployment**, and set **Source** to **GitHub Actions**.
3. Push to `main` (or run the workflow manually). The site publishes at `https://<user>.github.io/<repo>/`.

**Alternative (no workflow):** Settings → Pages → Source → **Deploy from a branch** → `main` / `/ (root)`. The included `.nojekyll` file makes Pages serve the files as-is.

### Custom domain
To serve from `arvumanalytics.com` (or any custom domain), add it under Settings → Pages → Custom domain (this creates a `CNAME` file). No other changes are needed — all asset URLs in the page are absolute HTTPS or inline, so the site works at any base path.

## Configuration

- **Domain / SEO** — `index.html` sets `<link rel="canonical">` and the `og:url` to `https://arvumanalytics.com/`. Update those two if you host elsewhere, so social/search metadata points at the right URL.
- **Pilot form** — the form POSTs to Formspree (`https://formspree.io/f/xjgneked`) via `fetch`, with a native-POST fallback if JavaScript is unavailable. To change the destination, edit the form's `action` attribute.
  - **Activate it:** a new Formspree form only goes live after its **first real submission** — Formspree emails you a confirmation link. Submit the live form once to activate delivery.
  - Fields sent: `nombre`, `empresa`, `email`, `tiendas`, `caso_de_uso` (plus a `_gotcha` honeypot and `_subject`).
- **Social preview image** — not included. For richer link cards, add a 1200×630 image and reference it with `<meta property="og:image">` (absolute URL).

## Project structure

```
.
├── index.html                 # the entire site
├── .github/workflows/pages.yml # GitHub Pages deploy on push to main
├── .nojekyll                  # serve files as-is (branch-deploy method)
├── .gitignore
└── README.md
```

## Notes

- Fonts load from the network (Figtree + Caprasimo from Google Fonts; "Lovelo Black" display face from `db.onlinewebfonts.com`), each with system fallbacks. To remove the third-party dependency, self-host the font files.
- Respects `prefers-reduced-motion` — the ticker, rotating headline, and entrance animations are disabled for users who ask for reduced motion.

---

© Arvum. All rights reserved. This page and its content are proprietary.
