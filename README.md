# replyhawk.com (landing page)

Static landing page for ReplyHawk. Deployed via GitHub Pages with a custom domain.

## Files

- `index.html`            — marketing landing page
- `privacy.html`          — Privacy Policy (referenced by Thumbtack API access form)
- `terms.html`            — Terms of Service (referenced by Thumbtack API access form)
- `CNAME`                 — created at deploy time to map the custom domain to GitHub Pages
- `BRAND.md`              — internal brand guide (do not deploy publicly; harmless if it stays)
- `PARTNERSHIP-READINESS.md` — internal partnership readiness notes (same)

## Deploy on GitHub Pages

1. **Create a public repo** named e.g. `replyhawk-site`.
2. **Push:**
   ```bash
   cd ~/Desktop/ALEX/replyhawk
   git init -b main
   git add .
   git commit -m "ReplyHawk landing"
   git remote add origin git@github.com:<your-user>/replyhawk-site.git
   git push -u origin main
   ```
3. **Enable Pages:** repo Settings → Pages → Source: `Deploy from a branch` → Branch: `main` → `/ (root)` → Save.
4. **Custom domain:**
   - In the same Pages settings, type your domain (e.g. `reply-hawk.com`) and save. GitHub creates a `CNAME` file in the repo.
   - At your DNS provider (Cloudflare), add a `CNAME` record: name `@` (or `www`), value `<your-user>.github.io`.
   - Enable "Enforce HTTPS" in Pages settings once the cert is issued (~5 minutes).

## URLs to paste into the Thumbtack API access form

After deploy:

| Field            | URL                                       |
|------------------|-------------------------------------------|
| Company website  | `https://reply-hawk.com`                  |
| Client URI       | `https://reply-hawk.com`                  |
| Policy URI       | `https://reply-hawk.com/privacy.html`     |
| Terms of Service | `https://reply-hawk.com/terms.html`       |
| Logo URI         | (optional — leave blank or link a PNG)    |
| Redirect URI     | `https://app.reply-hawk.com/api/auth/callback/thumbtack` (placeholder until the app is deployed) |

`hello@replyhawk.ai`, `privacy@replyhawk.ai`, `legal@replyhawk.ai`, `partnerships@replyhawk.ai`, `founders@replyhawk.ai` are referenced in the HTML — set those up as forwarders on your domain (Cloudflare Email Routing → free) before launch.
