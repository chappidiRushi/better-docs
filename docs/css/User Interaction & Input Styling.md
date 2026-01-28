---

id: css-user-interaction
title: User Interaction & Input Styling
sidebar_position: 25
--------------------

# User Interaction & Input Styling

> CSS participates in **input handling, hit testing, selection, and pointer negotiation**, sitting between hardware capabilities, UA heuristics, and author intent.

Progression: **input capability detection → pointer negotiation → hit testing → text interaction → caret control**.

---

## Pointer & Hover Media Features

> Media queries describe **what the primary input device can do**, not what is currently being used.

Relevant features:

```css
@media (hover: hover | none)
@media (pointer: fine | coarse | none)
@media (any-hover: hover | none)
@media (any-pointer: fine | coarse | none)
```

Rules:

* `pointer` / `hover` → primary input
* `any-*` → union of all available inputs
* Do NOT assume `hover: none` means touch-only

<details>
<summary>Examples</summary>

```css
/* Hover affordances only when reliable */
@media (hover: hover) {
  .menu-item:hover {
    background: #eee;
  }
}

/* Coarse pointer hit targets */
@media (pointer: coarse) {
  button {
    min-height: 44px; /* touch guideline */
  }
}
```

</details>

---

## `touch-action`

> Controls **browser gesture handling** before JS runs.

Blueprint:

```css
touch-action:
  auto |
  none |
  pan-x | pan-y |
  pinch-zoom |
  manipulation; /* pan + zoom */
```

Rules:

* Applied during hit testing
* Required for high-performance pointer events
* Ignored by non-touch inputs

<details>
<summary>Examples</summary>

```css
/* Prevent scroll hijacking */
.map {
  touch-action: pan-x pan-y; /* allow scroll, block zoom */
}

/* Custom gesture surface */
.canvas {
  touch-action: none; /* JS handles all gestures */
}
```

</details>

---

## `user-select`

> Controls **text selection behavior**, not focus.

Blueprint:

```css
user-select: auto | none | text | all;
```

Rules:

* Selection does not imply copyability
* Some UA elements override this (inputs, textareas)

<details>
<summary>Examples</summary>

```css
/* Prevent accidental selection */
.button {
  user-select: none;
}

/* Force selectable UI text */
.code {
  user-select: text;
}
```

</details>

---

## Hit Testing

> Determines **which element receives pointer events**.

Relevant properties:

```css
pointer-events: auto | none;
```

Rules:

* Hit testing respects visual geometry
* Transforms affect hit region
* `pointer-events: none` removes element from target chain

<details>
<summary>Examples</summary>

```css
/* Click-through overlay */
.overlay {
  pointer-events: none;
}

/* SVG fine-grained hit control */
svg path {
  pointer-events: visibleStroke; /* SVG-specific */
}
```

</details>

---

## Caret Styling

> Controls **text cursor appearance**, not text input behavior.

Blueprint:

```css
caret-color: auto | <color>;
```

Rules:

* Inherits
* Applies to editable elements only
* Can affect perceived contrast

<details>
<summary>Examples</summary>

```css
/* High-contrast caret */
input, textarea {
  caret-color: red;
}

/* Theme-aware caret */
.dark {
  caret-color: CanvasText;
}
```

</details>

---

## CSS vs Input Semantics

> CSS cannot change input meaning.

Limits:

* Cannot redefine gestures
* Cannot change keyboard behavior
* Cannot override platform accessibility input rules

CSS **can**:

* Negotiate gestures
* Improve hit accuracy
* Reduce accidental interactions

---

## Notes

* Input capability queries are hints, not guarantees
* Gesture control happens before JS
* Hit testing is geometry + visibility based
* Caret styling affects usability, not behavior

## Caveats

* Overusing `touch-action: none` breaks scrolling
* Removing selection harms accessibility
* Pointer media queries are device-dependent
* Always test with mouse, touch, keyboard
