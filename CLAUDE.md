# CLAUDE.md - Parry Marketing Website

Marketing website for Parry, an AI-powered mobile app for text message reply suggestions.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Framework | Astro 5 (static output, zero JS) |
| Styling | Tailwind CSS v4 (CSS-based `@theme` config) |
| Icons | `@phosphor-icons/core` via `<Icon>` component |
| Fonts | Google Fonts: Sora (headings), Outfit (body) |
| Hosting | Cloudflare Pages (planned) |
| Domain | parry.app |

## Commands

```bash
npm run dev         # Start dev server (localhost:4321)
npm run build       # Build to dist/ (static HTML/CSS)
npm run preview     # Preview built site
```

## Project Structure

```
src/
├── layouts/
│   └── BaseLayout.astro        # Shared <html>, fonts, meta, bg gradient
├── pages/
│   ├── index.astro             # Landing page
│   ├── privacy.astro           # Privacy policy
│   ├── terms.astro             # Terms of service
│   └── 404.astro               # Custom 404 page
├── components/
│   ├── Header.astro            # Logo + nav
│   ├── Footer.astro            # Links, copyright, support email
│   ├── Hero.astro              # Headline, CTA, phone mockup
│   ├── HowItWorks.astro        # 3-step section
│   ├── Features.astro          # Feature cards
│   ├── SocialProof.astro       # Ratings/press section
│   ├── Privacy.astro           # Trust/privacy section
│   ├── DownloadCTA.astro       # App Store/Play Store badges
│   ├── FAQ.astro               # <details>/<summary> accordion
│   ├── PhoneMockup.astro       # CSS phone frame with mock app UI
│   └── Icon.astro              # Phosphor icon loader (build-time SVG)
├── styles/
│   └── global.css              # Tailwind @theme tokens, utilities, animations
public/
├── favicon.svg
├── favicon.ico
├── og-image.svg                # Placeholder (needs real PNG)
├── app-store-badge.svg         # Placeholder (not official Apple asset)
├── google-play-badge.svg       # Placeholder (not official Google asset)
├── llms.txt                    # AI crawler discoverability
└── robots.txt
```

## Design System

Design tokens originate from the mobile app at `~/Documents/parry/src/theme/`.
Full mobile app docs at `~/Documents/parry/docs/` (start with `docs/00-OVERVIEW.md`).

All color, font, and radius tokens are defined in `src/styles/global.css` inside the
`@theme {}` block and are usable as Tailwind utilities (e.g. `text-text-secondary`, `bg-surface`).

Web-specific notes:
- **text-secondary** was bumped from the mobile app's `0.65` opacity to `0.72` for WCAG AA on dark backgrounds.
- **glass-card** (Header, PhoneMockup) includes `backdrop-filter: blur()`. Expensive on mobile.
- **glass-card-lite** (all other cards) has the same look but skips `backdrop-filter` for performance.

## Conventions

- All icons via `<Icon name="..." />`, never inline SVG.
- Decorative icons: `text-warm-gold`. Interactive icons: `text-accent`.
- Headings: `font-[var(--font-sora)]`. Body text uses Outfit (set on `<body>`).
- Cross-page anchor links use absolute paths (`/#download`, not `#download`).
- Legal pages (`/privacy`, `/terms`) use the `prose-legal` class for consistent typography.

## Critical URLs

The Parry mobile app links to these from its settings screen:
- `https://parry.app/privacy` -- Privacy Policy
- `https://parry.app/terms` -- Terms of Service

**These routes MUST work and contain the correct legal text.**

## Gotchas

- **Tailwind v4** uses CSS-based `@theme {}` in `global.css`, not a JS config file.
- **Icon.astro** reads SVGs from `@phosphor-icons/core` at build time via `createRequire`.
  Names are kebab-case (`shield-check`, `clipboard-text`). Verify names at phosphoricons.com.
- **og:image** meta references `og-image.png` but only `og-image.svg` placeholder exists.
- **Store badges** are placeholder SVGs, not official Apple/Google assets.
- **canonicalPath** prop on BaseLayout is optional; omit it for the 404 page.
