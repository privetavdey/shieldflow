# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the project

No build step — open `index.html` directly in a browser (double-click or `open index.html`). All dependencies are loaded from CDN at runtime.

## Architecture

Everything lives in a single file: [index.html](index.html). It contains all CSS, HTML markup, and JavaScript.

### Three UI variants

The page renders three independent "cards" for the same Shield/Unshield flow, each exploring a different interaction pattern. A pill switcher at the bottom toggles which card is visible (`.card.active`):

- **V1 — Balance / Slider** (`#card-v1`): Horizontal drag slider controls the Public/Shielded split. Cards show balances and accept direct text input. Clicking the USD value toggles ALEO ↔ USD display mode.
- **V2 — Amount Input** (`#card-v2`): Classic amount input field with 25%/50%/75%/Max preset buttons and a Shield/Unshield tab toggle.
- **V3 — Vertical Balance Sliders** (`#card-v3`): Two side-by-side vertical cards, each draggable from the bottom, linked so their total always equals 100%.

### Key dependencies (CDN)

- **Rive** (`@rive-app/canvas` from unpkg): Renders `bg.riv` as an animated gradient fill inside the slider tracks. Falls back to a CSS gradient (`has-fallback` class) if Rive fails to load.
- **`<number-flow>`** custom element: Provides animated number transitions in V1 and V3. Code waits for `customElements.whenDefined('number-flow')` before enabling it; falls back to plain `<span>` drag display while loading.

### Mock data constants (top of `<script>`)

```js
const TOTAL_ALEO = 100000;
const ALEO_PRICE = 0.08;
```

All balance calculations derive from these two values.

### Rive file

`bg.riv` is the animated gradient used in slider fills. `loader-with-vm.riv` is present but unused in the current markup. Each variant (V1, V3) initialises its own `rive.Rive` instance pointing at `bg.riv`.

### Fonts

`'ABC Diatype Mono'` and `'Innovator Grotesk'` are referenced in CSS but must be loaded from an external source (not included in this repo). The UI will fall back to system fonts if unavailable.

### Assets

SVG icons in `assets/` — token icons, dividers, close/arrow/refresh buttons. The divider between V1 balance cards uses four state SVGs (`divider-inactive`, `divider-left`, `divider-right`, `divider-active`) swapped via CSS `:has()` selectors.
