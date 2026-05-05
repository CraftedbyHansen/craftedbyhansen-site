# CLAUDE.md

Guidance for Claude Code in this repo.

## What this is

The marketing one-pager for **craftedbyhansen.com**. A single static `index.html` at the repo root, with inline CSS. No framework, no build step, no backend, no analytics, no forms.

## Deployment

Cloudflare Pages, production branch `main`. No build command, output is the repo root.

## Working rules

- **Initial repo bootstrap (first commit + push to main) is allowed.** After that, all changes ship via PR with my review before merge.
- **No merging to `main` without my review.** Don't merge PRs on my behalf.
- Don't add a build system, package manager, or framework without asking. The whole point is that this is one file.
- Don't add tracking, analytics, or third-party scripts.
- The design comes from the *Crafted by Hansen Design System* skill. If you need to reference brand tokens or components, load that skill.

## Voice rules for any copy you touch

- No em dashes.
- No semicolons.
- No banned-AI words (delve, leverage, foster, robust, seamless, etc.).
- Don't rewrite design copy unless I ask.
