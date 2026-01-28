---

id: css-backgrounds-borders-masks
title: Backgrounds, Borders & Masks
sidebar_position: 18
--------------------

# Backgrounds, Borders & Masks

> Backgrounds, borders, masks, and clipping participate in the **painting stage**, controlling how boxes are visually filled, stroked, clipped, and composited.

---

## Multiple Backgrounds

> CSS allows stacking multiple background layers, painted back-to-front.

```css
/* Value Blueprint
background:
  <bg-layer># , <final-bg-color?>

<bg-layer> =
  <image> || <position> [ / <size> ]? || <repeat> || <attachment> || <origin> || <clip>

<image>        = url() | gradient() | image-set() | none
<position>     = <length-percentage>{1,2} | left | center | right | top | bottom
<size>         = auto | cover | contain | <length-percentage>{1,2}
<repeat>       = repeat | no-repeat | round | space
*/
```

<details>
<summary>Examples</summary>

```css
.box {
  background:
    url(top.png) center / contain no-repeat,
    linear-gradient(red, blue),
    #000;
}

/* Rules:

- First layer is painted on top
- Each layer has its own:
  - background-size
  - background-position
  - background-repeat
- background-color is always bottom-most

Visual order:
[top image]
[gradient]
[color]
*/
```

</details>

---

## Border Rendering

> Borders are painted outside the content box and follow box edges, respecting writing modes.

```css
/* Value Blueprint
border:
  <border-width> || <border-style> || <color>

border-style = none | hidden | solid | dotted | dashed | double | groove | ridge | inset | outset
border-width = thin | medium | thick | <length>
color        = <color> | currentColor
*/
```

<details>
<summary>Examples</summary>

```css
.box {
  border: 4px solid red;   /* <length> <style> <color> */
  border-inline-start: 2px dashed blue;
}

/* Notes:

- Borders participate in box size (unless border-box)
- Logical borders map via writing-mode
- Border styles affect join & dash rendering
*/
```

</details>

---

## `border-radius` Math

> Border radius is resolved per-corner and clamped to prevent overlap.

```css
/* Value Blueprint
border-radius:
  <length-percentage>{1,4} [ / <length-percentage>{1,4} ]?

- Percentages resolve against box size
- Slash separates horizontal / vertical radii
*/
```

<details>
<summary>Examples</summary>

```css
.card {
  border-radius: 40px 10px; /* horizontal / vertical */
}

/* Resolution:

- Radii are percentages of box size
- If sum of radii > box size → scaled down proportionally

Visual:
┌──────────────┐
│  ◜───────◝  │
│  │           │
│  ◟───────◞  │
└──────────────┘
*/
```

</details>

---

## Masking

> Masks control pixel-level visibility using images or gradients.

```css
/* Value Blueprint
mask-image     = <image>#
mask-mode      = match-source | luminance | alpha
mask-repeat    = <repeat-style>#
mask-position  = <position>#
mask-size      = <bg-size>#
mask-composite = add | subtract | intersect | exclude
*/
```

<details>
<summary>Examples</summary>

```css
.masked {
  mask-image: radial-gradient(circle, #000 60%, transparent 100%);
  mask-repeat: no-repeat;
  mask-position: center;
}

/* Notes:

- Mask alpha controls visibility
- Multiple masks behave like backgrounds
- mask-composite controls layer blending
*/
```

</details>

---

## Clip-Path

> `clip-path` defines a geometric clipping region.

```css
/* Value Blueprint
clip-path =
  inset() | circle() | ellipse() | polygon() | path() | url()

Units:
- <length> | <percentage>
- Reference box: border-box (default)
*/
```

<details>
<summary>Examples</summary>

```css
.shape {
  clip-path: polygon(0 0, 100% 0, 50% 100%);
}

/* Shapes:

- inset()
- circle()
- ellipse()
- polygon()
- url(#svgPath)

Notes:
- Clipping affects hit-testing
- clip-path does not affect layout
*/
```

</details>

---

## Backdrop Filters

> Backdrop filters apply effects to content behind an element.

```css
/* Value Blueprint
backdrop-filter:
  none | <filter-function>+

filter-function =
  blur() | brightness() | contrast() | drop-shadow() |
  grayscale() | hue-rotate() | invert() | opacity() |
  saturate() | sepia()
*/
```

<details>
<summary>Examples</summary>

```css
.glass {
  background: rgb(255 255 255 / 0.2);
  backdrop-filter: blur(10px) saturate(120%);
}

/* Notes:

- Requires stacking context
- Expensive (offscreen rendering)
- Applies after painting backdrop
*/
```

</details>

---

## Notes

* Backgrounds and masks are painted per box fragment
* Borders participate in hit testing
* Masking and clipping do not affect layout
* Backdrop filters trigger compositing layers

## Caveats

* Masks and backdrop-filter are GPU-expensive
* border-radius affects overflow clipping
* clip-path may disable optimizations
