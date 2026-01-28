---

id: css-scroll-overflow
title: Scroll, Overflow & Viewport Behavior
sidebar_position: 21
--------------------

# Scroll, Overflow & Viewport Behavior

> This section covers how CSS creates **scroll containers**, routes scroll input, and constrains sticky/viewport-relative behavior — from basic overflow to spec-level scroll chaining and snapping.

Progression: **mental model → platform behavior → engine internals**.

---

## Overflow & Scroll Containers (Foundations)

> A scroll container is created when overflow clips and scrolling is enabled.

```css
/* Value Blueprint
overflow = visible | hidden | clip | scroll | auto
overflow-x = visible | hidden | clip | scroll | auto
overflow-y = visible | hidden | clip | scroll | auto
*/
```

Rules:

* `visible` → no scroll container
* `hidden` → clips, scrollable programmatically
* `clip` → clips, **no scrolling at all**
* `scroll` → always scrollable
* `auto` → scrollable only if overflow exists

<details>
<summary>Examples</summary>

```css
/* Beginner */
.box {
  overflow: auto;
}

/* Intermediate: axis-specific */
.panel {
  overflow-x: auto;
  overflow-y: hidden;
}

/* Advanced: clip vs hidden */
.mask {
  overflow: clip; /* no scrollbars, no scrolling */
}
```

</details>

---

## Scroll Containers & Viewport Types

> Not all viewports are equal.

Viewport concepts:

* **Layout viewport** → used for layout
* **Visual viewport** → visible area (zoom / OSK)
* **Scrollport** → padding box of scroll container

Special cases:

* Root scroller (`html` / `body`)
* `position: fixed` attaches to **layout viewport**

<details>
<summary>Examples</summary>

```css
/* Root scrolling */
html {
  overflow: auto;
}

/* Fixed ignores parent scroll */
.header {
  position: fixed;
  top: 0;
}
```

</details>

---

## Scroll Chaining & Propagation

> Scroll input propagates up the ancestor chain when a scroll boundary is hit.

Default behavior:

* Inner scroll reaches limit
* Remaining delta bubbles to ancestor

Problems:

* Scroll bleed
* Page bounce

<details>
<summary>Examples</summary>

```css
.modal {
  overflow: auto;
  max-height: 80vh;
}
```

</details>

---

## `overscroll-behavior`

> Controls scroll chaining and boundary behavior.

```css
/* Value Blueprint
overscroll-behavior = auto | contain | none
overscroll-behavior-x = auto | contain | none
overscroll-behavior-y = auto | contain | none
*/
```

Rules:

* `auto` → default chaining
* `contain` → stop chaining, allow glow/bounce
* `none` → stop chaining + disable bounce

<details>
<summary>Examples</summary>

```css
/* Prevent scroll bleed */
.modal {
  overscroll-behavior: contain;
}

/* Full isolation */
.drawer {
  overscroll-behavior: none;
}
```

</details>

---

## Scroll Snapping

> Scroll snapping constrains scroll offsets to **snap points**.

```css
/* Value Blueprint
scroll-snap-type = none | x | y | block | inline | both [ mandatory | proximity ]?

scroll-snap-align = none | start | end | center
scroll-snap-stop = normal | always
*/
```

Rules:

* Snapping occurs **after scroll ends**
* `mandatory` forces snap
* `proximity` snaps only if near

<details>
<summary>Examples</summary>

```css
.container {
  scroll-snap-type: x mandatory;
  overflow-x: auto;
}

.item {
  scroll-snap-align: center;
}
```

</details>

---

## Sticky Positioning Constraints

> `position: sticky` toggles between relative and fixed **within a scroll container**.

```css
/* Value Blueprint
position = static | relative | absolute | fixed | sticky
*/
```

Rules:

* Sticky is relative until threshold
* Clipped by nearest scroll container
* Disabled if any ancestor has `overflow: hidden/clip`

<details>
<summary>Examples</summary>

```css
.header {
  position: sticky;
  top: 0;
}

/* Sticky fails */
.wrapper {
  overflow: hidden; /* blocks sticky */
}
```

</details>

---

## Programmatic Scrolling & CSS

> CSS defines containers; JS drives scroll offsets.

Relevant APIs:

* `scrollTop` / `scrollLeft`
* `scrollIntoView()`
* `scroll-behavior: smooth`

```css
/* Value Blueprint
scroll-behavior = auto | smooth
*/
```

<details>
<summary>Examples</summary>

```css
html {
  scroll-behavior: smooth;
}
```

</details>

---

## Notes

* Overflow creates new scroll formatting contexts
* Sticky depends on scroll containers, not viewport
* Snap points do not affect layout
* Root scrolling differs by UA

## Caveats

* `overflow: hidden` breaks sticky
* Nested scroll hurts accessibility
* Scroll snapping can fight user intent
* Mobile browsers vary in viewport handling
