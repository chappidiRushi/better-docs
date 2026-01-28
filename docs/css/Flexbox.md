---

id: css-flexbox-spec
title: Flexbox (Specification-Level)
sidebar_position: 11
--------------------

# Flexbox (Specification-Level)

> Flexbox defines a one-dimensional formatting context with a complex sizing algorithm designed to distribute free space, resolve intrinsic sizes, and align items along a main and cross axis. It replaces block/inline flow for its children and has many non-obvious edge cases.

---

## Flex Formatting Context

> A layout context established by `display: flex | inline-flex` that suppresses block and inline layout for direct children and applies the flex layout algorithm.

<details>
<summary>Examples</summary>

```css
.container {
  display: flex;              /* flex | inline-flex */
}

/* Effects:
   - Children become flex items
   - Floats are ignored
   - Margin collapsing is disabled
   - Flex items establish independent formatting contexts
*/
```

</details>

---

## Main Axis vs Cross Axis

> Flex layout resolves sizes and alignment along two orthogonal axes derived from writing mode and `flex-direction`.

<details>
<summary>Examples</summary>

```css
.row {
  flex-direction: row;        /* row | row-reverse | column | column-reverse */
}

.col {
  flex-direction: column;
}

/* Axis mapping:
   row (LTR):
   ┌──────────────────────────┐
   │ item → item → item       │  ← main axis (inline)
   │                          │
   │                          │
   └──────────────────────────┘
              ↑
           cross axis (block)

   column:
   ┌───────┐
   │ item  │
   │  ↓    │  ← main axis (block)
   │ item  │
   │  ↓    │
   │ item  │
   └───────┘
          → cross axis (inline)
*/
```

</details>

---

## Flex Sizing Algorithm

> Multi-step algorithm that resolves flex base sizes, hypothetical sizes, free space distribution, and final used sizes.

<details>
<summary>Examples</summary>

```css
.item {
  flex: 1 1 auto;             /* grow | shrink | basis */
}

/* Visual flow:

   Container: 600px
   ┌────────────────────────────────────┐
   │ [200] [200] [200]                  │ ← flex base sizes
   └────────────────────────────────────┘

   Free space: 0px → no grow

   If container = 800px:
   Free space = 200px

   Grow distribution (1:1:1):
   +66.6px each

   Final sizes:
   ┌────────────────────────────────────┐
   │ [266] [266] [266]                  │
   └────────────────────────────────────┘

   Algorithm stages:
   1. Determine flex base size
   2. Apply min/max clamp
   3. Resolve free space
   4. Distribute grow/shrink
   5. Freeze items when limits hit
*/
```

</details>

---

## `flex-basis` vs `width`

> `flex-basis` controls the initial main-size contribution of a flex item, often overriding `width` or `height` depending on axis.

<details>
<summary>Examples</summary>

```css
.item-a {
  flex-basis: auto;           /* auto | content | <length> | <percentage> */
  width: 200px;               /* used when flex-basis:auto */
}

.item-b {
  flex-basis: 0;              /* ignores width, purely flexible */
}

/* Visual comparison:

   flex-basis:auto (width honored):
   ┌──────────────┐
   │   200px      │
   └──────────────┘

   flex-basis:0 (width ignored):
   ┌───┐ ┌───┐ ┌───┐
   │   │ │   │ │   │  ← all equal via flex-grow
   └───┘ └───┘ └───┘

   Rules:
   - basis always maps to main-size
   - width/height only consulted if basis=auto
*/
```

</details>

---

## Min / Max Constraints

> `min-width`, `max-width`, `min-height`, and `max-height` participate directly in the flex sizing algorithm and can override flex factors.

<details>
<summary>Examples</summary>

```css
.item {
  flex: 1 1 200px;
  min-width: 150px;           /* auto | <length> | <percentage> */
  max-width: 300px;
}

/* Important:
   - min-size defaults to auto (content-based)
   - min-width:auto prevents shrinking
   - Common source of overflow bugs
*/
```

</details>

---

## Overflow & Flex Edge Cases

> Overflow behavior in flex containers interacts with min-size, intrinsic sizing, and scroll container creation.

<details>
<summary>Examples</summary>

```css
.container {
  display: flex;
  overflow: auto;             /* visible | hidden | auto | scroll */
}

.item {
  min-width: 0;               /* allows flex item to shrink */
}

/* Edge cases:
   - overflow creates scroll container
   - min-width:auto causes unexpected overflow
   - flex items can overflow container
*/
```

</details>

---

## Alignment & Distribution

> Alignment properties control how flex items and lines are positioned once sizing is resolved.

<details>
<summary>Examples</summary>

```css
.container {
  justify-content: space-between; /* flex-start | flex-end | center | space-* */
  align-items: center;            /* stretch | flex-start | flex-end | center | baseline */
  align-content: space-around;    /* affects multi-line containers */
  flex-wrap: wrap;                /* nowrap | wrap | wrap-reverse */
}

/* Visuals:

justify-content: space-between
┌──────────────────────────┐
│▮      ▮      ▮         │
└──────────────────────────┘

align-items: center
┌──────────────────────────┐
│        ▮ ▮ ▮           │ ← centered on cross axis
└──────────────────────────┘

align-content (multi-line):
┌──────────────────────────┐
│▮ ▮ ▮                   │
│                          │
│▮ ▮ ▮                   │
└──────────────────────────┘
*/
```

</details>

---

## Flex Item Ordering

> Visual order can differ from source order using the `order` property.

<details>
<summary>Examples</summary>

```css
.item {
  order: 1;                   /* integer, default 0 */
}

.first {
  order: -1;
}

/* Notes:
   - Affects visual order only
   - Does not affect DOM or tab order
*/
```

</details>

---

## Notes

* Flexbox is main-axis-first
* Intrinsic sizing heavily influences results
* Min-size defaults are critical
* Many bugs are algorithmic, not syntax-related

## Caveats

* Flexbox is not two-dimensional
* `min-width:auto` is the #1 flex bug
* Percentage sizing depends on definite container sizes
