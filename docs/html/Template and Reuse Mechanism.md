---

id: template-reuse
title: Template and Reuse Mechanisms
sidebar_position: 18
--------------------

# Template and Reuse Mechanisms

> Native HTML mechanisms for **declarative reuse, inert markup, efficient cloning, and DOM composition**.

---

## Unified Example (Template → Fragment → Shadow DOM)

```html
<!-- TEMPLATE -->
<template id="card-tpl"> <!-- inert: not rendered, not parsed into DOM -->
  <article class="card"> <!-- no scripts run, no images load -->
    <h2 class="title"></h2>
    <p class="content"></p>
  </article>
</template>

<!-- HOST ELEMENT -->
<div id="host"></div>

<script type="module">
  const tpl = document.getElementById('card-tpl');

  // DOCUMENT FRAGMENT (implicit)
  const fragment = tpl.content.cloneNode(true); // deep clone: true | shallow: false

  fragment.querySelector('.title').textContent = 'Title';
  fragment.querySelector('.content').textContent = 'Reusable content';

  // SHADOW DOM ATTACHMENT
  const host = document.getElementById('host');
  const shadow = host.attachShadow({ mode: 'open' }); // open | closed
  shadow.append(fragment);
</script>
```

---

## `<template>`

* Holds inert DOM subtree
* Not rendered
* Not focusable
* Scripts do **not execute**
* Images / media do **not load**
* Parsed once, cloned many times

**Special properties:**

* `template.content` → `DocumentFragment`

> One-liner: *`<template>` is parsed HTML without side effects.*

---

## Document Fragments

* Lightweight container for DOM nodes
* Not part of the live DOM tree
* Appending fragment moves its children
* Minimizes reflow / repaint

**Creation:**

* `document.createDocumentFragment()`
* `template.content`

> One-liner: *Fragments batch DOM mutations efficiently.*

---

## Cloning and Instantiation

* `cloneNode(deep)`

  * `true`: deep clone (subtree)
  * `false`: shallow clone (node only)

**Cloning semantics:**

* IDs duplicated (must be fixed)
* Event listeners NOT cloned
* State (value, checked) cloned at time of clone

> One-liner: *Cloning copies structure, not behavior.*

---

## Shadow DOM Interaction

* Shadow root isolates:

  * DOM tree
  * CSS scope
  * Event retargeting

**attachShadow options:**

* `mode: open | closed`

**Interaction rules:**

* Light DOM slotted via `<slot>`
* Events bubble but are retargeted
* Styles do not leak across boundary

> One-liner: *Shadow DOM composes DOM trees without global side effects.*

---

## `<slot>` (Composition Point)

```html
<template id="layout">
  <slot name="header"></slot> <!-- named slot -->
  <slot></slot> <!-- default slot -->
</template>
```

* Fallback content allowed
* Slot assignment via `slot` attribute
* Dynamic reassignment supported

> One-liner: *Slots define insertion points between DOM trees.*

---

## Often-Missed Details (Hardcore)

* `<template>` can appear anywhere (including `<head>`)
* Template contents are parsed in the **HTML namespace**
* Scripts inside templates execute only after cloning + insertion
* Shadow DOM does NOT prevent JS access
* CSS `:host`, `:host-context()` control styling

---

## Mental Model (Interview Grade)

> **HTML reuse is achieved by combining inert markup (`<template>`), cheap cloning (`DocumentFragment`), and encapsulation (Shadow DOM) — without string concatenation or runtime parsing.**
