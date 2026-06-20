# pacioli-web

Static marketing site for [Pacioli](https://pacioli.io) — an open-source,
double-entry accounting app. This repo is plain static hosting: `index.html`
is served as-is with **no build step, bundler, or framework**.

## Contents

| File | Purpose |
| --- | --- |
| `index.html` | The landing page (self-contained: inline CSS + script, Google Fonts, client-side GitHub API fetch for release info). |
| `404.html` | Branded "page not found" page. |
| `favicon.svg` | Ledger-book glyph favicon (gold stroke, transparent background). |
| `robots.txt` | Allows all crawlers; points to the sitemap. |
| `sitemap.xml` | Lists the apex URL. |
| `_headers` | Security headers (CSP, nosniff, frame/permissions policy). Netlify-native format; Cloudflare Pages reads it too. |
| `_redirects` | Redirects `www.pacioli.io` → apex `pacioli.io`. Netlify-native format; Cloudflare Pages reads it too. |
| `netlify.toml` | Netlify deploy config (publish root, no build step). |

## Local preview

No tooling required — serve the folder statically:

```sh
npx serve .
```

Then open the printed URL and confirm the page renders, fonts load, and `404.html` resolves.

## Deploy

**Production host: Netlify** (project `pacioli-io`). The site is connected via
Netlify's Git integration, so every push to `main` auto-builds and deploys —
no GitHub Actions or repository secrets required. Because the repo is plain
static files (`_headers`/`_redirects` are Netlify-native), there is no build step.

### Netlify (Git integration)

1. Netlify dashboard: **Add new site → Import an existing project →** pick this repo.
2. Build settings: **Build command: (empty)**, **Publish directory: `.`** (also pinned in `netlify.toml`).
3. Deploy, then **Domain management → Add a custom domain** `pacioli.io` (and `www.pacioli.io`).
4. Point DNS: use Netlify DNS, or an `ALIAS`/`ANAME` (or `A` → Netlify load balancer) on the apex and a `CNAME` for `www`. The `www` → apex 301 is handled by `_redirects` (and Netlify's primary-domain redirect).

### Cloudflare Pages (optional alternative / mirror)

The same files also run on Cloudflare Pages. **The apex `pacioli.io` (and `www`)
can only point to one host at a time**, so CF can only be a mirror on its
`*.pages.dev` subdomain while Netlify owns the apex.

- Dashboard: **Workers & Pages → Create → Pages → Connect to Git**, framework preset **None**, build command empty, output directory `/`.
- Or CLI: `npx wrangler pages deploy . --project-name=pacioli-web`.

## Notes

- The `_headers` Content-Security-Policy is tuned to the page's needs: inline
  styles/script, Google Fonts (`fonts.googleapis.com` / `fonts.gstatic.com`),
  and the GitHub API (`api.github.com`). Adjust it if `index.html`'s external
  dependencies change.
