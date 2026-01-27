---

id: html-and-accessibility-a11y
title: HTML and Accessibility (A11y)
sidebar_position: 19
--------------------

# HTML and Accessibility (A11y)

> How HTML semantics map to the accessibility tree, drive assistive technologies, and define keyboard and focus behavior.

---

## Unified Example (Semantic-First Accessibility)

```html
<header> <!-- implicit banner landmark -->
  <nav aria-label="Primary"> <!-- nav landmark -->
    <a href="#main">Skip to content</a> <!-- keyboard bypass -->
    <a href="/home">Home</a>
  </nav>
</header>

<main id="main"> <!-- main landmark (unique) -->
  <article> <!-- article landmark -->
    <h1>Title</h1> <!-- heading level defines outline -->

    <button>Action</button> <!-- role=button, focusable, keyboard-enabled -->

    <div role="button" tabindex="0">Bad Button</div> <!-- anti-pattern -->

    <input type="text" aria-label="Name"> <!-- accessible name -->
  </article>
</main>

<footer> <!-- contentinfo landmark -->
  <small>©</small>
</footer>
```

---

## Native Semantics vs ARIA

* Native HTML defines:

  * Role
  * Name computation
  * Keyboard behavior
  * Focusability

* ARIA:

  * Supplements missing semantics
  * Does NOT add behavior
  * Can override native roles (dangerous)

**Rule:**

> *Use ARIA only when HTML has no semantic equivalent.*

---

## Accessibility Tree Mapping

* Separate tree from DOM
* Generated from:

  * Element semantics
  * ARIA roles / properties
  * Visibility (`hidden`, `display:none`)

**Excluded from tree:**

* `display: none`
* `hidden`
* `aria-hidden="true"`

> One-liner: *ATs never read the DOM — they read the accessibility tree.*

---

## Landmark Roles

**Implicit landmarks:**

* `<header>` → banner (contextual)
* `<nav>` → navigation
* `<main>` → main (one per document)
* `<aside>` → complementary
* `<footer>` → contentinfo (contextual)

**Explicit ARIA landmarks:**

* `role="banner"`
* `role="navigation"`
* `role="main"`

> One-liner: *Landmarks define page regions for AT navigation.*

---

## Focus Order and Tab Navigation

* Default tab order: DOM order

* Focusable by default:

  * links, buttons, inputs

* `tabindex`:

  * `0`: include in tab order
  * `-1`: programmatic focus only
  * `>0`: anti-pattern

* `inert`: removes subtree from focus + a11y tree

> One-liner: *Focus order is structural, not visual.*

---

## Keyboard Accessibility

* Buttons: Enter / Space
* Links: Enter
* Inputs: type-specific
* Dialogs:

  * Esc closes
  * Focus trapped (modal)

**ARIA does NOT add keyboard support**

> One-liner: *If it’s clickable, it must be operable by keyboard.*

---

## Screen Reader Behavior

* Announces:

  * Role
  * Name
  * State

**Accessible name sources (priority):**

1. `aria-label`
2. `aria-labelledby`
3. `<label>`
4. Text content

> One-liner: *If it has no accessible name, it doesn’t exist to AT.*

---

## Common Anti-Patterns

* `<div role="button">` without keyboard handlers
* Positive `tabindex`
* `aria-hidden` on focusable content
* Overriding native roles (`role="button"` on `<button>`)
* Click handlers without keyboard equivalents

> One-liner: *Most accessibility bugs come from fighting native HTML.*

---

## Mental Model (Interview Grade)

> **HTML accessibility is automatic when semantics, focus order, and keyboard behavior are respected — ARIA is a last resort, not a foundation.**
