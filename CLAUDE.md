# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Stack & Conventions

This project has two hard constraints that must never be violated:

1. **Single-file project.** The entire project must live in one `index.html` file, with all CSS and JavaScript inlined using `<style>` and `<script>` tags. Do not split markup, styles, or scripts into separate files, and do not add additional HTML pages. Linking to external images and to external CSS/JavaScript libraries (e.g. via CDN `<link>`/`<script src>` tags) is allowed. This constraint exists so the finished project can be copy-pasted as a single file for sharing in class and on single-file code platforms (e.g. CodePen, JSFiddle).
2. **Vanilla only, no build step.** Use plain HTML, CSS, and JavaScript only. No frameworks or libraries that require a build/compile step (e.g. React, Vue, Angular, JSX, TypeScript, Sass/Less, bundlers like Webpack/Vite). The file must run directly by opening it in a browser, with no installation or build process required.

## Tech stack (hard constraints — do not deviate)
- Vanilla HTML, CSS, and JavaScript only. No React, Vue, or any JS framework.
- Tailwind CSS for all styling (via CDN only).
- No backend, no database. Fully static site.
- A toggle for light and dark theme, with the choice remembered
  across visits.

## Working conventions
- Before implementing any non-trivial feature, ask clarifying
  questions about scope, edge cases, and constraints first —
  don't propose a plan until you've asked.

## Feature Plan

### Phase 1 — Portal shell + Atkinson Cycle Visualiser
- [ ] Portal shell: header/main/footer scaffold; theme toggle (Tailwind `class` dark-mode strategy, persisted to `localStorage`, applied pre-paint to avoid flash)
- [ ] Tools registry + hash router (`TOOLS_REGISTRY` array, `#toolId` navigation via `hashchange`, mount/unmount lifecycle per tool) — the extensible pattern every future tool plugs into
- [ ] Homepage view: card grid rendered from the registry (one card per tool)
- [ ] Atkinson Cycle Visualiser tool:
  - Two coupled sliders — compression ratio and expansion ratio, with expansion always ≥ compression
  - Air-standard Atkinson cycle math (isentropic compression → constant-volume heat addition → isentropic expansion → blowdown + isobaric exhaust)
  - Two `<canvas>` views (P-V diagram + piston/crank schematic) driven by one `requestAnimationFrame` loop and a shared cursor, so they stay in sync
  - Auto-play on entering the tool, with a play/pause control
  - Canvas colors follow the current theme
  - Short explanatory write-up: the 4 strokes, why higher expansion than compression improves efficiency, contrast with the Otto cycle

### Data model
- `TOOLS_REGISTRY`: `{ id, title, description, icon, viewId, mount, unmount }[]` — one entry per tool; grows over time, nothing else changes when a tool is added
- Cycle state: corner states `{ P, V, T }` at points 1 → 2 → 3 → 4 → 4a; a dense animation path is `{ P, V, stroke }[]`
- Persisted state: `localStorage.theme` (`'light' | 'dark'`) — the only persisted app state in phase 1

### Key flows
- **Navigation:** click a card → hash changes → router hides/shows the matching `[data-view]` section, unmounts the tool being left, mounts the tool being entered (mount is idempotent, guarded so re-entering doesn't double-attach)
- **Theme:** pre-paint inline script sets the `dark` class from `localStorage` (falling back to OS preference) → toggle button flips the class and rewrites `localStorage` → canvas drawing re-reads the current theme every animation frame
- **Visualiser interaction:** slider input → recompute cycle states/path → reset animation cursor → RAF loop advances the cursor and redraws both canvases from it each frame

### Later phases
- Phase 2+: each new tool is one registry entry + one `<section>` + one mount/unmount pair — no router, grid, or theme changes expected. Add phases here as they're scoped.

*(Check items off, or prune a phase's detail down to a one-line summary, once it ships — keep this section current rather than letting it grow indefinitely.)*
