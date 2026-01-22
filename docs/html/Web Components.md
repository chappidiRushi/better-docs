---

id: html-web-components
title: Web Components
sidebar_position: 22
--------------------

# HTML and Web Components

> How HTML is extended using Custom Elements, Shadow DOM, slots, and declarative composition — and where HTML alone stops.

---

## Unified Example (Custom Element + Shadow DOM + Slots)

```html
<!-- DECLARATIVE SHADOW DOM -->
<user-card>
  <template shadowroot="open"> <!-- open | closed -->
    <style>
      :host { display: block; border: 1px solid #ccc; padding: 1rem; }
      ::slotted(h2) { color: teal; }
    </style>

    <slot name="title"></slot> <!-- named slot -->
    <slot></slot> <!-- default slot -->
  </template>

  <h2 slot="title">John Doe</h2> <!-- slot assignment -->
  <p>Content from light DOM</p>
</user-card>

<script>
  class UserCard extends HTMLElement {
    constructor() {
      super(); // required
    }
  }

  customElements.define('user-card', UserCard); // name must contain hyphen
</script>
```

---

## Custom Elements Overview

* User-defined HTML elements
* Must contain a hyphen (`my-element`)
* Registered via `customElements.define()`

**Base classes:**

* `HTMLElement`
* `HTMLButtonElement` (customized built-ins)

**Lifecycle callbacks:**

* `connectedCallback()`
* `disconnectedCallback()`
* `attributeChangedCallback()`
* `adoptedCallback()`

> One-liner: *Custom Elements let you define new HTML tags with behavior.*

---

## HTML Extensions via Custom Elements

* Autonomous custom elements:

  * `<my-widget>`

* Customized built-ins:

```html
<button is="fancy-button"></button> <!-- limited support -->
```

* Attributes reflect as strings
* Properties hold state

> One-liner: *HTML is extended by registering new element types, not by modifying the parser.*

---

## Slotting and Composition

* `<slot>` defines insertion points
* Light DOM projected into Shadow DOM
* Slot assignment via `slot` attribute

**Rules:**

* Unassigned nodes go to default slot
* Slots can have fallback content
* Re-slotting supported dynamically

> One-liner: *Slots compose DOM trees without merging them.*

---

## Declarative Shadow DOM

* Shadow root defined in HTML
* Parsed before JS executes
* Enables SSR + hydration

**Attributes:**

* `shadowroot="open | closed"`

**Benefits:**

* Faster first paint
* No JS bootstrapping race

> One-liner: *Declarative Shadow DOM brings Web Components to server-rendered HTML.*

---

## Limitations of HTML Alone

* No state management
* No reactivity
* No conditional rendering
* No loops

**Requires JS for:**

* Dynamic updates
* Event handling
* Data binding

> One-liner: *HTML declares structure; JavaScript provides behavior.*

---

## Often-Missed Details (Hardcore)

* Custom elements upgrade lazily
* Unregistered elements render as `HTMLElement`
* Attribute changes are string-based
* Shadow DOM retargets events
* CSS inheritance stops at shadow boundary

---

## Mental Model (Interview Grade)

> **Web Components extend HTML by adding new element types and encapsulation boundaries — but HTML remains declarative, static, and behavior-agnostic without JavaScript.**
