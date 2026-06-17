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
| `_headers` | Cloudflare Pages security headers (CSP, nosniff, frame/permissions policy). |
| `_redirects` | Redirects `www.pacioli.io` → apex `pacioli.io`. |
| `.github/workflows/deploy.yml` | Optional auto-deploy on push to `main`. |

## Local preview

No tooling required — serve the folder statically:

```sh
npx serve .
```

Then open the printed URL and confirm the page renders, fonts load, and `404.html` resolves.

## Deploy

### (a) Cloudflare dashboard (Git integration)

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
