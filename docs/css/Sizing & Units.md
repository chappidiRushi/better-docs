---

id: css-sizing-units
title: Sizing & Units
sidebar_position: 14
--------------------

# Sizing & Units

> CSS sizing is resolved through a multi-phase value resolution pipeline combining absolute units, relative units, intrinsic sizing keywords, aspect-ratio constraints, and context-dependent percentage resolution.

---

## Absolute & Relative Units

> Absolute units map to physical or reference pixels; relative units depend on font metrics, viewport, or containing context.

<details>
<summary>Examples</summary>

```css
.box {
  width: 300px;      /* px | cm | mm | in | pt | pc */
  font-size: 1rem;   /* root font-size */
  padding: 2em;      /* element font-size */
  border-radius: 5q; /* quarter-millimeters */
}

/* Resolution notes:

Absolute:
- px is reference pixel (device + zoom adjusted)
- Physical units map to CSS px

Relative:
- em → element font-size
- rem → root font-size
- ex / ch → font metrics
*/
```

</details>

---

## Viewport Units (Dynamic)

> Viewport units resolve against different viewport definitions depending on browser UI state and writing mode.

<details>
<summary>Examples</summary>

```css
.hero {
  height: 100vh;     /* legacy viewport */
  min-height: 100svh; /* small viewport */
  max-height: 100lvh; /* large viewport */
  padding: 5dvh;     /* dynamic viewport */
}

/* Visual:

Mobile viewport states:

┌───────────────┐  lvh (URL hidden)
│               │
│               │
└───────────────┘

┌───────────┐      svh (URL shown)
│           │
└───────────┘

Notes:
- vh is unstable on mobile
- dv* updates on scroll
*/
```

</details>

---

## Intrinsic Sizing Keywords

> Intrinsic keywords size boxes based on content contributions and available space.

<details>
<summary>Examples</summary>

```css
.item {
  width: min-content; /* min-content | max-content | fit-content() */
  max-width: fit-content(400px); /* <length> | <percentage> */
}

/* Behavior:

min-content:
┌─word─┐

max-content:
┌────long unbroken text────┐

fit-content:
min(max-content, max(min-content, available))
*/
```

</details>

---

## Aspect Ratio

> `aspect-ratio` defines a preferred width/height ratio used during auto sizing and replaced element layout.

<details>
<summary>Examples</summary>

```css
.media {
  aspect-ratio: 16 / 9; /* <number> / <number> | auto */
  width: 100%;
  height: auto;
}

/* Resolution:

Known width → compute height
Known height → compute width
Both auto → intrinsic size wins

Visual:

Width = 320px
Height = 180px
*/
```

</details>

---

## Percentage Sizing Traps

> Percentage sizes resolve against different containing boxes depending on property, layout mode, and formatting context.

<details>
<summary>Examples</summary>

```css
.parent {
  height: auto; /* percentage trap */
}

.child {
  height: 50%;  /* resolves to auto → ignored */
}
```

```css
.flex {
  display: flex;
  height: 400px;
}

.item {
  height: 50%; /* works: flex container has definite size */
}

/* Rules:

- Percent width → containing block width
- Percent height → only if CB height is definite
- Grid & flex establish definite sizes early
- Tables resolve percentages late
*/
```

</details>

---

## Modern Sizing Functions

> CSS provides math-based functions that clamp or interpolate values during computed-value resolution.

<details>
<summary>Examples</summary>

```css
.box {
  width: clamp(320px, 50vw, 960px); /* min, preferred, max */
  font-size: min(2rem, 5vw);        /* min() | max() */
}

/* Resolution:

clamp(a, b, c):
- resolves to b
- but never < a or > c

Used heavily for fluid typography & layouts
*/
```

</details>

---

## Container & Query-Relative Units

> Container query units resolve against the nearest query container, not the viewport.

<details>
<summary>Examples</summary>

```css
.card {
  container-type: inline-size;
}

.card__title {
  font-size: 5cqi; /* cqw | cqh | cqi | cqb | cqmin | cqmax */
}

/* Notes:

- cqi → inline size
- cqb → block size
- cq* units participate after container resolution
- Avoids viewport coupling
*/
```

</details>

---

## Auto, Stretch & Fill Semantics

> Some sizing keywords are layout-mode dependent and not absolute values.

<details>
<summary>Examples</summary>

```css
.item {
  width: auto;     /* content or stretch depending on context */
  height: stretch; /* grid-only, experimental */
}

/* Behavior:

- auto ≠ intrinsic
- auto may mean:
  - shrink-to-fit
  - stretch
  - max-content

Depends on formatting context
*/
```

</details>

---

## Replaced Elements & Intrinsic Ratios

> Replaced elements have intrinsic dimensions and ratios that affect sizing.

<details>
<summary>Examples</summary>

```css
img {
  width: 100%;
  height: auto; /* preserves intrinsic ratio */
}

/* Rules:

- Images/videos have intrinsic width/height
- aspect-ratio can override intrinsic ratio
- Percentage sizing respects intrinsic constraints
*/
```

</details>

---

## Notes

* Unit resolution happens before layout
* Percentages may defer or fail resolution
* Intrinsic keywords bypass fixed sizing
* Aspect-ratio participates in min/max sizing

## Caveats

* Mixing intrinsic and percentage sizing can cause cycles
* Viewport units differ across browsers
* Font-relative units depend on font fallback

