# craftedbyhansen-site

The marketing one-pager for **Crafted by Hansen, LLC** at [craftedbyhansen.com](https://craftedbyhansen.com).

Static HTML, no framework, no build step. All public-facing files live in `public/` (the one-pager, favicon, OG image, self-hosted brand fonts under `public/assets/fonts/`). The site deploys as a **Cloudflare Worker** with static assets, configured in `wrangler.jsonc` at the repo root.

## Local preview

Serve the `public/` directory with any static server:

```sh
python3 -m http.server 8000 --directory public
# then visit http://localhost:8000
```

## Deploy

```sh
npx wrangler deploy
```

`wrangler.jsonc` at the repo root names the Worker `craftedbyhansen-site` and binds `./public` as the static asset directory. The production branch is `main`. DNS for `craftedbyhansen.com` is moving to Cloudflare in parallel. Once the zone is on Cloudflare, route the apex and `www` to this Worker.

## Editing

Voice rules for any copy on the page:

- No em dashes.
- No semicolons.
- No banned-AI words (delve, leverage, foster, robust, seamless, and friends).

Don't restyle without intent. The visual system comes from the *Crafted by Hansen Design System* skill.
