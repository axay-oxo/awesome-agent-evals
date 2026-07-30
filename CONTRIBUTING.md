# Contributing

Thanks for helping keep this list useful and current. Additions, corrections, and fixes are all welcome.

## Add a tool

This list is **data-driven** — the README and the site are generated from [`data/tools.json`](data/tools.json). **Edit that file, not the README** (the README's list section is overwritten by the build).

1. Add one object to `data/tools.json`:
   ```json
   {
     "name": "Tool Name",
     "repo": "owner/repo",
     "url": "https://github.com/owner/repo",
     "category": "<a valid category>",
     "desc": "One honest sentence: what it does and who it's for."
   }
   ```
   Include `repo` (`owner/name`) when it's on GitHub so the star count auto-populates.
2. Regenerate:
   ```bash
   node scripts/generate.mjs      # rebuilds the README block and the site
   ```
3. Open a PR using the template.

## Rules

- **Open-source or genuinely useful** — no pure marketing, no paid placements.
- **No dead links.** Prefer tools you've actually used.
- **One honest sentence** per entry; match the tone of the existing descriptions.
- Put it in the right `category`. If none fits, mention it in the PR.

New to open source? Adding one good entry here is a perfectly good first PR.

## Fixes

Broken link, wrong description, or a tool that's been archived? Open an issue or a PR against `data/tools.json`.
