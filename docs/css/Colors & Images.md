---

id: css-colors-images
title: Colors & Images
sidebar_position: 17
--------------------

# Colors & Images

> CSS color and image rendering is governed by modern color spaces, interpolation rules, alpha compositing, and image resolution selection — all resolved before painting.

---

## Color Spaces

> CSS supports multiple color spaces with different gamuts and perceptual models.

<details>
<summary>Examples</summary>

```css
.box {
  color: rgb(255 0 0);                 /* sRGB */
  background: color(display-p3 1 0 0); /* wide-gamut */
  border-color: oklch(62% 0.25 29);    /* perceptual */
}

/* Spaces:

- sRGB        → legacy, device-mapped
- display-p3  → wide-gamut (Apple / HDR displays)
- lab / lch   → absolute colorimetric
- oklab / oklch → perceptual, uniform

Notes:
- Colors are converted to a common space before blending
- Out-of-gamut values are clipped
*/
```

</details>

---

## Color Interpolation

> Interpolation defines how colors blend in gradients, transitions, and animations.

<details>
<summary>Examples</summary>

```css
.gradient {
  background: linear-gradient(
    in oklch,
    red,
    blue
  );
}

/* Interpolation rules:

- Default space: sRGB
- Can be overridden: in lab | lch | oklab | oklch
- Hue interpolation may be:
  - shorter | longer | increasing | decreasing

Perceptual spaces avoid muddy midpoints
*/
```

</details>

---

## Alpha Compositing

> Alpha compositing blends colors using the Porter–Duff model.

<details>
<summary>Examples</summary>

```css
.overlay {
  background: rgb(0 0 0 / 0.5);
}

/* Formula (simplified):

result = src × α + dst × (1 − α)

Notes:
- Applied during painting
- Happens after color interpolation
- Affected by stacking contexts
*/
```

</details>

---

## Gradients

> Gradients are images generated procedurally and sized like replaced images.

<details>
<summary>Examples</summary>

```css
.bg {
  background:
    linear-gradient(45deg, red, blue),
    radial-gradient(circle at center, white, black),
    conic-gradient(from 0deg, red, yellow, green);
}

/* Types:

- linear-gradient()
- radial-gradient()
- conic-gradient()

Notes:
- Gradients have no intrinsic size
- They participate in background-size & positioning
*/
```

</details>

---

## `image-set()`

> `image-set()` selects the most appropriate image based on resolution or type.

<details>
<summary>Examples</summary>

```css
.hero {
  background-image: image-set(
    url("hero@1x.png") 1x,
    url("hero@2x.png") 2x,
    url("hero.avif") type("image/avif")
  );
}

/* Notes:

- Resolution selection happens at computed value time
- type() enables format negotiation
- Similar to <picture> element logic
*/
```

</details>

---

## Images as Replaced Content

> Images behave as replaced elements with intrinsic dimensions.

<details>
<summary>Examples</summary>

```css
img {
  max-width: 100%;
  height: auto;
}

/* Rules:

- Intrinsic width/height from image metadata
- aspect-ratio may override
- Object-fit controls content box mapping
*/
```

</details>

---

## Notes

* Color interpolation occurs before compositing
* Wide-gamut colors require compatible displays
* Gradients are resolution-independent
* Image selection happens before layout

## Caveats

* Mixing color spaces may cause clipping
* Older browsers default to sRGB
* HDR colors may appear muted on SDR displays
