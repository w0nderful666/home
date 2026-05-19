# 🏠 w0nderful / home

[**English**](README.md)

> 全屏 Canvas 背景动画 · 故障字符网格特效 · 鼠标跟随圆环 · 零依赖开源个人主页项目

**关键词：** `博客背景动画` `个人主页` `全屏动画背景` `Canvas特效` `Canvas背景` `开源项目` `故障字符网格` `鼠标跟随圆环` `打字机效果` `零依赖` `前端动画` `单文件部署`

受 Xiaomi MiMo 100T 活动页面启发的一个展示页 — 实时 Canvas 故障字符网格动画、平滑光标交互、极简暗色主题。

> **⚠️ 免责声明**
> 本项目**仅供学习交流使用**。视觉效果受 Xiaomi MiMo 100T 活动页面（`100t.xiaomimimo.com`）启发。所有代码均为独立编写，未使用原站的任何资产或代码。Xiaomi MiMo 及相关商标归其各自所有者所有。如认为侵权，请提交 Issue，我们将及时处理。

---

## 在线演示

https://w0nderful666.github.io/home

---

## 快速开始

```bash
git clone https://github.com/w0nderful666/home.git
cd home
open index.html
```

或者直接用浏览器打开 `index.html` — 单个文件，零依赖。

---

## 实现原理

### 1. Canvas 故障字符网格

使用 Canvas 2D API 在 `<canvas>` 上绘制密集的字符网格。

```
网格间距：   水平 10px，垂直 20px
字体：       16px 等宽字体
字符集：    A-Z, 0-9, !@#$&*()-_+=/[]{};:<>., 等
```

**渲染循环（`requestAnimationFrame`）：**

- 每 **50ms**，随机选中 ~5% 的格子进行**突变**。
- 每个突变的格子获得一个**新随机字符**和一个**新目标颜色**。
- 颜色通过**线性插值（lerp）** 平滑过渡：`current = lerp(oldColor, targetColor, progress)`，每帧 `progress += 0.05`。渐变更持续约 20 帧（~330ms）。
- 仅正在渐变的字符触发重绘，最大限度减少 CPU 占用。

**为什么会有闪烁效果：**  
不同格子的突变时间错开，每个颜色渐变异步进行，整体呈现有机的 shimmer 效果 — 类似数字静态噪点或故障显示。

### 2. 环形光晕效果

环形聚光灯完全通过 **CSS 径向渐变** 实现，无需 JavaScript：

```css
/* 中心暗化 — 形成圆环的"洞" */
.vignette-center {
  background: radial-gradient(circle, rgba(0,0,0,0.7) 0%, transparent 60%);
}

/* 边缘暗化 — 形成圆环的外边界 */
.vignette-outer {
  background: radial-gradient(circle, transparent 50%, #000 100%);
}
```

可见的环形区域就是 60% 的中心渐隐与 50% 的外围渐隐之间的空隙。调整这两个百分比可以改变圆环的半径和宽度。

### 3. 鼠标跟随圆环

一个 `<div>` 跟随鼠标移动：

| 属性 | 值 |
|------|------|
| 定位 | `fixed` |
| 尺寸 | 200×200px（在交互元素上缩小至 0） |
| 颜色 | `#f6f3ec` |
| 混合模式 | `mix-blend-mode: difference` |
| 平滑 | Lerp 系数 0.18 |

**如何检测按钮：**  
每次 `mousemove` 时，`document.elementFromPoint(e.clientX, e.clientY)` 检测鼠标下方是什么元素。如果是 `<button>`、`<a>`、`<input>` 等（或其子元素），圆环添加 `.hidden` 类，触发 CSS 过渡缩小至 0×0 并淡出。

**Lerp 平滑公式：**
```
actualX += (targetX - actualX) * 0.18;
actualY += (targetY - actualY) * 0.18;
```

0.18 的系数产生微妙的延迟感，使交互自然流畅。

### 4. 打字机效果

通过递归 `setTimeout` 逐个字符输入，暂停后逐个删除，循环切换多组短语。

---

## 自定义指南

### 修改标题/名称

编辑 `.content` 内的 HTML：

```html
<p class="name-display">你的名字</p>
<p class="sub-name">你的标语</p>
```

### 修改打字机短语

```js
const phrases = ['你的标题', '另一个短语', '其他内容'];
```

### 修改按钮链接

```html
<a class="btn" href="https://你的网站.com" target="_blank">
  <span class="btn-text">按钮文字</span>
</a>
```

### 修改顶部导航栏链接

```html
<a class="header-link" href="https://你的网站.com">链接文字</a>
```

### 修改故障字符集

```js
const SYMBOLS = '你自定义的字符';
```

### 修改环形光晕尺寸

在 CSS 中调整渐变百分比：

```css
.vignette-center {
  background: radial-gradient(circle, rgba(0,0,0,0.7) 0%, transparent 60%);
  /*    ↑ 增大 = 暗心变小，光环变宽  */
  /*    ↓ 减小 = 暗心变大，光环变窄  */
}
.vignette-outer {
  background: radial-gradient(circle, transparent 50%, #000 100%);
  /*    ↑ 增大 = 光环向外移          */
  /*    ↓ 减小 = 光环向内移          */
}
```

### 修改网格密度/动画速度

```js
const CELL_W = 10;     // 水平间距（像素）— 越小列数越多
const CELL_H = 20;     // 垂直间距（像素）— 越小行数越多
const UPDATE_MS = 50;  // 突变间隔（毫秒）— 越小闪烁越快
```

### 修改颜色

```js
const COLORS = ['#颜色1', '#颜色2', '#颜色3'];
```

---

## 嵌入你的网站

**方式 A：iframe 嵌入（最简单）**
```html
<iframe src="https://w0nderful666.github.io/home"
  style="width:100%;height:100vh;border:0;border-radius:12px;"></iframe>
```

**方式 B：提取组件**  
将 `<canvas>` + `<script>` 块以及 `.vignette-*` / `.overlay` 的 div 复制到你自己的 hero 区域。

**方式 C：单文件部署**  
下载 `index.html`，自定义后上传到任意静态托管平台（GitHub Pages、Vercel、Netlify 等）。

---

## 技术栈

| 层级 | 技术 |
|------|------|
| 渲染 | Canvas 2D API |
| 特效 | CSS 径向渐变、mix-blend-mode |
| 动画 | requestAnimationFrame、CSS transitions |
| 鼠标交互 | elementFromPoint、lerp 插值 |
| 依赖 | **零依赖** |

---

## 搜索关键词 / SEO Tags

`博客背景动画` `个人主页` `全屏动画背景` `Canvas特效` `Canvas背景` `开源项目` `前端动画` `故障字符网格` `鼠标跟随圆环` `打字机效果` `零依赖` `单文件部署` `个人网站` `背景特效` `暗黑风格`

---

## 许可证

MIT © 2026 [w0nderful](https://github.com/w0nderful666)
