---

id: web-components

title: Web Components

sidebar_position: 12

-----------

# Web Components

> Native component model for encapsulation, reuse, and interoperability without frameworks

---

## Web Components Basics

```txt
Suite of standards enabling custom, encapsulated HTML elements
```

```txt
Core pillars:
- Custom Elements
- Shadow DOM
- HTML Templates
```

<details>
<summary>Examples</summary>

```txt
Goals:
- Encapsulation
- Reusability
- Style isolation
```

```html
<my-widget></my-widget> <!-- custom element -->
```

</details>

---

## Custom Elements

```txt
Define new HTML tags with custom behavior
```

```js
customElements.define(name, constructor, options?)
// name: kebab-case string
// constructor: class extends HTMLElement
// options: { extends?: string }
```

<details>
<summary>Examples</summary>

```js
class MyBox extends HTMLElement {
  connectedCallback() {
    this.textContent = 'Hello';
  }
}

customElements.define('my-box', MyBox);
```

```js
// lifecycle callbacks
connectedCallback();
disconnectedCallback();
attributeChangedCallback(name, oldVal, newVal);
```

```txt
Rules:
- Name must contain hyphen
- Must extend HTMLElement (or built-in via is="")
```

</details>

---

## Shadow DOM

```txt
Encapsulated DOM subtree with scoped styles and markup
```

```js
element.attachShadow({ mode: 'open' | 'closed' })
```

<details>
<summary>Examples</summary>

```js
class ShadowEl extends HTMLElement {
  constructor() {
    super();
    const root = this.attachShadow({ mode: 'open' });
    root.innerHTML = `
      <style>p { color: red }</style>
      <p>Scoped</p>
    `;
  }
}

customElements.define('shadow-el', ShadowEl);
```

```txt
Features:
- Style scoping
- DOM encapsulation
- Slot-based composition
```

```html
<slot name="content"></slot>
```

</details>

---

## Notes

* Web Components are framework-agnostic
* Native browser support (no build step)
* Interop with React/Vue requires care

## Caveats

* Styling across shadow boundaries is limited
* Server-side rendering is non-trivial
* Overuse can harm accessibility if misdesigned
