# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the app

There is no build step. Open `index.html` in a browser or serve it with any static HTTP server:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

React 18 and Babel standalone are loaded from CDN. JSX is transpiled in-browser — no `npm install` or bundler needed.

## Architecture

This repo contains two things:

**Root — App UI Kit**
A click-through prototype of the NOURI mobile app. `index.html` bootstraps a React app by loading JSX files as `<script type="text/babel">` tags in this order:

1. `ios-frame.jsx` — `IOSDevice` wrapper component
2. `primitives.jsx` — shared design tokens (`N` object) + primitive components (`Icon`, `Eyebrow`, `Button`, `Chip`, `Card`, `Ring`, `TabBar`)
3. Screen files: `HomeScreen.jsx`, `CheckInScreen.jsx`, `TrackScreen.jsx`, `LearnScreen.jsx`, `CommunityScreen.jsx`, `LabsScreen.jsx`

All components are exposed as globals via `Object.assign(window, {...})` at the bottom of each file — this is intentional, not a bug, because there is no module system. The root `App` component lives directly in an inline `<script type="text/babel">` in `index.html`.

**`NOURI Design System/` — Brand & Design System**
The authoritative brand source. Key files:
- `README.md` — full brand guidelines, content rules, visual foundations, iconography
- `colors_and_type.css` — all CSS custom properties (colors, type scale, spacing, radii, shadows, motion)
- `preview/` — standalone HTML cards rendering individual design tokens (01–25)
- `SKILL.md` — Claude Code skill manifest; invoke with `/nouri-design` for brand context

The root `colors_and_type.css` is a copy of `NOURI Design System/colors_and_type.css`. Keep them in sync.

## Design tokens

The `N` object in `primitives.jsx` mirrors the CSS variables in `colors_and_type.css` for use in inline styles:

```js
N.navy, N.paper, N.purple, N.teal   // brand colors
N.serif, N.sans, N.mono              // font stacks
N.border, N.ink, N.ink500, N.ink300 // neutrals
```

## Brand conventions (apply to all UI copy and new components)

- **Language**: Spanish, neutral Latin American. Always `tú`, never `usted`.
- **No emoji** anywhere — not in UI, push, or marketing.
- **Typography**: Instrument Serif for display (≥26px only), Geist for UI/body, Geist Mono for data.
- **Icons**: Lucide-style inline SVGs, `strokeWidth={1.5}`, `size` on 4px rhythm (16/20/24/32).
- **Backgrounds**: Default to `--paper` (`#FBF7F1`), not white. Dark sections use `--nouri-navy`.
- **Radius**: Never `0` or `4px`. Use `--r-sm` (6px) through `--r-pill` (999px).
- **Motion**: `cubic-bezier(0.22, 0.61, 0.36, 1)`, no spring/bounce. Durations: 140ms (hover), 260ms (enter/exit), 480ms (modal).
- **Buttons**: Sentence case, verb-first (e.g., *Agregar nota*).

## Deployment

Deployed to GitHub Pages as a static site. `index.html` at root is the entry point.
