---

id: css-inline-layout-text-flow
title: Inline Layout & Text Flow
sidebar_position: 10
--------------------

# Inline Layout & Text Flow

> Defines how inline-level content is fragmented into lines, how line boxes are constructed, how baselines align, and how text and inline elements wrap, break, and interact with each other. This is one of the most spec-heavy and engine-sensitive parts of CSS.

---

## Inline Formatting Algorithm

> Algorithm that lays out inline-level boxes inside a block container by generating line boxes, placing inline fragments, and handling wrapping, alignment, and overflow.

<details>
<summary>Examples</summary>

```css
p {
  font-size: 16px;
  line-height: 1.5;      /* unitless | number | length | percentage */
  white-space: normal;  /* normal | nowrap | pre | pre-wrap | pre-line | break-spaces */
}

/* Algorithm characteristics:
   - Inline boxes are fragmented across lines
   - Layout proceeds left-to-right or right-to-left based on direction
   - Shaping happens before line breaking
*/
```

</details>

---

## Line Box Construction

> Line boxes are rectangular areas that contain inline-level fragments. Their height is determined by the maximum of inline box metrics participating in the line.

<details>
<summary>Examples</summary>

```css
p {
  line-height: 24px;     /* defines minimum line box height */
}

span {
  font-size: 32px;       /* increases line box height */
}

img {
  vertical-align: middle; /* affects line box alignment */
}

/* Notes:
   - Each visual line creates a new line box
   - Inline elements can increase line box height
   - Line boxes do not overlap
*/
```

</details>

---

## Baseline Alignment

> Inline and inline-block elements align relative to baselines, which are font-metric-dependent reference lines used for vertical alignment.

<details>
<summary>Examples</summary>

```css
.text {
  font-size: 16px;
}

.inline-block {
  display: inline-block;
  height: 40px;
  vertical-align: baseline; /* baseline | middle | top | bottom | text-top | text-bottom */
}

/* Baseline rules:
   - Text baseline comes from font metrics
   - Inline-block baseline is bottom margin edge
   - Replaced elements have special baseline rules
*/
```

</details>

---

## `vertical-align` Reality

> `vertical-align` does not align elements globally; it only affects alignment within a single line box or table cell.

<details>
<summary>Examples</summary>

```css
span {
  vertical-align: super;   /* baseline | sub | super | middle | top | bottom | <length> | <percentage> */
}

.icon {
  vertical-align: -4px;    /* length relative to baseline */
}

/* Reality check:
   - Has no effect on block-level layout
   - Often misused for centering
   - Percentage resolves against line-height
*/
```

</details>

---

## Inline Replaced Elements

> Replaced elements (img, video, iframe) participate in inline layout but use intrinsic dimensions instead of font metrics.

<details>
<summary>Examples</summary>

```css
img {
  width: 40px;            /* auto | length | percentage */
  height: auto;
  vertical-align: text-bottom;
}

iframe {
  aspect-ratio: 16 / 9;   /* affects intrinsic sizing */
}

/* Characteristics:
   - Use intrinsic size when width/height are auto
   - Baseline behavior differs from text
   - Contribute to line box height
*/
```

</details>

---

## Text Wrapping & Breaking

> Controls how text is wrapped, broken, and hyphenated when reaching line boundaries.

<details>
<summary>Examples</summary>

```css
p {
  overflow-wrap: break-word; /* normal | break-word | anywhere */
  word-break: normal;        /* normal | break-all | keep-all */
  hyphens: auto;             /* none | manual | auto */
}

.code {
  white-space: pre;          /* preserves whitespace, disables wrapping */
}

/* Wrapping rules:
   - Soft wrap opportunities depend on language
   - Breaking happens after shaping
   - Hyphenation is language-aware
*/
```

</details>

---

## Notes

* Inline layout is font-metric driven
* Line boxes are ephemeral layout artifacts
* Most vertical alignment bugs are baseline-related
* Text shaping precedes line breaking

## Caveats

* Inline layout differs subtly across engines
* `vertical-align` is frequently misunderstood
* Mixing large inline-blocks with text affects line height
