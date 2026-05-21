# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
bundle install          # install dependencies
bundle exec jekyll serve  # local dev server (http://localhost:4000)
```

Deployment: push to `master` — GitHub Pages builds and deploys automatically.

## Architecture

Jekyll static site using the `pages-themes/hacker` remote theme, hosted on GitHub Pages.

- `_posts/` — blog posts in Markdown, filename format: `YYYY-MM-DD-slug.md`
- `_config.yml` — site title, author, theme, plugins
- `assets/css/style.scss` — custom styles on top of the remote theme
- `index.md`, `about.md` — top-level pages

### Post front matter

```yaml
---
layout: post
title: "Post Title"
date: YYYY-MM-DD HH:MM:SS +ZZZZ
categories: tag1 tag2
---
```

The site uses `kramdown` as the Markdown processor and `jekyll-feed` for the RSS feed.
