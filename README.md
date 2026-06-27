# pacioli-web

The source for **[pacioli.io](https://pacioli.io)** — the marketing site for Pacioli, an open-source, currency-agnostic accounting platform for crypto and fiat.

---

## Looking for the Pacioli app?

This repository hosts the **marketing site only**. If you're here to use, install, or contribute to the application itself, head to one of these instead:

| | Repository | What's there |
| --- | --- | --- |
| 🧾 | **[Pacioli](https://github.com/GiveProtocolFoundation/Pacioli)** | The desktop + web application (Tauri + React + Rust). Multi-chain transaction indexing, double-entry accounting, cost-basis reports, and more. |
| 📚 | **[pacioli-docs](https://github.com/GiveProtocolFoundation/pacioli-docs)** | User guide, developer docs, API reference, and self-hosting guide. |
| 🌐 | **[pacioli-web](https://github.com/GiveProtocolFoundation/pacioli-web)** *(this repo)* | The static landing page served at pacioli.io. |

Live URLs: **App** → [pacioli.io](https://pacioli.io) · **Docs** → [docs.pacioli.io](https://docs.pacioli.io)

---

## About Pacioli

Named for [Luca Pacioli](https://en.wikipedia.org/wiki/Luca_Pacioli), the 15th-century mathematician who documented double-entry bookkeeping, the project brings professional accounting standards to multi-chain finance — without surrendering custody of your data.

- **Open-source** (AGPL-3.0) and community-driven
- **Local-first**: your ledger stays on your device
- **Multi-chain**: Bitcoin, Ethereum (and ERC-20 chains), Solana, Polkadot, plus traditional fiat accounts
- **Self-hostable** for organizations that need full control

Pacioli is a project of the [Give Protocol Foundation](https://github.com/GiveProtocolFoundation).

---

## About this repository

The marketing site is **plain static HTML, CSS, and a tiny inline script** — no build step, no bundler, no framework. The whole landing page lives in [`index.html`](./index.html) and is served as-is.

### Contents

| File | Purpose |
| --- | --- |
| `index.html` | The landing page (self-contained: inline CSS + script, Google Fonts, client-side GitHub API fetch for release info). |
| `404.html` | Branded "page not found" page. |
| `pacioli-logo.svg` | Pacioli brand mark, shared with the application. |
| `robots.txt` | Allows all crawlers; points to the sitemap. |
| `sitemap.xml` | Lists the apex URL. |
| `_headers` | Security headers (CSP, nosniff, frame/permissions policy). Netlify-native format; Cloudflare Pages reads it too. |
| `_redirects` | Redirects `www.pacioli.io` → apex `pacioli.io`. Netlify-native format; Cloudflare Pages reads it too. |
| `netlify.toml` | Netlify deploy config (publish root, no build step). |

### Local preview

No tooling required — serve the folder statically:

```sh
npx serve .
```

Then open the printed URL and confirm the page renders, fonts load, and `404.html` resolves.

### Deploy

**Production host: Netlify** (project `pacioli-io`). Connected via Netlify's Git integration, so every push to `main` auto-builds and deploys — no GitHub Actions or repository secrets required. Because the repo is plain static files (`_headers` and `_redirects` are Netlify-native), there is no build step.

**Netlify setup:**

1. Netlify dashboard: **Add new site → Import an existing project →** pick this repo.
2. Build settings: **Build command: (empty)**, **Publish directory: `.`** (also pinned in `netlify.toml`).
3. Deploy, then **Domain management → Add a custom domain** `pacioli.io` (and `www.pacioli.io`).
4. Point DNS: use Netlify DNS, or an `ALIAS`/`ANAME` (or `A` → Netlify load balancer) on the apex and a `CNAME` for `www`. The `www` → apex 301 is handled by `_redirects` (and Netlify's primary-domain redirect).

**Cloudflare Pages (optional mirror):** the same files run on CF Pages on its `*.pages.dev` subdomain. The apex `pacioli.io` can only point to one host at a time, so Netlify owns the apex while CF can mirror.

```sh
npx wrangler pages deploy . --project-name=pacioli-web
```

### Notes

- The `_headers` Content-Security-Policy is tuned to the page's needs: inline styles/script, Google Fonts (`fonts.googleapis.com` / `fonts.gstatic.com`), and the GitHub API (`api.github.com`). Adjust it if `index.html`'s external dependencies change.
- The brand mark (`pacioli-logo.svg`) is kept in sync with the application repo by manual copy. If the app updates the logo, re-copy from [`Pacioli/src/assets/pacioli_logo_black.svg`](https://github.com/GiveProtocolFoundation/Pacioli/blob/main/src/assets/pacioli_logo_black.svg).

---

## License

Pacioli is released under the **GNU AGPL-3.0**. See the [main repository's LICENSE](https://github.com/GiveProtocolFoundation/Pacioli/blob/main/LICENSE) for the full text.
