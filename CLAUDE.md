# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
pnpm dev          # Start dev server
pnpm build        # Production build
pnpm preview      # Preview production build
pnpm check        # Astro type-check
pnpm lint         # ESLint
pnpm lint:fix     # ESLint with auto-fix
pnpm format       # Prettier (write)
pnpm format:check # Prettier (check only)
pnpm validate     # lint + check + build (full CI pass)
pnpm test         # Vitest unit tests
pnpm test:e2e     # Playwright e2e tests
```

Unit tests live in `src/__tests__/`. Run a single file: `pnpm test src/__tests__/i18n.test.ts`.

## Stack

- **Astro 6** — static output, deployed to GitHub Pages (`https://radbuilds.github.io`)
- **Tailwind CSS v4** — loaded as a Vite plugin (not the classic Astro integration); config in `tailwind.config.ts`
- **React 19** — used for interactive island components only; most UI is `.astro`
- **Content Layer API** — content collections defined in `src/content.config.ts` with Zod schemas
- **TypeScript** — strict mode; path alias `@/*` → `src/*`

## Architecture

### Configuration is centralised in `src/config/`

| File | Purpose |
|---|---|
| `site.config.ts` | Single source of truth: site URL, author, email, social links, branding, theme, Giscus config |
| `nav.config.ts` | Nav items (ordered) and footer links |
| `i18n.config.ts` | i18n disabled by default; supports en/nl/de/fr/es |
| `consent.config.ts` | Cookie consent settings |

All pages pull from `site.config.ts` — don't hardcode metadata anywhere else.

### Layouts wrap pages, not the other way around

```
BaseLayout        ← HTML shell, theme bootstrap, fonts, SEO, animations
  └── LandingLayout   ← for index.astro (hero, landing sections)
  └── PageLayout      ← generic content pages
  └── BlogLayout      ← blog posts (TOC, Giscus, related posts, share)
  └── ProjectLayout   ← project detail pages
  └── MarketingLayout ← marketing pages
```

`BaseLayout` owns: theme system (localStorage, 3-state light/dark/system), favicon color sync, reveal animations (Intersection Observer with stagger), cursor trail, back-to-top button with scroll ring, skip-to-content.

### Theme system

13 color themes (orange → magenta) defined as CSS files in `src/styles/themes/`. The active theme is set via a `data-theme` attribute on `<html>`. The default is configured in `site.config.ts` (`branding.themeColor`). Light/dark mode is handled separately via `data-appearance` on `<html>`.

### Content collections

**The primary content lives in `src/content/`** — this is the main thing being maintained in this repo. Blog posts are MDX files in `src/content/blog/en/`.

Schemas for all collections are defined in `src/content.config.ts`. Collections: `blog`, `pages`, `authors`, `faqs`, `projects`, `stack`. Drafts are excluded from production builds via the `draft` frontmatter field.

### Social links

`site.config.ts` stores raw profile URLs in `socialLinks[]`. The helper `resolveSocialLinks()` in `src/lib/utils.ts` parses them into `{ href, label, icon }` objects. Supported platforms: GitHub, X/Twitter, LinkedIn, Instagram, Bluesky.

### JSON-LD / SEO

Schema generators are in `src/lib/schema.ts`. Pass schema objects to `BaseLayout` via the `schemas` prop; the layout injects them as `<script type="application/ld+json">`. OG images are generated at build time via `src/pages/og/`.

### API routes

`src/pages/api/` contains server-side endpoints (contact form, newsletter). These use the Resend SDK — the `RESEND_API_KEY` env var must be set for forms to work.

### Styling conventions

- Global tokens: `src/styles/tokens/` (colors, spacing, typography, primitives)
- Global styles: `src/styles/global.css`
- Component-scoped `<style>` blocks for component-specific styles; use `is:global` only when styling needs to escape Astro's scoping
- `cn()` from `src/lib/cn.ts` for conditional class merging (wraps clsx + tailwind-merge)
- Button variants are defined in `src/components/ui/form/Button/button.variants.ts`; `btn-primary` is a global class applied via `is:global` in `Button.astro`

### Effects / animations

- `LetterGlitchBand` — desktop-only CTA background effect (hidden on mobile via `glitch:hidden`)
- `CursorTrail` — canvas cursor effect, enabled in BaseLayout
- Reveal animations — `data-reveal` attribute triggers fade-in via IntersectionObserver in BaseLayout
- Stagger index — set `--stagger-index` CSS var on elements to control animation order

---

## Owner & Positioning Context

This site belongs to **Radosław Domański** (Radek), a fractional CTO based in Poland.

### Who he is

15+ years from developer to tech lead to CTO, across telco, insurance, fintech, blockchain, and AI.
Not a stack specialist — a systems thinker. Breadth is the differentiator.

### What this site is for

Marketing site for his fractional CTO practice. Target audience: founders of late-stage startups
and early scale-ups (seed done, 3–10 engineers, no full-time CTO yet).

**Not** a personal blog or portfolio site — the blog supports thought leadership for the fCTO practice.

### Positioning (settled)

- **One-liner:** "I help early-stage startups and scale-ups build the right thing — before they've hired a full-time CTO."
- **Entry motion:** Technical Diagnostic Sprint (2 weeks, fixed scope and price)
- **Follow-on:** Ongoing retainer in three tiers (Strategic 1d/wk, Active 2d/wk, Embedded 3d/wk) — **no prices on the site**
- **Contact:** contact@radbuilds.eu

### Tech stack (his actual background)

TypeScript, Node.js, React, AWS, AI tooling (LangChain, ElevenLabs, Azure AI Foundry, OpenAI).

Proof points from real engagements:
- MonedaPay: DORA/MICA/Travel Rule compliance — architecture + legal + AML + development in parallel
- Evalyn: voice-based AI recruitment SaaS (ElevenLabs, Azure OpenAI, LangChain) from zero to multi-tenant
- Sonnar (NZ): delivery practices overhaul, enabling team to do more with fewer engineers

### Content rules

- No prices in copy — link to contact for pricing conversations
- Geography: Poland primary, EU secondary
- Voice: direct, no hype, no buzzwords. "Build the right thing" is the frame.

### Full fCTO context (for deeper questions)

The complete fCTO knowledge base lives in Radek's Obsidian vault. If you need more detail
than this file provides — positioning decisions, offer specifics, pipeline status, content drafts —
read these files directly:

| Need | File |
|------|------|
| Full positioning doc | `/Users/radek/work/knowledge-centre/domains/work/fCTO/positioning/positioning.md` |
| Full offer doc (scope, process, tiers) | `/Users/radek/work/knowledge-centre/domains/work/fCTO/positioning/offer.md` |
| Curated memory (decisions, pipeline, commitments) | `/Users/radek/work/knowledge-centre/_bmad/memory/chat-cto/MEMORY.md` |
| Roadmap & current phase | `/Users/radek/work/knowledge-centre/domains/work/fCTO/roadmap/roadmap.md` |

The fCTO agent (Forge / ChatCTO) owns that vault. If you're making copy or structural changes to
this site and need strategic alignment, check those files or note the question for Radek to resolve
with Forge in a separate session.
