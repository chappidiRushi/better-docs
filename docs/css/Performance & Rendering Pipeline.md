---

id: css-performance-pipeline
title: Performance & Rendering Pipeline
sidebar_position: 26
--------------------

# Performance & Rendering Pipeline

> CSS performance is governed by **how often styles are recalculated, layout is invalidated, pixels are painted, and layers are composited**. Understanding the pipeline is mandatory for avoiding jank.

Progression: **style calculation → layout → paint → composite → containment optimizations**.

---

## Rendering Pipeline Overview

> Browsers process CSS in ordered phases. Not all changes trigger all phases.

Pipeline stages:

1. **Style calculation** – resolve cascaded & computed values
2. **Layout (reflow)** – calculate box geometry
3. **Paint** – rasterize pixels into layers
4. **Composite** – GPU combines layers

```text
Style → Layout → Paint → Composite
```

Invalidation can stop early if changes are isolated.

---

## Style Calculation

> Resolves cascade, inheritance, variables, media/container queries.

Triggers:

* Class/attribute changes
* Variable updates
* Media query changes

Cost characteristics:

* Tree-wide when variables are used high in DOM
* Scoped when selectors are narrow

<details>
<summary>Examples</summary>

```css
/* Expensive: variable defined on :root */
:root {
  --theme-color: red;
}

/* Cheaper: scoped variable */
.card {
  --theme-color: red;
}
```

</details>

---

## Layout Thrashing

> Forced synchronous layout caused by **read-after-write** patterns.

Common layout reads:

* `offsetWidth / offsetHeight`
* `getBoundingClientRect()`
* `scrollTop`

<details>
<summary>Examples</summary>

```js
// BAD: thrashing
el.style.width = '200px';
console.log(el.offsetWidth); // forces layout

// GOOD: batch reads/writes
el.style.width = '200px';
requestAnimationFrame(() => {
  console.log(el.offsetWidth);
});
```

</details>

---

## Paint vs Composite

> Not all visual changes repaint pixels.

Categories:

* **Layout-affecting** → layout + paint + composite
* **Paint-only** → paint + composite
* **Composite-only** → composite only (GPU)

Composite-only properties:

```css
transform
opacity
filter /* partial */
```

<details>
<summary>Examples</summary>

```css
/* Cheap animation */
.box {
  transform: translateX(100px);
}

/* Expensive animation */
.box {
  width: 200px; /* layout + paint */
}
```

</details>

---

## Stacking & Layer Promotion

> Elements may be promoted to composited layers.

Triggers:

* `transform`
* `opacity < 1`
* `position: fixed`
* `will-change`

Layer cost:

* GPU memory
* Upload overhead

---

## CSS Containment

> Explicitly limits how changes propagate.

Blueprint:

```css
contain:
  none |
  size |
  layout |
  paint |
  style |
  strict | /* size + layout + paint + style */
  content; /* layout + paint + style */
```

Effects:

* Stops layout & paint invalidation
* Creates new formatting context

<details>
<summary>Examples</summary>

```css
.card {
  contain: layout paint;
}
```

</details>

---

## `content-visibility`

> Skips rendering work for off-screen content.

Blueprint:

```css
content-visibility: visible | hidden | auto;
contain-intrinsic-size: <length> | auto;
```

Rules:

* `auto` skips layout & paint when offscreen
* Requires intrinsic size to prevent jumps

<details>
<summary>Examples</summary>

```css
.section {
  content-visibility: auto;
  contain-intrinsic-size: 800px;
}
```

</details>

---

## GPU Myths

> GPU acceleration is not free.

Reality:

* Too many layers hurt performance
* Uploading textures is expensive
* GPU does not fix layout bottlenecks

Bad practices:

* Blanket `will-change`
* Animating layout properties

---

## Debugging Performance

Tools:

* Chrome DevTools → Performance
* Layers panel
* Paint flashing
* Rendering stats

---

## Notes

* Most performance wins come from **avoiding layout**
* Composite-only animations are safest
* Containment is the strongest CSS performance tool
* Measure before optimizing

## Caveats

* Overusing containment breaks intrinsic sizing
* `content-visibility` can harm accessibility if misused
* GPU memory is finite
* Performance characteristics vary per browser
