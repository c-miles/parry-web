# Parry Web

Marketing and landing page for [Parry](https://parryai.app), an AI-powered mobile app that suggests text message replies. Built as a static site with Astro. Also hosts the privacy policy and terms of service pages linked directly from the mobile app.

## Tech Stack

- **Astro 5** -- static HTML/CSS output, zero client-side JavaScript
- **Tailwind CSS v4** -- CSS-based `@theme` configuration
- **Phosphor Icons** -- light-weight SVGs loaded at build time
- **Cloudflare Pages** -- hosting (planned)

## Getting Started

**Prerequisites:** Node.js 18+

```bash
npm install         # Install dependencies
npm run dev         # Start dev server at localhost:4321
npm run build       # Build static site to dist/
npm run preview     # Preview the production build
```

## Pages

| Route | Description |
|-------|-------------|
| `/` | Landing page with hero, features, how it works, FAQ, and download CTAs |
| `/privacy` | Privacy policy (linked from the Parry mobile app) |
| `/terms` | Terms of service (linked from the Parry mobile app) |
| `/404` | Custom 404 error page |

## Project Structure

```
src/
├── components/     # Astro components (Header, Hero, Features, FAQ, etc.)
├── layouts/        # BaseLayout with shared HTML shell, fonts, meta tags
├── pages/          # File-based routing (index, privacy, terms, 404)
└── styles/         # global.css with Tailwind @theme tokens and utilities
public/             # Static assets (favicons, badges, robots.txt, llms.txt)
```

## Design

The visual design matches the Parry mobile app, using the same color palette, glassmorphism cards, and typography. Design tokens originate from the mobile app's theme files in the sibling repo at `~/Documents/parry/`.
