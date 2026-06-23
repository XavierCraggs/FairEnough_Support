# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Static HTML website for FairEnough — a sharehouse management iOS app. No build step, no framework, no dependencies. Three files:

- `index.html` — public landing page
- `app-store/privacy.html` — Privacy Policy (linked from App Store listing)
- `app-store/support.html` — Support page (linked from App Store listing)

## Deploying

Push to `main` on GitHub. The site is served via GitHub Pages (or equivalent static host) from the repo root.

```bash
git add .
git commit -m "your message"
git push
```

## Architecture

All three pages share the same design tokens and font stack — defined in each file's `<style>` block (no shared CSS file). If you update colours or fonts, update all three files.

### index.html structure

The page is divided into five named sections (`#features`, `#screenshots`, `#house-pass`, `#faq`) rendered in order:

1. **Hero** — blue gradient (`linear-gradient` from `#0f4eab` to `#4a90e0`), three phone mockups rendered via JavaScript
2. **Features** — four alternating text/visual rows (`.frow` / `.frow.rev`)
3. **Screenshots** — blue gradient background, three phone mockups
4. **House Pass** — two pricing cards (Free + Coming Soon)
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
