---

id: css-typography-engine
title: Typography Engine
sidebar_position: 16
--------------------

# Typography Engine

> CSS typography is governed by a multi-stage text engine involving font selection, glyph shaping, metric resolution, line box construction, and overflow handling.

---

## Font Selection Algorithm

> Font selection resolves a glyph by walking the font-family list, matching style axes, and falling back per Unicode range.

<details>
<summary>Examples</summary>

```css
body {
  font-family: "Inter", "Segoe UI", system-ui, sans-serif;
  font-weight: 450; /* variable font axis */
}

/* Resolution order:

1. font-family list order
2. font-style / weight / stretch matching
3. Unicode-range coverage
4. System fallback

Notes:
- Font fallback happens per-glyph
- Mixed fonts in a single word are possible
*/
```

</details>

---

## `@font-face` Descriptors

> `@font-face` defines downloadable fonts and their matching characteristics.

<details>
<summary>Examples</summary>

```css
@font-face {
  font-family: "MyFont";
  src:
    url("myfont.woff2") format("woff2"),
    url("myfont.woff") format("woff");
  font-weight: 100 900;      /* variable range */
  font-style: normal italic; /* axis mapping */
  font-display: swap;        /* auto | block | swap | fallback | optional */
  unicode-range: U+000-5FF;  /* Latin subset */
}

/* Notes:

- Descriptors participate in font matching
- unicode-range enables subsetting
- font-display affects FOIT/FOUT
*/
```

</details>

---

## Variable Fonts

> Variable fonts expose continuous design axes instead of discrete font files.

<details>
<summary>Examples</summary>

```css
.text {
  font-variation-settings:
    "wght" 650,  /* weight */
    "wdth" 110,  /* width */
    "slnt" -5;   /* slant */
}

/* Notes:

- font-weight / stretch / style map to axes first
- font-variation-settings overrides them
- Axes are font-specific
*/
```

</details>

---

## Line-Height Resolution

> `line-height` controls line box height and inline alignment, not font size.

<details>
<summary>Examples</summary>

```css
p {
  font-size: 16px;
  line-height: 1.5;  /* unitless multiplier */
}

span {
  line-height: normal; /* font-metric dependent */
}

/* Resolution:

- Unitless → multiplies font-size
- Length → absolute line box height
- Inherited as computed value

Visual:

baseline
│
│   ascent
│  ┌──────┐
│  │ text │
│  └──────┘
│   descent
*/
```

</details>

---

## Text Overflow & Wrapping

> Text overflow is handled during inline formatting and line breaking.

<details>
<summary>Examples</summary>

```css
.text {
  white-space: nowrap;          /* normal | nowrap | pre | pre-wrap | pre-line */
  overflow: hidden;
  text-overflow: ellipsis;      /* clip | ellipsis */
}

.paragraph {
  overflow-wrap: break-word;    /* normal | anywhere */
  word-break: break-all;        /* normal | keep-all */
  hyphens: auto;                /* manual | auto */
}

/* Notes:

- text-overflow works only on single-line boxes
- overflow-wrap affects unbreakable strings
- word-break is more aggressive
*/
```

</details>

---

## Glyph Shaping & Directionality

> Text shaping converts characters into positioned glyphs using OpenType features.

<details>
<summary>Examples</summary>

```css
.text {
  direction: rtl;              /* ltr | rtl */
  unicode-bidi: isolate;       /* bidi control */
  font-feature-settings:
    "liga" on,
    "kern" on;
}

/* Notes:

- Shaping happens before line breaking
- Required for Arabic, Indic scripts
- Bidi reorders glyphs visually
*/
```

</details>

---

## Notes

* Font selection is per-glyph, not per-element
* Line boxes are influenced by the tallest inline
* Variable fonts reduce file count
* Typography interacts deeply with writing modes

## Caveats

* Font metrics differ across platforms
* line-height: normal is font-dependent
* text-overflow does not affect layout width
