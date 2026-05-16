# CLAUDE.md - parry Marketing Website

Marketing website for parry, an AI-powered mobile app for text message reply suggestions. Operated by Anomaly Labs LLC (parry is a registered DBA).

## Tech Stack

| Layer | Technology |
|-------|------------|
| Framework | Astro 5 (static output, zero JS by default) |
| Styling | Tailwind CSS v4 (CSS-based `@theme` config) |
| Icons | `@phosphor-icons/core` via `<Icon>` component |
| Fonts | Google Fonts: Fraunces (display serif), Geist (body sans-serif) |
| Hosting | Render (auto-deploys on push to `main`) |
| Domain | parryai.app (registered via Namecheap) |

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
│   └── BaseLayout.astro        # Shared <html>, fonts, meta, Organization JSON-LD
├── pages/
│   ├── index.astro             # Landing page (passes MobileApplication JSON-LD)
│   ├── privacy.astro           # Privacy policy
│   ├── terms.astro             # Terms of service
│   └── 404.astro               # Custom 404 page (no canonicalPath)
├── components/
│   ├── Header.astro            # Logo only (no nav CTA)
│   ├── Footer.astro            # Links, copyright, support email
│   ├── Hero.astro              # Headline, CTA, phone mockup, sentinel
│   ├── HowItWorks.astro        # 3-step section
│   ├── Features.astro          # Feature cards
│   ├── SocialProof.astro       # Claude AI attribution badge
│   ├── PainPoint.astro         # "Sound familiar?" empathy section
│   ├── TonesShowcase.astro     # WAI-ARIA tabs showing tones in action
│   ├── Privacy.astro           # Trust/privacy section
│   ├── DownloadCTA.astro       # App Store badge
│   ├── FAQ.astro               # <details>/<summary> + FAQPage JSON-LD
│   ├── MobileDownloadBar.astro # Fixed bottom bar, IntersectionObserver-gated
│   ├── PhoneMockup.astro       # CSS phone frame with placeholder app UI
│   └── Icon.astro              # Phosphor icon loader (build-time SVG)
├── styles/
│   └── global.css              # Tailwind @theme tokens, utilities, animations
public/
├── favicon.svg / favicon.ico / apple-touch-icon.png
├── og-image.png                # 1200×630
├── app-store-badge.svg         # Placeholder (not official Apple asset)
├── google-play-badge.svg       # Placeholder (not official Google asset)
├── brand/                      # Wordmark and logo variants
├── llms.txt                    # AI crawler discoverability
└── robots.txt                  # AI crawler allowlist + scraper blocklist
```

## Design System — Editorial Dark

Independent design system. Diverged from the mobile app's glassmorphism in the PR #7 redesign; tokens now originate directly in `src/styles/global.css`, not from the mobile app's theme files.

Key tokens (all defined in `@theme {}` inside `global.css`, usable as Tailwind utilities):

- **Background:** `--color-bg-base: #161220` (warm plum-tinted near-black, solid — no gradient)
- **Accent:** `--color-accent: #E8734A` (single coral accent; `--color-accent-text: #F39C7A` for body-text occurrences)
- **Text:** `--color-text-primary` (white), `--color-text-secondary` (white 0.78), `--color-text-tertiary` (white 0.55)
- **Hairlines:** `--color-hairline` (white 0.08) instead of card shadows
- **Fonts:** `--font-display: Fraunces`, `--font-body: Geist`
- **Type scale:** `--text-hero`, `--text-display`, `--text-section`, `--text-eyebrow` (clamp-based for fluid scaling)

Surface primitives (utility classes in `global.css`):

- `site-header` — fixed translucent header with hairline-bottom (no `backdrop-filter`)
- `surface-elevated` — rare inset card surface
- `hairline-top` / `hairline-bottom` — single-pixel dividers
- `eyebrow` — small-caps section label
- `btn-primary` — solid coral pill button
- `hero-glow` — single low-intensity radial behind Hero
- `prose-legal` — typography wrapper for `/privacy` and `/terms`
- `font-display` — Fraunces with letter-spacing and optical sizing

## Conventions

- All icons via `<Icon name="..." />`, never inline SVG.
- Headings use `font-display`. Body text uses Geist (set on `<body>`).
- Cross-page anchor links use absolute paths (`/#download`, not `#download`).
- Legal pages (`/privacy`, `/terms`) use the `prose-legal` class.
- `BaseLayout` props: `title`, `description`, `canonicalPath?`, `jsonLd?`, `showMobileDownloadBar?`. Omit `canonicalPath` for the 404 page.

## Critical URLs

The parry mobile app currently links `https://parryai.app/privacy` from its AI consent modal (`AIConsentModal.tsx`). Once a Settings screen ships there, `/terms` will be linked too. **These routes MUST work and contain the correct legal text** — they are referenced from the App Store listing and the in-app consent flow.

## Legal entity

- Operating entity: **Anomaly Labs LLC** (Washington state, formed April 2026)
- DBA: **parry**
- Legal pages (`privacy.astro`, `terms.astro`) use "Anomaly Labs LLC" for legal-claim contexts (operator, owner, indemnification, copyright) and keep "parry" for brand-voice references.

## Structured data

- `Organization` JSON-LD ships sitewide via `BaseLayout.astro` (legalName = Anomaly Labs LLC, name = parry).
- `MobileApplication` JSON-LD ships on `/` via the `jsonLd` prop in `index.astro`.
- `FAQPage` JSON-LD ships on `/` from `FAQ.astro`, sourced from the same `faqs` array.

## Gotchas

- **Tailwind v4** uses CSS-based `@theme {}` in `global.css`, not a JS config file.
- **Icon.astro** reads SVGs from `@phosphor-icons/core` at build time via `createRequire`. Names are kebab-case (`shield-check`, `clipboard-text`). Verify at phosphoricons.com.
- **`apple-itunes-app`** meta in `BaseLayout.astro` uses placeholder `app-id=0000000000` — swap once the App Store Connect record is created (numeric ID is assigned then, no review needed).
- **Store badges** are placeholder SVGs, not official Apple/Google assets. Tracked in issue #9.
- **Phone mockup** uses placeholder content. Real screenshots tracked in issue #9.
- **`canonicalPath`** is optional; omit it for the 404 page (and the JSON-LD `og:url` will also be omitted, which is correct behavior).
