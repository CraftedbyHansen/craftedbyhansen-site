# Handoff: craftedbyhansen-site

Date: 2026-05-05
Branch: latest work on `polish/pre-deploy-fonts-favicon-og` (PR open against `main`).

## What shipped

A flat repo with a single static one-pager and a small assets folder.

```
craftedbyhansen-site/
├── index.html        # the one-pager
├── favicon.svg       # rust submark, browser tab icon (modern browsers)
├── favicon.png       # 32x32 PNG fallback (legacy browsers)
├── og-image.svg      # 1200x630 "Crafted by Hansen" in Self Modern on cream
├── og-image.png      # rasterized version of og-image.svg, same dimensions
├── assets/
│   └── fonts/        # Self Modern + Transcript (4 woff2 files)
├── README.md         # project + deploy
├── CLAUDE.md         # repo rules for Claude Code
├── .gitignore        # macOS junk, node_modules
└── Claude/Handoff/craftedbyhansen-site.md  # this note
```

The page is fully self-contained: inline CSS, brand fonts self-hosted, no external font CDN, no `<script>`, no `<iframe>`. The only outbound URLs are the intentional Projects / Contact links (postcards.film, instagram, mailto).

## What was in the design zip and what I ignored

The zip is a *Design System* bundle, not just a one-pager. Everything else stayed out of the repo unless flagged below.

**Used:**

- `CraftedbyHansen.com.html`. Copied verbatim as `index.html` in the initial commit. Polish pass swapped fonts and added meta tags but kept the visual design intact.
- `fonts/`. All four woff2 files copied to `assets/fonts/`.
- `assets/logos/andyhansen_submark_rust.svg`. Copied to `/favicon.svg`. Picked rust because the site's accent (`#A8553A`) is a terracotta close to the brand rust (`#B7764A`). A 32x32 `/favicon.png` rasterization sits next to it for legacy browsers.
- The `og-image.svg` is hand-built: 1200x630 cream canvas with the words "Crafted by Hansen" set in Self Modern (the woff2 is embedded inline as base64 so the SVG renders the same in any environment, including server-side rasterizers). Replaces an earlier draft that used the andyhansen wordmark, which read as the wrong brand. A matching `og-image.png` ships alongside, generated locally via `qlmanage` + `sips`.
- `colors_and_type.css`. Read for reference to confirm the brand font mapping (Self Modern = display serif, Transcript = body sans). Not shipped.

**Not used (intentionally left out of this repo):**

- `CraftedbyHansen.com (standalone).html`. A Claude artifact bundler wrapper around the same page. Not a deployable static page. Discarded.
- `assets/logos/` (other 13 colorways). Only the rust submark and brown wordmark are referenced. The other SVGs stay in the design system skill.
- `preview/`. Design system preview cards (color, type, spacing, components). Not part of the public site.
- `ui_kits/private-gallery/`. Full HTML/JSX recreation of the wedding-film delivery site (a different product, lives at `collection.andyhansenfilms.com`). Not for this repo.
- `uploads/`. Duplicates of the brand fonts and SVG logos, plus `Andy Hansen Style Guide.pdf`.
- `_scratch/`, `_export/`. Internal work files.
- `README.md`, `SKILL.md`. Design system docs (the skill manifest). Belong in the design system skill, not this repo.

## Audit findings (post-polish)

- **No Claude / Anthropic / bundler / artifact watermarks** in `index.html`.
- **No external font CDNs.** Google Fonts (`googleapis.com`, `gstatic.com`) refs are fully removed.
- **No `<script>` or `<iframe>` tags.** Page is HTML + inline CSS + self-hosted fonts.
- **External URLs that remain are all intentional:**
  - `https://postcards.film`. Outbound link in Projects.
  - `https://www.instagram.com/craftedbyhansen/`. Outbound in Contact.
  - `mailto:hello@craftedbyhansen.com`. Intentional.
  - `https://craftedbyhansen.com/og-image.svg` and `/favicon.svg`. Self-references in OG / icon meta tags. These need to resolve once deployed.

## Things still open / to decide

1. **OG meta tags currently point at `/og-image.svg`, not the PNG.** The `og-image.png` ships next to it, ready to switch in if you find a platform that does not handle SVG OG images. Twitter / X has historically been the unreliable one, so do a unfurl test there post-deploy and flip the two `og:image` / `twitter:image` URLs in `index.html` to `/og-image.png` if needed.

2. **Portrait placeholder.** The About section still shows a CSS gradient block labeled "Portrait · TK". Replace with a real image when you have one.

3. **Footer year.** Hardcoded `© 2026`. Fine now, will need updating in January.

## What you should verify before deploying

- [ ] Serve the repo locally (`python3 -m http.server 8000` from the repo root, then open `http://localhost:8000`). **Do not open `index.html` via `file://`** because the absolute paths (`/favicon.svg`, `/assets/fonts/...`) only resolve from a server root.
- [ ] Confirm Self Modern shows on headings / about copy / contact email link, and Transcript shows on body / labels / wordmark / footer. Compare against the design system preview.
- [ ] Confirm favicon shows in the browser tab.
- [ ] Open devtools, confirm no 404s on font files, no console errors.
- [ ] View source, confirm `<meta property="og:*">` and `<meta name="twitter:*">` tags are in `<head>`.
- [x] Initial bootstrap pushed to `origin/main` (commit `f958a52`).
- [x] Polish pass on branch `polish/pre-deploy-fonts-favicon-og`. PR open for your review.
- [x] `og-image.png` (1200x630) and `favicon.png` (32x32) rasterized via macOS `qlmanage` + `sips`. Both ship in the repo. The OG meta tags still point at the SVG, by request.
- [ ] Cloudflare Pages: connect repo, leave build command empty, build output `/`, production branch `main`.
- [ ] Cloudflare DNS: once the zone is moved over, point apex + `www` at the Pages project. Verify the OG meta URLs (`https://craftedbyhansen.com/og-image.svg`) actually resolve.
