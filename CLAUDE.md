# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Jorge Casas Universe** is a personal portfolio website for Jorge Andrés Casas Acero (digital transformation consultant, Bogotá, Colombia). It uses an interactive solar system metaphor as navigation.

Live URL: https://jac-universe-web.vercel.app/

## Key Fact: No Build Process

The entire site lives in a **single file: `index.html`** (~395KB, ~4800 lines). All HTML, CSS, and JavaScript are inline. There are no build tools, no package.json, no bundler, no TypeScript, no linter. To deploy, simply push to `main` — Vercel auto-deploys.

```bash
# Push to deploy
git push origin main
```

Opening `index.html` directly in a browser works without a local server.

## Repository Structure

```
index.html         # The entire application (HTML + inline CSS + inline JS)
DESIGN_SYSTEM.md   # Design tokens, components, performance specs, roadmap
manifest.json      # PWA configuration
vercel.json        # Deployment routing and security headers
sitemap.xml        # SEO sitemap
og-image.png       # Social media preview (1200×630)
404.html           # Custom error page
icons/             # PWA icons (8 sizes: 72–512px)
```

## Architecture: Solar System Navigation

The navigation is a canvas-rendered solar system with 11 clickable planets across 3 orbits. Each planet maps to a page section rendered as a `.pg` div below the canvas.

| Orbit | Radius | Planets |
|-------|--------|---------|
| 0 (center) | — | Acerca (About) |
| 1 | 240px | Expertise, Blog |
| 2 | 380px | Arsenal IA, Lab IA, Cursos |
| 3 | 520px | Servicios, Membresía, Streaming, Directorio, Contacto |

**Canvas layer:** Two separate renderers run on a single `<canvas>`:
- **Starfield:** 3 static star layers (280, 38, 7 stars)
- **Flow field:** 220 live particles with generative art, seeded with `"JORGE"` (numeric value 330). Mouse repels particles within a 140px radius. Canvas uses dirty-checking — only redraws if particle moved > 0.3px.

**Page navigation:** Clicking a planet calls `showPage(id)` which fades out the current `.pg`, fades in the target, and announces the change via an `aria-live` region. URL hash updates accordingly.

## Theme System

Three themes toggled via a panel in the bottom-right corner, persisted in `localStorage`:

| Theme | Key CSS vars |
|-------|-------------|
| **Cre8tera Dark** (default) | `--bg: #060400`, `--gold: #c9a84c`, `--white: #ede9e3` |
| **Stellar Light** | `--bg: #f5f0e8`, `--gold: #8a5e00`, `--white: #1a1208` |
| **Alto Contraste** | `--bg: #ffff00`, `--gold: #000`, `--white: #000` |

All color usage should go through CSS custom properties — never hardcode hex values in new styles.

## Typography

| Use | Font | Weight | Size |
|-----|------|--------|------|
| Headings h1/h2 | Cormorant Garamond | 300/400 | `clamp(1.6rem, 2.8vw, 2.6rem)` |
| Headings h3 | Cormorant Garamond | 400 | `1.32rem` |
| Body | Cormorant Garamond | 300 | `0.96rem` |
| UI / monospace | Space Mono | 400 | `0.48–0.72rem` |
| Navigation / labels | Syne | 700 | `0.84rem` |

## Key Components (CSS Classes)

- `.pg` — page section container; hidden by default, shown via JS
- `.rev` — scroll-reveal element (fade + translateY via IntersectionObserver)
- `.tilt` — card with hover micro-elevation (`translateY(-3px) scale(1.012)`)
- `.feat` — featured card with darker background (used in testimonials)
- Orbit rings use `::before`/`::after` for inner ring and outer glow

## Accessibility Requirements

All interactive additions must maintain WCAG 2.1 AA compliance:
- `aria-label` on all planets, buttons, and form inputs
- `tabindex="0"` + `Enter`/`Space` keyboard handlers on custom interactive elements
- Minimum 44×44px tap targets
- `@media (prefers-reduced-motion: reduce)` must disable animations
- Focus styles: 2px gold outline on all interactive elements

## Content Language & Voice

All user-facing copy is in **Spanish** (neutral Colombian, no local slang). Tone is professional, direct, and warm — not flowery.

- ❌ "¡Hola, bienvenido a mi increíble universo digital!"
- ✅ "Explora mi trabajo en transformación digital y IA"

## Directorio (AI Tools Directory)

The Arsenal IA section lists 107 curated AI tools with:
- Category filter + text search (both active simultaneously)
- Price badges: `Gratis` (green), `Freemium` (gold), `De pago` (red)
- 16 categories (LLMs, Agentes, Video, Imagen, Audio, Educación, etc.)

## Contact Form

The contact form uses **Formspree** (not mailto). The form action points to the live Formspree endpoint — do not replace it with a mailto link.

## Performance Targets

| Metric | Target | Current |
|--------|--------|---------|
| FCP | <1.5s | ~1.2s |
| LCP | <2.5s | ~1.8s |
| CLS | <0.1 | 0.04 |
| TTI | <3.5s | ~2.3s |

Avoid adding external JS libraries or blocking resources — the single-file, zero-dependency approach is intentional.

## Deployment

Vercel auto-deploys on every push to `main`. Config in `vercel.json`:
- `cleanUrls: true` (no `.html` extensions)
- Security headers: `X-Frame-Options`, `X-Content-Type-Options`, `X-XSS-Protection`, `Referrer-Policy`
- Cache: `index.html` → 1 hour; images/sitemap → 24 hours

Browser support target: Chrome, Firefox, Safari, Edge (latest 2 versions). Mobile-first responsive from 375px.
