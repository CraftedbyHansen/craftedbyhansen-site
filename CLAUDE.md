# CLAUDE.md

Guidance for Claude Code in this repo.

## What this is

The marketing one-pager for **craftedbyhansen.com**. Static HTML, inline CSS, self-hosted brand fonts. No framework, no backend, no analytics, no forms.

## Deployment

Deploys as a **Cloudflare Worker** with static assets, configured in `wrangler.jsonc` at the repo root. Production branch is `main`. The Worker name is `craftedbyhansen-site` and must stay that way (Cloudflare matches by name on deploy).

All public-facing files live in `public/`. The `assets.directory` in `wrangler.jsonc` points there. Anything outside `public/` (this CLAUDE.md, the README, the handoff doc) is repo-internal and not served.

## Working rules

- **Initial repo bootstrap (first commit + push to main) is allowed.** After that, all changes ship via PR with my review before merge.
- **No merging to `main` without my review.** Don't merge PRs on my behalf.
- **Static assets only. Don't add server-side Worker code unless I ask.** No `src/index.ts`, no fetch handlers, no bindings beyond the assets binding. The whole point is that this is a static site served from a Worker.
- Don't add a build system, package manager, or framework without asking.
- Don't add tracking, analytics, or third-party scripts.
- The design comes from the *Crafted by Hansen Design System* skill. If you need to reference brand tokens or components, load that skill.

## Voice rules for any copy you touch

- No em dashes.
- No semicolons.
- No banned-AI words (delve, leverage, foster, robust, seamless, etc.).
- Don't rewrite design copy unless I ask.
