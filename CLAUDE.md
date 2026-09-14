# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page static marketing site for **Benchmark Accessibility**, Dean Bowles' consulting practice for Section 508 / WCAG 2.1 AA accessibility audits and remediation (primarily for Drupal). There is no backend, no build tooling, and no JavaScript — the entire site is `index.html` plus one stylesheet plus static image assets.

## Commands

There is no package manager, build step, linter, or test suite in this repo (no `package.json`, no config files of any kind besides the source files themselves).

To preview changes locally, serve the directory as static files, e.g.:

```bash
python3 -m http.server 8000
# then open http://localhost:8000/
```

Opening `index.html` directly in a browser also works since there are no server-side dependencies, but paths in `index.html` reference `/assets/...` as root-relative, so a local server is more accurate than `file://`.

## Architecture

- **`index.html`** — the entire site. One HTML document with semantic `<section>` landmarks (`#top`, `#why`, `#about`, `#services`, `#process`, `#accessibility`, `#contact`), each with an `aria-labelledby` tied to its heading. Sticky header nav with anchor links to these sections. No JS-driven behavior — all interactivity is native browser anchor scrolling, `mailto:` links, and CSS `:focus-visible`.
- **`assets/css/styles.css`** — all styling, in one file. Key structure:
  - CSS custom properties on `:root` define the color system (`--paper`, `--ink`, `--accent`, etc.), overridden inside `@media (prefers-color-scheme: dark)` for automatic dark mode — there is no manual theme toggle.
  - `@media (prefers-reduced-motion: reduce)` disables smooth scrolling and all animations/transitions.
  - Three Google Fonts loaded via `<link>` in `index.html`'s `<head>`: **Archivo** (headings/nav/buttons/labels), **Newsreader** (body serif), **IBM Plex Mono** (microcopy, prices, stats).
  - Layout is a single centered column (`.wrap`, `max-width: 820px`) reused across every section — no grid/flex page framework beyond that.
- **`assets/images/`** — the "Benchmark" logo pack (SVG + PNG variants: horizontal logo, reverse/dark variants, seal, icon, favicons). `README.txt` in this folder documents which file is intended for which use case (favicon, social avatar, print, email signature, etc.) and the brand colors/type.
- **`assets/scripts/`** — currently empty; present as a placeholder for future JS, not referenced by `index.html`.

## Working conventions specific to this site

The site's product is accessibility expertise, so the markup and CSS are intentionally written as a demonstration of WCAG 2.1 AA conformance — this is called out explicitly in the `#accessibility` section of the page itself. When editing, preserve these patterns rather than simplifying them away:

- Skip link (`.skip`) as the first focusable element in `<body>`.
- Every `<section>` has an `aria-labelledby` pointing to its own heading `id`.
- Visible focus indicator via `:focus-visible` (do not suppress focus outlines).
- Color tokens must maintain AA contrast in **both** the light and dark palettes defined in `styles.css` — `--accent-ink` is deliberately darker/lighter than `--accent` specifically for body-text-sized link contrast (see comment in `styles.css`).
- Respect `prefers-reduced-motion`.
- Heading levels are sequential (h1 → h2 → h3) with no skipped levels.

There are no forms; the calls-to-action are `mailto:` links with pre-filled subject/body (URL-encoded) to `dean.bowles@gmail.com`.
