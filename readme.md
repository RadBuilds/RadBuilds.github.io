# RadBuilds

Personal programming blog at [radbuilds.github.io](https://radbuilds.github.io).

## Commands

```bash
pnpm dev        # dev server at localhost:4321
pnpm build      # production build → dist/
pnpm preview    # preview production build locally
pnpm check      # type check
pnpm lint       # ESLint
```

## Adding a post

Create a `.mdx` file in `src/content/blog/en/`:

```markdown
---
title: 'Post title'
description: 'One-line description for SEO'
publishedAt: 2026-01-30
author: "Radosław Domański"
tags: ['tag1', 'tag2']
featured: false
locale: en
---

Content here...
```

## Configuration

- Site metadata: `src/config/site.config.ts`
- Navigation: `src/config/nav.config.ts`
- `.env` — `SITE_URL=https://radbuilds.github.io` (required for local build)

## Deployment

Push to `main` — GitHub Actions builds and deploys to GitHub Pages automatically.

# Credits
Built on [Astro Rocket](https://github.com/hansmartensdev/astro-rocket) — Astro 6, Tailwind CSS v4, deployed to GitHub Pages via GitHub Actions.
