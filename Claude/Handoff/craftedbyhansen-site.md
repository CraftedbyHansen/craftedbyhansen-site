# Handoff: craftedbyhansen-site

Date: 2026-05-05
Branch: latest work on `polish/pre-deploy-fonts-favicon-og` (PR open against `main`).

## What shipped

A flat repo with a single static one-pager and a small assets folder.

```
craftedbyhansen-site/
├── index.html        # the one-pager
├── favicon.svg       # rust submark, used as browser tab icon
├── og-image.svg      # 1200x630 brown wordmark on cream, for social unfurls
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
- `assets/logos/andyhansen_submark_rust.svg`. Copied to `/favicon.svg`. Picked rust because the site's accent (`#A8553A`) is a terracotta close to the brand rust (`#B7764A`).
- `assets/logos/andyhansen_logo_brown.svg`. Embedded as paths inside a hand-built `og-image.svg` (1200x630 cream canvas, brown wordmark centered). See "Things still open" below for a wrinkle.
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

1. **OG image PNG fallback.** I shipped `og-image.svg` only. Tried to generate `og-image.png` (1200x630) via `npx sharp-cli` and a permissions hook blocked the npx call. **Twitter / X is finicky with SVG OG images and may not unfurl them.** You'll want to generate the PNG manually before announcing widely. Open `og-image.svg` in any browser, screenshot or "Save as image", or run something like:

   ```
   npx sharp-cli -i og-image.svg -o og-image.png resize 1200 630
   ```

   If you ship `og-image.png`, also update the two `og:image` / `twitter:image` meta tags in `index.html` to point at `/og-image.png`.

2. **Favicon PNG fallback.** Same story for `favicon.png` (32x32). Modern browsers all read SVG favicons fine, so this is lower priority. Add only if you want a Safari-old / legacy fallback.

3. **The OG image wordmark says "andyhansen", not "Crafted by Hansen".** That's because the only wordmark SVG in the design system is for Andy Hansen Films (the parent brand). For accurate unfurls on the Crafted by Hansen site, you may want a custom "Crafted by Hansen" wordmark composed and rasterized. Until then, the andyhansen wordmark is the closest on-brand option you have, and the OG title text ("Crafted by Hansen") will appear next to the image in the unfurl card.

4. **Portrait placeholder.** The About section still shows a CSS gradient block labeled "Portrait · TK". Replace with a real image when you have one.

5. **Footer year.** Hardcoded `© 2026`. Fine now, will need updating in January.

## What you should verify before deploying

- [ ] Serve the repo locally (`python3 -m http.server 8000` from the repo root, then open `http://localhost:8000`). **Do not open `index.html` via `file://`** because the absolute paths (`/favicon.svg`, `/assets/fonts/...`) only resolve from a server root.
- [ ] Confirm Self Modern shows on headings / about copy / contact email link, and Transcript shows on body / labels / wordmark / footer. Compare against the design system preview.
- [ ] Confirm favicon shows in the browser tab.
- [ ] Open devtools, confirm no 404s on font files, no console errors.
- [ ] View source, confirm `<meta property="og:*">` and `<meta name="twitter:*">` tags are in `<head>`.
- [x] Initial bootstrap pushed to `origin/main` (commit `f958a52`).
- [x] Polish pass on branch `polish/pre-deploy-fonts-favicon-og`. PR open for your review.
- [ ] Generate `og-image.png` (1200x630) before announcing publicly. Twitter especially.
- [ ] Cloudflare Pages: connect repo, leave build command empty, build output `/`, production branch `main`.
- [ ] Cloudflare DNS: once the zone is moved over, point apex + `www` at the Pages project. Verify the OG meta URLs (`https://craftedbyhansen.com/og-image.svg`) actually resolve.
