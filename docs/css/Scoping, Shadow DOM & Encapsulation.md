---

id: css-shadow-dom
title: Scoping, Shadow DOM & Encapsulation
sidebar_position: 27
--------------------

# Scoping, Shadow DOM & Encapsulation

> Shadow DOM introduces **hard style boundaries**. CSS scoping here is not convention-based but enforced by the browser, with **explicit escape hatches**.

Progression: **DOM trees → encapsulation rules → allowed piercings → theming patterns → isolation guarantees**.

---

## Shadow Tree Styling Model

> Each shadow root creates an isolated styling scope.

Key rules:

* Styles in light DOM **do not cross** into shadow DOM
* Styles in shadow DOM **do not leak out**
* Shadow DOM has its own cascade & tree order

Shadow tree types:

* **Open** → JS-accessible (`element.shadowRoot`)
* **Closed** → JS-inaccessible (CSS rules identical)

```text
Document Tree
 └─ Custom Element
     └─ Shadow Root (new style scope)
```

<details>
<summary>Examples</summary>

```html
<my-card></my-card>

<script>
class MyCard extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.innerHTML = `
      <style>
        p { color: red; }
      </style>
      <p>Shadow text</p>
    `;
  }
}
</script>
```

</details>

---

## `:host`

> Targets the **custom element itself** from inside its shadow tree.

Blueprint:

```css
:host
:host(<selector>)
:host-context(<selector>) /* legacy / limited */
```

Rules:

* `:host` replaces styling the custom element
* `:host()` allows conditional styling
* `:host-context()` matches ancestors (discouraged)

<details>
<summary>Examples</summary>

```css
:host {
  display: block;
}

:host([variant="danger"]) {
  border: 1px solid red;
}
```

</details>

---

## `::part()`

> Controlled, explicit styling API for shadow internals.

Blueprint:

```css
::part(name)
```

Rules:

* Parts must be explicitly exposed
* Only exposed parts are stylable
* No selector chaining inside shadow tree

<details>
<summary>Examples</summary>

```html
<!-- shadow DOM -->
<button part="label">OK</button>
```

```css
my-button::part(label) {
  font-weight: bold;
}
```

</details>

---

## CSS Variable Piercing

> Custom properties **cross shadow boundaries by design**.

Rules:

* Variables inherit into shadow trees
* Variables are the primary theming mechanism
* Resolution still happens at computed-value time

<details>
<summary>Examples</summary>

```css
:root {
  --brand-color: blue;
}

my-card {
  --brand-color: green;
}
```

```css
/* inside shadow DOM */
.title {
  color: var(--brand-color);
}
```

</details>

---

## Style Isolation Guarantees

> Shadow DOM provides **strong isolation**, unlike CSS Modules or BEM.

Isolated by default:

* Selectors
* Specificity wars
* Accidental overrides

Not isolated:

* Fonts
* Custom properties
* UA styles

---

## Encapsulation Patterns

> Real-world component systems rely on layered APIs.

Common patterns:

* Internal styles → hard-coded
* Theme tokens → CSS variables
* Surface hooks → `::part`

Anti-patterns:

* Styling via deep selectors
* Excessive `:host-context`
* Re-exporting internals via parts

---

## Notes

* Shadow DOM is a **hard boundary**, not a naming convention
* Variables are the sanctioned escape hatch
* `::part` is safer than selector piercing
* Encapsulation improves long-term maintainability

## Caveats

* Overexposing parts weakens encapsulation
* Variables can create implicit coupling
* Shadow DOM does not fix bad semantics
* Tooling support varies across browsers
