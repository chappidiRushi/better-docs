---

id: interactive-elements
title: Interactive Elements
sidebar_position: 16
--------------------

# Interactive Elements

> Elements that create **native interactivity, stateful UI, focusable controls, and keyboard-driven behavior without JavaScript**.

---

## Unified Example (Interactive Model)

```html
<!-- DETAILS / SUMMARY -->
<details open> <!-- open: boolean, controls initial state -->
  <summary>Advanced Options</summary> <!-- first child summary defines toggle hit-area -->
  <p>Hidden by default. Toggled via UA-managed disclosure.</p>
</details>

<!-- DIALOG -->
<dialog id="dlg" open> <!-- open: controlled by show(), showModal(), close() -->
  <form method="dialog"> <!-- method=dialog: submit closes dialog -->
    <p>Modal or non-modal dialog content</p>
    <button value="ok">OK</button> <!-- value returned via close(value) -->
    <button value="cancel">Cancel</button>
  </form>
</dialog>

<!-- POPOVER API -->
<button popovertarget="menu">Menu</button> <!-- popovertarget: idref -->
<div id="menu" popover="auto"> <!-- popover: auto | manual -->
  <a href="#">Item 1</a>
  <a href="#">Item 2</a>
</div>

<!-- FOCUS MANAGEMENT -->
<button autofocus>Auto Focus</button> <!-- autofocus: boolean, applied on navigation -->
<div tabindex="0">Focusable Container</div> <!-- tabindex: -1 | 0 | positive (discouraged) -->

<!-- ACCESSKEY (Keyboard Shortcut) -->
<button accesskey="s">Save</button> <!-- accesskey: UA-defined modifier + key -->
```

---

## `<details>` and `<summary>`

* Native disclosure widget
* Keyboard-accessible (Enter / Space)
* `open` reflects expanded state
* Only the **first `<summary>`** is honored
* Summary is implicitly focusable

> One-liner: *Declarative toggle state with built-in accessibility and keyboard support.*

---

## `<dialog>`

* Represents application dialogs
* Two modes:

  * Non-modal: `show()`
  * Modal (top-layer): `showModal()`
* Traps focus when modal
* Integrated with forms via `method="dialog"`

> One-liner: *Top-layer element with UA-managed focus trapping and dismissal semantics.*

---

## Popover API

* Declarative transient UI (menus, tooltips, pickers)
* Automatically manages:

  * Focus return
  * Escape dismissal
  * Light-dismiss (outside click)
* `auto`: one popover at a time
* `manual`: developer-controlled lifecycle

> One-liner: *Standardized replacement for ad-hoc dropdown JS.*

---

## Focus Management

* Focus order defined by:

  1. DOM order
  2. `tabindex=0`
* `tabindex=-1`: programmatic focus only
* Positive tabindex breaks accessibility (anti-pattern)
* `autofocus` applies once per navigation

> One-liner: *Focus is a first-class HTML primitive, not a JS afterthought.*

---

## Keyboard Interaction Models

* Buttons: Enter / Space
* Links: Enter
* Details/Summary: Enter / Space
* Dialog:

  * Esc closes (modal)
  * Tab cycles within focus trap
* Popover:

  * Esc dismiss
  * Arrow keys UA-defined

> One-liner: *HTML defines keyboard semantics per role — JS should not override them.*

---

## Related / Often-Missed APIs

* `inert` attribute — removes subtree from focus & accessibility tree
* `hidden` attribute — removes from rendering & accessibility
* Top-layer stacking context (dialog, popover, fullscreen)
* `:focus-visible` vs `:focus`

---

## Mental Model (Interview Grade)

> **Interactive HTML elements encode state, focus, keyboard behavior, and accessibility at the platform level — JavaScript only orchestrates, it does not replace them.**
