---

id: css-accessibility
title: Accessibility-Relevant CSS
sidebar_position: 24
--------------------

# Accessibility‑Relevant CSS

> CSS plays a **direct role in accessibility** by influencing visibility, focus order, motion, contrast, and how assistive technologies interpret rendered content. These rules interact with **UA styles, OS modes, and user preferences**.

Progression: **visibility → focus → user preferences → system overrides → contrast pitfalls**.

---

## Visual vs Semantic Hiding

> Hiding content visually is not the same as removing it from the accessibility tree.

Key models:

* **Visual hiding** → content remains accessible
* **Semantic hiding** → content removed from a11y tree
* **Layout removal** → element not rendered

```css
/* Blueprint */
visibility: visible | hidden;
display: none;
opacity: <number>; /* 0–1 */
clip-path: inset(...);
position: absolute; left: -9999px;
```

Rules:

* `display: none` / `visibility: hidden` → removed from a11y tree
* `opacity: 0` → still focusable & announced
* Clipping / off-screen → screen-reader accessible

<details>
<summary>Examples</summary>

```css
/* Screen-reader-only utility */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  overflow: hidden;
  clip-path: inset(50%);
  white-space: nowrap;
}

/* BAD: visually hidden but still tabbable */
.hidden {
  opacity: 0;
}
```

</details>

---

## Focus Management

> CSS determines **focus visibility**, not focus order.

Focus sources:

* Keyboard navigation
* Programmatic focus
* UA heuristics

Relevant properties:

```css
outline: auto | none | <length> <style> <color>;
outline-offset: <length>;
:focus
:focus-visible
:focus-within
```

Rules:

* Removing outlines without replacement is an a11y failure
* `:focus-visible` respects UA heuristics
* `:focus-within` enables group-level affordances

<details>
<summary>Examples</summary>

```css
/* Accessible custom focus */
button:focus-visible {
  outline: 2px solid Highlight; /* system color */
  outline-offset: 2px;
}

/* Parent reacts when child is focused */
.form-group:focus-within {
  border-color: blue;
}
```

</details>

---

## Reduced Motion

> Motion preferences are **user-defined constraints**, not suggestions.

Query:

```css
@media (prefers-reduced-motion: reduce)
```

Implications:

* Disable non-essential animations
* Replace motion with opacity or instant transitions
* Avoid infinite animations

<details>
<summary>Examples</summary>

```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

</details>

---

## Forced Colors Mode

> Forced colors mode overrides author styles to ensure legibility.

Query:

```css
@media (forced-colors: active)
```

Key behaviors:

* Author colors replaced by system colors
* Borders/backgrounds may disappear
* Gradients & images may be suppressed

Special properties:

```css
forced-color-adjust: auto | none;
```

<details>
<summary>Examples</summary>

```css
@media (forced-colors: active) {
  .button {
    forced-color-adjust: none; /* opt-out selectively */
    border: 1px solid ButtonText;
    background: ButtonFace;
  }
}
```

</details>

---

## Contrast & Color Pitfalls

> Passing contrast ratios requires **stable backgrounds**.

Common pitfalls:

* Text over gradients or images
* Semi-transparent overlays
* Color-only state indicators

Relevant queries & features:

```css
@media (prefers-contrast: more | less)
@media (dynamic-range: high)
```

<details>
<summary>Examples</summary>

```css
/* Contrast-safe overlay pattern */
.hero::before {
  content: "";
  position: absolute;
  inset: 0;
  background: rgba(0,0,0,0.5); /* stabilizes contrast */
}

@media (prefers-contrast: more) {
  .text {
    color: CanvasText;
    background: Canvas;
  }
}
```

</details>

---

## CSS vs Assistive Technology Boundaries

> CSS cannot fix semantic issues.

Limits:

* Cannot change reading order
* Cannot add semantics
* Cannot expose hidden content meaningfully

CSS **can**:

* Improve visibility
* Respect user preferences
* Avoid breaking focus & contrast

---

## Notes

* Accessibility modes override author intent
* Focus visibility is mandatory
* Motion & color preferences are first‑class inputs
* CSS participates in a11y but does not own semantics

## Caveats

* Overusing `forced-color-adjust: none` is harmful
* Removing outlines without alternatives breaks navigation
* Visual hiding patterns must be deliberate
* Always test with keyboard + screen readers
