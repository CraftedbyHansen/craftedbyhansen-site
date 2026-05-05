# Handoff: craftedbyhansen-site

Date: 2026-05-05
Branch: `main`

## What shipped

A flat repo with a single static one-pager.

```
craftedbyhansen-site/
├── index.html        # the one-pager (was CraftedbyHansen.com.html in the design zip)
├── README.md         # project + deploy
├── CLAUDE.md         # repo rules for Claude Code
├── .gitignore        # macOS junk, node_modules
└── Claude/Handoff/craftedbyhansen-site.md  # this note
```

The page is fully self-contained: inline CSS, no images, no `<script>`, no `<iframe>`, no local asset files. Everything lives in `index.html`.

## What was in the design zip and what I ignored

The zip is a *Design System* bundle, not just a one-pager. I only used the landing HTML. Everything else stayed out of the repo.

**Used:**

- `CraftedbyHansen.com.html`. Copied verbatim as `index.html`. Identical to `_export/CraftedbyHansen.com.src.html` minus a Claude artifact thumbnail `<template>` block (the export had it, the deployable version did not).

**Not used (intentionally left out of this repo):**

- `CraftedbyHansen.com (standalone).html`. A Claude artifact bundler wrapper around the same page, with `__bundler_loading` and `__bundler_thumbnail` script scaffolding. Not a deployable static page. Discarded.
- `colors_and_type.css`. Design system tokens and type helpers. The one-pager already inlines everything it needs from this. If you ever split the CSS out, this is the source of truth.
- `fonts/`. The four brand woff2 files (Self Modern regular/italic, Transcript regular/italic). **The one-pager does not use the brand fonts.** It uses EB Garamond + Work Sans from Google Fonts. See "Things to verify" below.
- `assets/logos/`. Wordmark + submark in seven brand colorways (SVG). Not referenced by the one-pager (the header uses a text wordmark with a CSS dot).
- `preview/`. Design system preview cards (color, type, spacing, components). Not part of the public site.
- `ui_kits/private-gallery/`. Full HTML/JSX recreation of the wedding-film delivery site (a different product, lives at `collection.andyhansenfilms.com`). Not for this repo.
- `uploads/`. Duplicates of the brand fonts and SVG logos, plus `Andy Hansen Style Guide.pdf`.
- `_scratch/`, `_export/`. Internal work files.
- `README.md`, `SKILL.md`. Design system docs (the skill manifest). Belong in the design system skill, not this repo.

The temp unzip dir is at `/var/folders/d1/qdktrp1n1clfvf27l9_v0b1h0000gn/T/cbh-design.1TGV5hzbF1/` and will get cleaned up by macOS. Nothing else to do there.

## Audit findings

- **No Claude / Anthropic / bundler / artifact watermarks** in `index.html`.
- **No `<img>`, `<script>`, or `<iframe>` tags.** Zero local asset references.
- **No broken refs.** There are no relative paths to break.
- **External URLs in the page:**
  - `https://fonts.googleapis.com` and `https://fonts.gstatic.com`. Google Fonts CSS + woff2.
  - `https://postcards.film`. Intentional outbound link in Projects.
  - `https://www.instagram.com/craftedbyhansen/`. Intentional outbound in Contact.
  - `mailto:hello@craftedbyhansen.com`. Intentional.
- **One CDN dep flagged:** Google Fonts (EB Garamond + Work Sans). Notes below.

## Things still open / to decide

1. **Google Fonts CDN vs. self-hosted.** The one-pager loads EB Garamond + Work Sans from Google Fonts. You said "no external CDN deps that should be local" but also "don't restyle the page." I left Google Fonts in place because self-hosting would require choosing the exact weights to download, fetching the woff2s, and writing `@font-face` declarations. Tell me if you want me to self-host the fonts and I'll do it as a follow-up.

   Side note: the brand fonts in the zip (Self Modern + Transcript) are **not** the fonts on the one-pager. EB Garamond + Work Sans are Google substitutes. If you want the brand fonts on the public site, that's a design change, not a hosting change.

2. **No favicon.** The page has no `<link rel="icon">`. You probably want one before launch. The `assets/logos/andyhansen_submark_*.svg` files in the design zip are good favicon source candidates.

3. **Portrait placeholder.** The About section shows a CSS gradient block labeled "Portrait · TK". Replace with a real image when you have one.

4. **No `<meta property="og:*">` tags.** Fine for a quiet launch but worth adding before sharing the link. Title + description + a 1200×630 social image.

5. **Footer year.** Hardcoded `© 2026`. Fine now, will need updating in January.

## What you should verify before deploying

- [ ] Open `index.html` in a browser. Confirm fonts load, hero looks right, links work, mobile media query behaves.
- [ ] On GitHub, confirm the `CraftedbyHansen/craftedbyhansen-site` repo exists and is empty (or empty enough to accept a fresh push to `main`).
- [x] Initial push to `origin/main` is done (one commit, `f958a52`). Remote was switched from SSH to HTTPS to match how `gh` is configured on this machine. The other CraftedbyHansen repos use HTTPS too.
- [ ] Decide on Google Fonts vs self-hosted before flipping DNS.
- [ ] Decide on favicon source before flipping DNS.
- [ ] Cloudflare Pages: connect repo, leave build command empty, build output `/`, production branch `main`.
- [ ] Cloudflare DNS: once the zone is moved over, point apex + `www` at the Pages project.
