---

id: css-tooling-debugging
title: Tooling & Debugging
sidebar_position: 29
--------------------

# Tooling & Debugging

> Effective CSS debugging requires understanding **what the browser computed, what it actually used, and why**. Tooling exposes different layers of the rendering pipeline.

Progression: **inspection → value resolution → layout diagnostics → source mapping → regression prevention**.

---

## DevTools Deep Inspection

> DevTools exposes **author styles, cascaded rules, and overridden declarations**.

Key panels:

* **Elements** → DOM + matched rules
* **Styles** → cascade order & specificity
* **Computed** → final computed values
* **Layout** → box model, grids, flex overlays

Rules:

* Order in Styles ≠ cascade order
* Overridden rules still participate in cascade

<details>
<summary>Examples</summary>

```text
Elements → Styles
  .btn { color: red }        (overridden)
  button { color: blue }     (used)
```

</details>

---

## Computed vs Used Values

> Computed values are not always what gets painted.

Value stages:

1. Specified value
2. Computed value
3. Used value
4. Actual value (after rounding, snapping)

Examples of divergence:

* Percentages resolved late
* `auto` resolved during layout
* Fonts snapped to device pixels

<details>
<summary>Examples</summary>

```css
.box {
  width: 50%; /* computed as 50%, used after layout */
}

.text {
  font-size: 16px; /* used may be 15.875px */
}
```

</details>

---

## Layout Debugging

> Visual overlays expose formatting contexts.

Tools:

* Flexbox overlay
* Grid overlay
* Scroll containers
* Paint flashing

Common layout bugs:

* Unexpected containing blocks
* Min/max constraint conflicts
* Collapsing margins

<details>
<summary>Examples</summary>

```text
Enable:
  DevTools → Layout → Grid → Show track sizes
```

</details>

---

## Source Maps

> Source maps map generated CSS back to authoring sources.

Supports:

* Sass / Less
* PostCSS
* CSS-in-JS

Rules:

* Maps affect DevTools only
* Incorrect maps mislead debugging

<details>
<summary>Examples</summary>

```css
/* Generated CSS */
.button { color: red; }

/* Source map links back to button.scss */
```

</details>

---

## Visual Regression Testing

> Prevents **unintentional visual changes**.

Approaches:

* Screenshot diffing
* DOM snapshot comparison
* Perceptual diffing

Tools (conceptual):

* Playwright
* Percy
* Chromatic

<details>
<summary>Examples</summary>

```text
Baseline → Change → Diff → Review → Approve
```

</details>

---

## Debugging Strategy

> Always localize the failure.

Checklist:

1. Is the rule applied?
2. Is it overridden?
3. Is the value resolved as expected?
4. Is layout constraining it?
5. Is paint/composite hiding it?

---

## Notes

* DevTools shows **symptoms**, not intent
* Computed ≠ painted
* Layout tools save hours
* Automate visual checks

## Caveats

* DevTools behavior differs per browser
* Rounding issues are platform-specific
* Source maps can lie
* Pixel diffs need human review
