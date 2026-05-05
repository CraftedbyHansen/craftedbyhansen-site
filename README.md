# craftedbyhansen-site

The marketing one-pager for **Crafted by Hansen, LLC** at [craftedbyhansen.com](https://craftedbyhansen.com).

Static HTML, no framework, no build step. The whole site is a single `index.html` with inline CSS.

## Local preview

Open `index.html` directly in a browser, or serve the directory with any static server:

```sh
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploy

Deployment target is **Cloudflare Pages**.

1. Connect this repo to a Cloudflare Pages project.
2. Build command: leave empty.
3. Build output directory: `/` (repo root).
4. Production branch: `main`.

DNS for `craftedbyhansen.com` is moving to Cloudflare in parallel. Once the zone is on Cloudflare, point the apex (`craftedbyhansen.com`) and `www` at the Pages project.

## Editing

Voice rules for any copy on the page:

- No em dashes.
- No semicolons.
- No banned-AI words (delve, leverage, foster, robust, seamless, and friends).

Don't restyle without intent. The visual system comes from the *Crafted by Hansen Design System* skill.
