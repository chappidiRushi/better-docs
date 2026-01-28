---

id: css-transforms-animations
title: Transforms, Transitions & Animations
sidebar_position: 20
--------------------

# Transforms, Transitions & Animations

> This section covers **post-layout visual effects** in CSS — from simple transitions to compositor-level transforms and timeline-driven animations.

The order below progresses from **beginner mental model → spec-level internals**.

---

## Transforms (Basics → Internals)

> Transforms visually modify an element **without affecting document flow**.

```css
/* Value Blueprint
transform = none | <transform-function>+

transform-function =
  translate() | translateX() | translateY() | translateZ() |
  scale() | scaleX() | scaleY() | scaleZ() |
  rotate() | rotateX() | rotateY() | rotateZ() |
  skew() | skewX() | skewY() |
  matrix() | matrix3d()
*/
```

Key rules:

* Functions are applied **right → left**
* Percentages resolve against the **border box**
* Any transform creates a **new stacking context**
* 2D transforms are promoted to **4×4 matrices** internally

<details>
<summary>Examples (from simple → advanced)</summary>

```css
/* Beginner: simple movement */
.box {
  transform: translateX(100px);
}

/* Intermediate: combined transforms */
.card {
  transform: scale(1.1) rotate(5deg);
}

/* Advanced: transform order matters */
.panel {
  transform: translateX(50%) rotate(45deg) scale(1.2);
}

/* Actual execution order:
1. scale
2. rotate
3. translate
*/

/* Hardcore: explicit matrix */
.debug {
  transform: matrix(1, 0, 0, 1, 50, 0); /* translateX(50px) */
}
```

</details>

---

## Transitions (State-Driven Animation)

> Transitions animate **changes between two computed values**.

```css
/* Value Blueprint
transition =
  <property> <duration> <timing-function>? <delay>?

property = all | none | <custom-ident>#
*/
```

Rules:

* Only animates when a value **changes**
* Interpolates **computed values** (not layout results)
* Non-animatable properties are ignored
* Discrete properties jump at 50%

<details>
<summary>Examples</summary>

```css
/* Beginner */
.button {
  transition: background-color 200ms ease;
}

/* Intermediate: multiple properties */
.card {
  transition:
    transform 300ms cubic-bezier(.4,0,.2,1),
    opacity 150ms linear 50ms;
}

/* Advanced: interrupted transitions */
.menu {
  transition: transform 250ms ease;
}

.menu.open {
  transform: translateX(0);
}

.menu.closed {
  transform: translateX(-100%);
}
```

</details>

---

## Animations (Timeline-Driven)

> Animations run on an independent **timeline**, not tied to state changes.

```css
/* Value Blueprint
animation =
  <name> <duration> <timing-function>? <delay>?
  <iteration-count>? <direction>? <fill-mode>? <play-state>?
*/
```

Lifecycle:

1. before-start
2. active
3. after-end

Rules:

* Animations **override transitions**
* Negative delay jumps the timeline forward
* `animation-fill-mode` controls pre/post styles

<details>
<summary>Examples</summary>

```css
/* Beginner */
@keyframes fade {
  from { opacity: 0 }
  to   { opacity: 1 }
}

.box {
  animation: fade 500ms ease-out;
}

/* Intermediate: looping */
.loader {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

/* Advanced: negative delay */
.progress {
  animation: load 2s linear -1s forwards;
}
```

</details>

---

## Timing Functions & Interpolation

> Timing functions remap **progress**, not values.

```css
/* timing-function =
  linear | ease | ease-in | ease-out | ease-in-out |
  steps(<int>, start|end) | cubic-bezier(x1,y1,x2,y2)
*/
```

Interpolation details:

* Colors interpolate in the active **color space**
* Transforms decompose into translate/rotate/scale
* Mismatched transform functions fall back to matrices

<details>
<summary>Examples</summary>

```css
/* Stepped animation */
.sprite {
  animation: walk 1s steps(8, end) infinite;
}

/* Custom easing */
.modal {
  transition: transform 400ms cubic-bezier(.2,.8,.2,1);
}
```

</details>

---

## Composite-Only vs Layout Animations

> Not all animations cost the same.

Composite-only (fast, GPU-friendly):

* `transform`
* `opacity`

Paint-only (repaint required):

* `filter`
* `box-shadow`
* `background-position`

Layout-affecting (expensive):

* `width`, `height`
* `top`, `left`
* `margin`, `padding`

<details>
<summary>Examples</summary>

```css
/* Good */
.toast {
  transform: translateY(0);
  opacity: 1;
}

/* Bad */
.toast {
  top: 0; /* triggers layout */
}
```

</details>

---

## `will-change` (Advanced / Dangerous)

> A hint to pre-promote elements — misuse causes **memory and performance issues**.

```css
/* Value Blueprint
will-change = auto | <animateable-property>#
*/
```

Rules:

* Use only shortly before animation
* Remove after animation ends
* Never apply globally

<details>
<summary>Examples</summary>

```css
.modal {
  will-change: transform, opacity;
}

/* JS cleanup */
.modal.done {
  will-change: auto;
}
```

</details>

---

## Notes

* Transforms never affect layout flow
* Animations override transitions at equal priority
* Composite-only animations are cheapest
* Each transform creates a new stacking context

## Caveats

* `will-change` can degrade performance
* Filters often trigger repaint
* Excess animations hurt battery life
* Matrix interpolation can vary by engine
