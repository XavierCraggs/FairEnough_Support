# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Static HTML website for FairEnough — a sharehouse management iOS app. No build step, no framework, no dependencies. Live at `usefairenough.com`. Four files:

- `index.html` — public landing page
- `app-store/privacy.html` — Privacy Policy (linked from App Store listing)
- `app-store/support.html` — Support page (linked from App Store listing)
- `app-store/terms.html` — Terms of Service (added 2026-09-22; didn't exist before, despite the landing page promising one for months)

`support@usefairenough.com` is a real inbox — Cloudflare Email Routing, forwarding to a real address, configured in Cloudflare's dashboard rather than anything in this repo.

## Deploying

Push to `main` on GitHub. The site is served via GitHub Pages (or equivalent static host) from the repo root.

```bash
git add .
git commit -m "your message"
git push
```

## Architecture

All four pages share the same design tokens and font stack — defined in each file's `<style>` block (no shared CSS file). If you update colours or fonts, update all four files.

`index.html` has a full `@media (max-width: 768px)` block (added 2026-09-22) — it had none before and was unusable on a phone: the nav overlapped, the hero's three phone mockups overflowed, and the feature/pricing grids stayed locked at two columns regardless of viewport. The three-phone-mockup rows specifically cannot just shrink to fit — their internal text runs as small as 7px already at full size — so the hero keeps one phone and drops the other two on mobile, while the screenshots section (whose whole job is showing three different screens) scrolls horizontally at full size instead. The three `app-store/*.html` pages already had a basic breakpoint before this session; `index.html` did not.

### index.html structure

The page is divided into five named sections (`#features`, `#screenshots`, `#house-pass`, `#faq`) rendered in order:

1. **Hero** — blue gradient (`linear-gradient` from `#0f4eab` to `#4a90e0`), three phone mockups rendered via JavaScript
2. **Features** — four alternating text/visual rows (`.frow` / `.frow.rev`)
3. **Screenshots** — blue gradient background, three phone mockups
4. **House Pass** — two pricing cards (Free + $3.99 AUD/month, updated 2026-09-22 — previously said "Coming Soon" despite the subscription already selling live in the app; also previously listed two features, multiple houses and calendar sync, that don't exist yet — removed rather than left as a false claim on a page selling a real subscription)
5. **FAQ** — accordion via `.fi.open` toggle

Phone mockup screens (home, finance, chores) are rendered entirely in JavaScript at the bottom of `index.html`. The `home()`, `finance()`, and `chores()` functions write inline-styled HTML into named `div` IDs. If you add a new screen slot, give it an id and call the appropriate function on it.

### Design tokens (CSS custom properties)

```
--accent: #1A73E8   (primary blue)
--ok:     #1D8B5A   (green / positive amounts)
--muted:  #687387   (secondary text)
--border: #DCE3EF
--head:   'Plus Jakarta Sans'
--body:   'Manrope'
```

Phone mockup internals use a separate colour object `C` in JS (purple-toned: `#A78BFA` accent) — this reflects the app's actual UI theme, distinct from the website's blue.
