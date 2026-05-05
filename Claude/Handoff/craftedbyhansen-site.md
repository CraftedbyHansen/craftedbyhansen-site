# Handoff: craftedbyhansen-site

Date: 2026-05-05
Branch: latest work on `deploy/wrangler-static-assets` (PR open against `main`).

## What shipped

A flat repo with a single static one-pager, brand fonts self-hosted, deployed as a Cloudflare Worker with static assets.

```
craftedbyhansen-site/
├── wrangler.jsonc    # Worker config: name, assets binding, compat date
├── public/           # everything the Worker serves
│   ├── index.html
│   ├── favicon.svg / favicon.png
│   ├── og-image.svg / og-image.png
│   └── assets/fonts/ (Self Modern + Transcript, 4 woff2 files)
├── README.md         # project + deploy
├── CLAUDE.md         # repo rules for Claude Code
├── .gitignore
└── Claude/Handoff/craftedbyhansen-site.md  # this note (repo-internal, not served)
```

The page is fully self-contained: inline CSS, brand fonts self-hosted, no external font CDN, no `<script>`, no `<iframe>`. The only outbound URLs are the intentional Projects / Contact links (postcards.film, instagram, mailto).

## Deployment

The Cloudflare project for this repo was set up as a **Worker**, not a Pages project. `npx wrangler deploy` reads `wrangler.jsonc`, finds the existing `craftedbyhansen-site` Worker by name, and uploads `public/` as the bound static asset directory. `not_found_handling: "404-page"` means a missing path falls back to a 404 page if one exists, otherwise to a default 404 response. There is no Worker script, no fetch handler, no bindings beyond the assets binding.

## What was in the design zip and what I ignored

The zip is a *Design System* bundle, not just a one-pager. Everything else stayed out of the repo unless flagged below.

**Used:**

- `CraftedbyHansen.com.html`. The original landing page. Now lives at `public/index.html`.
- `fonts/`. All four woff2 files copied to `public/assets/fonts/`.
- `assets/logos/andyhansen_submark_rust.svg`. Copied to `public/favicon.svg`. Picked rust because the site's accent (`#A8553A`) is a terracotta close to the brand rust (`#B7764A`). A 32x32 `public/favicon.png` rasterization sits next to it for legacy browsers.
- The `public/og-image.svg` is hand-built: 1200x630 cream canvas with the words "Crafted by Hansen" set in Self Modern (the woff2 is embedded inline as base64 so the SVG renders the same in any environment, including server-side rasterizers). A matching `public/og-image.png` ships alongside, generated locally via `qlmanage` + `sips`.
- `colors_and_type.css`. Read for reference to confirm the brand font mapping (Self Modern = display serif, Transcript = body sans). Not shipped.

**Not used (intentionally left out of this repo):**

- `CraftedbyHansen.com (standalone).html`. A Claude artifact bundler wrapper around the same page. Discarded.
- `assets/logos/` (other 13 colorways). Only the rust submark and a brown wordmark colorway were referenced. Other SVGs stay in the design system skill.
- `preview/`. Design system preview cards. Not part of the public site.
- `ui_kits/private-gallery/`. JSX recreation of `collection.andyhansenfilms.com`. Not for this repo.
- `uploads/`. Duplicates of fonts and logos, plus the style guide PDF.
- `_scratch/`, `_export/`. Internal work files.
- The design system's own `README.md` and `SKILL.md`. Belong in the design system skill, not this repo.

## Audit findings

- **No Claude / Anthropic / bundler / artifact watermarks** in any served file.
- **No external font CDNs.** Google Fonts refs were stripped during the polish pass.
- **No `<script>` or `<iframe>` tags.** Page is HTML, inline CSS, self-hosted fonts.
- **External URLs that remain are all intentional:**
  - `https://postcards.film`. Outbound link in Projects.
  - `https://www.instagram.com/craftedbyhansen/`. Outbound in Contact.
  - `mailto:hello@craftedbyhansen.com`. Intentional.
  - `https://craftedbyhansen.com/og-image.svg` and `/favicon.svg`. Self-references in OG / icon meta tags. Need to resolve once deployed.

## Things still open / to decide

1. **OG meta tags currently point at `/og-image.svg`, not the PNG.** `og-image.png` ships next to it, ready to switch in if a platform fails to render the SVG. Twitter / X is the usual culprit. Run an unfurl test post-deploy and flip the two `og:image` / `twitter:image` URLs in `public/index.html` to `/og-image.png` if needed.

2. **Portrait placeholder.** The About section still shows a CSS gradient block labeled "Portrait · TK". Replace with a real image when you have one.

3. **Footer year.** Hardcoded `© 2026`. Fine now, will need updating in January.

## What you should verify before deploying

- [ ] Serve `public/` locally (`python3 -m http.server 8000 --directory public`, open `http://localhost:8000`). **Do not open `index.html` via `file://`** because the absolute paths only resolve from a server root.
- [ ] Confirm Self Modern shows on hero / project names / about copy / contact email link.
- [ ] Confirm Transcript shows on body / labels / wordmark / footer.
- [ ] Confirm favicon shows in the browser tab.
- [ ] Devtools network tab: no 404s on font files, no console errors.
- [ ] View source: confirm OG and Twitter Card meta tags are present in `<head>`.
- [x] Initial bootstrap pushed (`f958a52`).
- [x] Polish pass merged to `main` (`2b38bf1`).
- [x] Worker deploy config on branch `deploy/wrangler-static-assets`. PR open for your review.
- [ ] After merging, run `npx wrangler deploy` from the repo root. Confirm the upload lists files from `public/` only.
- [ ] Cloudflare DNS: once the zone is moved over, point the apex and `www` at the Worker.
- [ ] Post-deploy: run unfurl tests in the Meta Open Graph debugger and Twitter Card validator.
