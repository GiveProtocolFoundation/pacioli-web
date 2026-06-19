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
| `.github/workflows/deploy.yml` | Optional auto-deploy to Cloudflare Pages on push to `main`. |

## Local preview

No tooling required — serve the folder statically:

```sh
npx serve .
```

Then open the printed URL and confirm the page renders, fonts load, and `404.html` resolves.

## Deploy

The site runs unchanged on either Netlify or Cloudflare Pages — `_headers` and
`_redirects` are read by both. **The apex `pacioli.io` (and `www`) can only
point to one host at a time**, so pick one as the production host; the other can
run in parallel on its platform subdomain (`*.netlify.app` / `*.pages.dev`).

### Netlify (Git integration — recommended where a Netlify account exists)

1. Netlify dashboard: **Add new site → Import an existing project →** pick this repo.
2. Build settings: **Build command: (empty)**, **Publish directory: `.`** (also pinned in `netlify.toml`).
3. Deploy, then **Domain management → Add a custom domain** `pacioli.io` (and `www.pacioli.io`).
4. Point DNS: use Netlify DNS, or an `ALIAS`/`ANAME` (or `A` → Netlify load balancer) on the apex and a `CNAME` for `www`. The `www` → apex 301 is handled by `_redirects` (and Netlify's primary-domain redirect).

### Cloudflare Pages (a) dashboard (Git integration)

1. Push this repo to GitHub.
2. In the Cloudflare dashboard: **Workers & Pages → Create → Pages → Connect to Git**.
3. Select this repository.
4. Build settings: **Framework preset: None**, **Build command: (empty)**,
   **Build output directory: `/`**.
5. Save and deploy. Add the custom domain `pacioli.io` (and `www.pacioli.io`) under the project's **Custom domains** tab.

### (b) Wrangler CLI

```sh
npx wrangler pages deploy . --project-name=pacioli-web
```

### (c) GitHub Actions (optional)

`.github/workflows/deploy.yml` auto-deploys on push to `main`. It requires two
repository secrets:

- `CLOUDFLARE_API_TOKEN` — a token with the **Cloudflare Pages: Edit** permission.
- `CLOUDFLARE_ACCOUNT_ID` — your Cloudflare account ID.

## Notes

- The `_headers` Content-Security-Policy is tuned to the page's needs: inline
  styles/script, Google Fonts (`fonts.googleapis.com` / `fonts.gstatic.com`),
  and the GitHub API (`api.github.com`). Adjust it if `index.html`'s external
  dependencies change.
