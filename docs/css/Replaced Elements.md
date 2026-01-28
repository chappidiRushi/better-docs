---

id: css-replaced-elements
title: Replaced Elements
sidebar_position: 19
--------------------

# Replaced Elements

> Replaced elements are elements whose **content is rendered outside the CSS formatting model**, supplied by an external object (image, media, UA widget), while **CSS controls only the box geometry and positioning**.

---

## What Are Replaced Elements

> Elements whose internal rendering is opaque to CSS.

**Fully replaced**

* `img`
* `video`
* `audio`
* `iframe`
* `embed`
* `object`

**Partially replaced (UA widgets)**

* `input`
* `textarea`
* `select`
* `button`

**Not replaced (but often confused)**

* Inline `svg`
* `canvas`

<details>
<summary>Examples</summary>

```css
img, video, iframe {
  display: block;
}

/* Key properties:
- UA paints internal content
- CSS cannot affect internal layout
- Box participates normally in layout
*/
```

</details>

---

## Intrinsic Dimensions

> Replaced elements may contribute **intrinsic width, height, and aspect ratio** to layout.

Intrinsic data provided by the resource:

* intrinsic width
* intrinsic height
* intrinsic aspect-ratio

Sizing precedence:

1. Author-specified width / height
2. Intrinsic size
3. Intrinsic ratio resolution
4. UA fallback size (≈ 300×150)

<details>
<summary>Examples</summary>

```css
img {
  width: auto;
  height: auto;
}

/* Behavior:
- Uses intrinsic dimensions
- Subject to min/max constraints
- Can overflow containers
*/
```

</details>

---

## Aspect-Ratio Interactions

> Replaced elements participate in `aspect-ratio` resolution even when one axis is auto.

```css
/* Value Blueprint
aspect-ratio = auto | <ratio>

<ratio> = <number> / <number>
*/
```

Resolution rules:

* Intrinsic ratio overrides `aspect-ratio: auto`
* Specified ratio used if one dimension is auto
* Both auto → intrinsic ratio wins
* Min/max constraints clamp final size

<details>
<summary>Examples</summary>

```css
img {
  width: 300px;
  height: auto; /* resolved via intrinsic ratio */
}

video {
  width: 100%;
  aspect-ratio: 16 / 9;
}
```

</details>

---

## Object Sizing (`object-fit` / `object-position`)

> Controls how the replaced **content** fits inside the element’s content box.

```css
/* Value Blueprint
object-fit = fill | contain | cover | none | scale-down

object-position = <position>
*/
```

<details>
<summary>Examples</summary>

```css
img {
  width: 200px;
  height: 200px;
  object-fit: cover;        /* contain | fill | none | scale-down */
  object-position: center; /* <length-percentage>{1,2} */
}

/* Notes:
- Does NOT affect layout size
- Only affects internal object rendering
*/
```

</details>

---

## CSS vs UA Styling Limits

> User agents retain control over **internal UI and behavior**, especially for form controls.

CSS can control:

* Box size & positioning
* Margin / border / padding
* Transform, opacity, visibility

CSS cannot reliably control:

* Native widgets
* Internal control layout
* Media playback chrome

<details>
<summary>Examples</summary>

```css
input {
  appearance: none; /* UA-dependent */
  width: 200px;
}

/* Notes:
- Full control requires custom components
- appearance has inconsistent support
*/
```

</details>

---

## Layout Participation

> Replaced elements integrate into layout modes as **atomic boxes**.

Layout behavior:

* Inline replaced → baseline-aligned
* Flex items → intrinsic size = flex-basis:auto
* Grid items → contribute intrinsic size to tracks
* Tables → treated as atomic cells

<details>
<summary>Examples</summary>

```css
.container {
  display: flex;
}

.container img {
  flex: 0 1 auto; /* auto resolves to intrinsic size */
}
```

</details>

---

## Notes

* Replaced elements establish no internal formatting context
* Intrinsic sizing heavily influences flex/grid algorithms
* `object-fit` never affects box metrics
* Accessibility depends on UA rendering

## Caveats

* Form controls remain UA-dependent
* Intrinsic sizes can cause overflow surprises
* `aspect-ratio` + intrinsic sizing can create cycles
