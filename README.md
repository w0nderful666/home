# w0nderful / home

A showcase page inspired by Xiaomi MiMo 100T — featuring a real-time canvas glitch effect with animated character grids, smooth cursor interactions, and a minimal dark theme.

> **⚠️ Disclaimer**
> This project is created **for learning and educational purposes only**. The visual effect is inspired by the Xiaomi MiMo 100T campaign page (`100t.xiaomimimo.com`). All code is independently written, no assets or code from the original site are used. Xiaomi MiMo and related trademarks belong to their respective owners. If you are a rights holder and believe this infringes your rights, please open an issue and we will address it promptly.

## Features

- **Canvas 2D Glitch Grid** — Dense character grid with random mutation + smooth color transitions
- **Ring Light Effect** — CSS radial gradient overlays create a circular spotlight on the canvas
- **Cursor Ring** — Mouse-following ring with lerp smoothing, shrinks on interactive elements
- **Typewriter Text** — Cycling type-in/type-out animation
- **Full-screen Hero** + scroll-down to README section
- **Zero dependencies** — No frameworks, no libraries, no build step

## Demo

https://w0nderful666.github.io/home

## Quick Start

```bash
git clone https://github.com/w0nderful666/home.git
cd home
open index.html
```

Or just open `index.html` in any browser — it's a single file.

## How It Works

### Canvas Glitch Grid

A dense grid of characters (letters, numbers, symbols) is drawn on a 2D canvas. Every 50ms, ~5% of them randomly change to a new character + new color, with smooth color interpolation over ~20 frames. This creates the flickering "digital rain" effect.

```
Grid: 10px × 20px cells, 16px monospace font
Mutation: 50ms interval, 5% of cells per tick
Colors: #53514D, #998f84 with linear interpolation
```

### Ring Light Effect

Two CSS radial gradient overlays create the circular spotlight:
- `.vignette-center` — dark center → transparent at 60% radius
- `.vignette-outer` — transparent at 50% → dark edges

The visible ring is the gap between these two gradients.

### Cursor Ring

A `position: fixed` div follows the mouse with lerp smoothing (factor 0.18). Uses `elementFromPoint` to detect interactive elements; when hovering a button/link, the ring shrinks to 0 with CSS transition. `mix-blend-mode: difference` gives the inverted highlight effect.

## Embed into Your Site

Three options:

**Option A: Iframe embed (easiest)**
```html
<iframe src="https://w0nderful666.github.io/home"
  style="width:100%;height:100vh;border:0;border-radius:12px;"></iframe>
```

**Option B: Extract just the hero background**
Copy the `<canvas>` + `<script>` block and the `.vignette-*` / `.overlay` divs into your own hero section.

**Option C: Single file**
Download `index.html`, edit the content to your liking, and deploy anywhere.

## Customization

Key parameters you can tweak in `<script>`:

```js
// Character set
const SYMBOLS = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ!@#$&*...';

// Grid density & speed
const CELL_W = 10, CELL_H = 20;   // pixels per cell
const UPDATE_MS = 50;              // ms between mutations

// Color palette
const COLORS = ['#53514D', '#53514D', '#998f84'];
```

And the ring light effect in CSS:

```css
.vignette-outer { background: radial-gradient(circle, transparent 50%, #000 100%); }
.vignette-center { background: radial-gradient(circle, rgba(0,0,0,0.7) 0%, transparent 60%); }
```

## Tech Stack

- Vanilla JavaScript (Canvas 2D API)
- CSS3 (radial gradients, mix-blend-mode, transitions, keyframes)
- **Zero dependencies** — no frameworks, no libraries, no build step

## Browser Support

Chrome, Firefox, Safari, Edge — any modern browser with Canvas 2D support.

## License

MIT © 2026 [w0nderful](https://github.com/w0nderful666)
