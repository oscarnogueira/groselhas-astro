# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev        # Start dev server at localhost:4321
npm run build      # Build production site to ./dist/
npm run preview    # Build and preview locally (uses wrangler dev)
npm run check      # Full validation: build + tsc + wrangler dry-run deploy
npm run deploy     # Deploy to Cloudflare Workers
npm run cf-typegen # Regenerate Cloudflare Worker types
```

## Architecture

This is an Astro blog deployed on **Cloudflare Workers** via the `@astrojs/cloudflare` adapter. It uses server-side rendering with Cloudflare's runtime.

### Key Files

- `src/consts.ts` — Site-wide constants (`SITE_TITLE`, `SITE_DESCRIPTION`)
- `src/content.config.ts` — Content collection schema (Zod-validated blog frontmatter)
- `src/env.d.ts` — Cloudflare runtime type declarations
- `astro.config.mjs` — Astro config (site URL, integrations, Cloudflare adapter)
- `wrangler.json` — Cloudflare Workers config (assets dir, compatibility flags)

### Routing

File-based routing from `src/pages/`:
- `index.astro` → `/`
- `about.astro` → `/about`
- `blog/index.astro` → `/blog` (sorted listing)
- `blog/[...slug].astro` → `/blog/[slug]` (dynamic, generated via `getStaticPaths()`)
- `rss.xml.js` → `/rss.xml`

### Content

Blog posts live in `src/content/blog/` as `.md` or `.mdx` files. The filename becomes the URL slug. Required frontmatter: `title`, `description`, `pubDate`. Optional: `updatedDate`, `heroImage`.

### Styling

Global styles in `src/styles/global.css` (Bear Blog-inspired minimalist). CSS custom properties for theming (`--accent`, `--black`, `--gray-dark`, etc.). Component-scoped styles via Astro `<style>` blocks. Mobile breakpoint at 720px.

### Layouts & Components

- `src/layouts/BlogPost.astro` — Wraps individual blog posts
- `src/components/BaseHead.astro` — SEO metadata (canonical, OG, Twitter tags)
- `src/components/Header.astro` / `Footer.astro` — Site chrome
- `src/components/HeaderLink.astro` — Nav link with active-state detection
