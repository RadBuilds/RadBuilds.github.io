# Programming and Madness

Blog built with [Astro](https://astro.build), deployed to GitHub Pages automatically on push to `master`.

## Local dev

```bash
npm install
npm run dev      # http://localhost:4333
npm run build    # production build → dist/
```

## Adding a post

Create `src/content/blog/<slug>.md`:

```yaml
---
title: 'Post Title'
description: 'One sentence summary.'
pubDate: 'YYYY-MM-DD'
tags: ['tag1', 'tag2']
---
```
