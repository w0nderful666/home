# 🏠 w0nderful / home

[**中文**](README.zh-CN.md)

> 全屏 Canvas 背景动画 · 故障字符网格特效 · 鼠标跟随圆环 · 零依赖开源个人主页项目

A showcase page inspired by the Xiaomi MiMo 100T campaign — real-time canvas glitch effect with animated character grids, smooth cursor interactions, and a minimal dark theme.

**中文关键词：** `博客背景动画` `个人主页` `全屏动画背景` `Canvas特效` `Canvas背景` `开源项目` `故障字符网格` `鼠标跟随圆环` `打字机效果` `零依赖` `前端动画`

> **⚠️ Disclaimer**
> This project is created **for learning and educational purposes only**. The visual effect is inspired by the Xiaomi MiMo 100T campaign page (`100t.xiaomimimo.com`). All code is independently written; no assets or code from the original site are used. Xiaomi MiMo and related trademarks belong to their respective owners. If you believe this infringes your rights, please open an issue and we will address it promptly.

---

## Demo

https://w0nderful666.github.io/home

---

## Quick Start

```bash
git clone https://github.com/w0nderful666/home.git
cd home
open index.html
```

Or just open `index.html` in any browser — it's a single file with zero dependencies.

---

## How It Works

### 1. Canvas Glitch Grid

A dense grid of characters is drawn onto an HTML5 `<canvas>` element using the Canvas 2D API.

```
Grid spacing:   10px horizontally, 20px vertically
Font:           16px monospace
Character set:  A-Z, 0-9, !@#$&*()-_+=/[]{};:<>., etc.
```

**Rendering loop (`requestAnimationFrame`):**

- Every **50ms**, ~5% of the cells are randomly selected to mutate.
- Each mutated cell gets a **new random character** and a **new target color**.
- The color transitions smoothly using **linear interpolation (lerp)**: `current = lerp(oldColor, targetColor, progress)`, where `progress += 0.05` each frame. The fade takes ~20 frames (~330ms).
- Only characters actively fading trigger a redraw, minimizing CPU usage.

**Why this creates the flickering effect:**  
Because mutations happen at staggered times for different cells, and each color fade is asynchronous, the grid appears to shimmer organically — like digital static or a glitch display.

### 2. Ring Light Effect

The ring spotlight is achieved entirely with **CSS radial gradients**, no JavaScript involved:

```css
/* Darkens center — creates the hole in the ring */
.vignette-center {
  background: radial-gradient(circle, rgba(0,0,0,0.7) 0%, transparent 60%);
}

/* Darkens edges — creates the outer boundary of the ring */
.vignette-outer {
  background: radial-gradient(circle, transparent 50%, #000 100%);
}
```

The visible ring is the gap between the 60% center gradient and the 50% outer gradient. By adjusting these percentages, you can change the ring's radius and width.

### 3. Cursor Ring

A `<div>` follows the mouse cursor:

| Property | Value |
|----------|-------|
| Position | `fixed` |
| Size | 200×200px (shrinks to 0 on interactive elements) |
| Color | `#f6f3ec` |
| Blend mode | `mix-blend-mode: difference` |
| Smoothing | Lerp factor 0.18 |

**How it detects buttons:**  
On each `mousemove`, `document.elementFromPoint(e.clientX, e.clientY)` checks what element is under the cursor. If it's a `<button>`, `<a>`, `<input>`, etc. (or a child of one), the ring adds the `.hidden` class, triggering a CSS transition that shrinks it to 0×0 and fades it out.

**Lerp smoothing formula:**
```
actualX += (targetX - actualX) * 0.18;
actualY += (targetY - actualY) * 0.18;
```

The 0.18 factor creates a subtle lag that feels natural and polished.

### 4. Typewriter Text

A simple recursive `setTimeout` cycle types characters one by one, then deletes them after a pause, cycling through a list of phrases.

---

## Customization Guide

### Change the title / name

Edit the HTML inside `.content`:

```html
<p class="name-display">Your Name</p>
<p class="sub-name">your tagline here</p>
```

### Change the typewriter phrases

```js
const phrases = ['your title', 'another phrase', 'something else'];
```

### Change the button link

```html
<a class="btn" href="https://your-site.com" target="_blank">
  <span class="btn-text">Your Button Text</span>
</a>
```

### Change the header link

```html
<a class="header-link" href="https://your-site.com">Your Link</a>
```

### Change the glitch character set

```js
const SYMBOLS = 'YOUR_CUSTOM_CHARS_HERE';
```

### Change the ring spotlight size

In CSS, adjust the gradient percentages:

```css
.vignette-center {
  background: radial-gradient(circle, rgba(0,0,0,0.7) 0%, transparent 60%);
  /*    ↑ increase = smaller dark center, wider ring  */
  /*    ↓ decrease = larger dark center, narrower ring  */
}
.vignette-outer {
  background: radial-gradient(circle, transparent 50%, #000 100%);
  /*    ↑ increase = ring moves outward               */
  /*    ↓ decrease = ring moves inward                 */
}
```

### Change grid density / animation speed

```js
const CELL_W = 10;     // horizontal spacing (px) — smaller = more columns
const CELL_H = 20;     // vertical spacing (px)   — smaller = more rows
const UPDATE_MS = 50;  // mutation interval (ms)  — smaller = faster flicker
```

### Change colors

```js
const COLORS = ['#color1', '#color2', '#color3'];
```

---

## Embed into Your Site

**Option A: Iframe (easiest)**
```html
<iframe src="https://w0nderful666.github.io/home"
  style="width:100%;height:100vh;border:0;border-radius:12px;"></iframe>
```

**Option B: Extract components**  
Copy the `<canvas>` + `<script>` block and the `.vignette-*` / `.overlay` divs into your own hero section.

**Option C: Single file deploy**  
Download `index.html`, customize, and upload to any static host (GitHub Pages, Vercel, Netlify, etc.).

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Rendering | Canvas 2D API |
| Effects | CSS radial gradients, mix-blend-mode |
| Animation | requestAnimationFrame, CSS transitions |
| Mouse | elementFromPoint, lerp interpolation |
| Dependencies | **Zero** |

---

## Keywords / 中文搜索关键词

`博客背景动画` `个人主页` `全屏动画背景` `Canvas特效` `Canvas背景` `开源项目` `前端动画` `故障字符网格` `鼠标跟随圆环` `打字机效果` `零依赖` `单文件部署` `个人网站` `背景特效` `暗黑风格`

---

## License

MIT © 2026 [w0nderful](https://github.com/w0nderful666)
