---
id: css-custom-properties
title: CSS Variables
sidebar_position: 22
---

# CSS Variables (Custom Properties)

> Custom properties are **late-bound variables** resolved during the cascade and computed-value stage, enabling dynamic theming, composition, and runtime value substitution.

Progression: **mental model → resolution timing → architecture & performance**.

---

## What Custom Properties Are (Foundations)

> Custom properties store **raw token streams**, not typed CSS values.

Key properties:

* Declared with `--*`
* Accessed via `var()`
* Inherit by default
* Not validated until used

```css
/* Syntax Blueprint
--custom-name: <declaration-value>;
var(--custom-name, <fallback>?)
*/
```

<details>
<summary>Examples</summary>

```css
:root {
  --brand-color: oklch(65% 0.2 230);
}

.button {
  color: var(--brand-color);
}

/* Note:
- Value is not parsed until var() is substituted
*/
```

</details>

---

## Resolution Timing (Critical Concept)

> Custom properties resolve **after cascade**, during computed value resolution.

Resolution order:

1. Cascade & inheritance
2. `var()` substitution
3. Property value parsing
4. Computed value resolution

Implications:

* Variables can reference other variables
* Circular references invalidate the property
* Media/container queries do NOT re-evaluate tokens

<details>
<summary>Examples</summary>

```css
:root {
  --size: 10px;
  --double: calc(var(--size) * 2);
}

.box {
  width: var(--double); /* resolves after cascade */
}
```

</details>

---

## Fallback Chains & Invalid Values

> Fallbacks activate only if substitution fails.

```css
/* Value Blueprint
var(--a, var(--b, <fallback>))
*/
```

Rules:

* Missing variable → fallback used
* Invalid at computed-value time → fallback used
* Cycles → entire declaration invalid

<details>
<summary>Examples</summary>

```css
.box {
  color: var(--primary, var(--secondary, red));
}

/* If --primary missing → try --secondary → else red */
```

</details>

---

## Scope, Cascade & Inheritance

> Custom properties obey **normal cascade rules**.

Scoping rules:

* Scoped to the element they’re declared on
* Inherit by default
* Can be overridden at any level

Interaction with cascade:

* Respect specificity
* Respect `!important`
* Respect `@layer`

<details>
<summary>Examples</summary>

```css
:root {
  --gap: 16px;
}

.card {
  --gap: 8px; /* scoped override */
  padding: var(--gap);
}
```

</details>

---

## Typed vs Untyped Reality

> Custom properties are **untyped** until substitution.

Consequences:

* Can store fragments (`10px solid red`)
* Can break shorthand parsing
* Cannot be partially validated

```css
/* Dangerous but powerful */
.box {
  border: var(--border); /* entire shorthand */
}
```

<details>
<summary>Examples</summary>

```css
:root {
  --border: 2px dashed oklch(60% 0.15 30);
}

.card {
  border: var(--border);
}
```

</details>

---

## Performance Implications

> Custom properties participate in **style recalculation**, not layout by default.

Performance characteristics:

* Changing a variable triggers style recalc on descendants
* No layout unless it affects layout properties
* Cheap compared to JS-driven inline styles

Bad patterns:

* High-frequency mutation on large subtrees
* Using variables for per-frame animation

<details>
<summary>Examples</summary>

```css
/* Good: theming */
:root {
  --bg: white;
}

.dark {
  --bg: black;
}

/* Bad: animation */
.box {
  transition: none;
  width: calc(var(--i) * 1px);
}
```

</details>

---

## Theming Architectures

> Custom properties enable **token-based design systems**.

Common patterns:

* Global tokens (`:root`)
* Contextual overrides (containers)
* Component-level tokens
* Dark/light mode toggles

<details>
<summary>Examples</summary>

```css
:root {
  --color-bg: white;
  --color-text: black;
}

[data-theme="dark"] {
  --color-bg: black;
  --color-text: white;
}

.page {
  background: var(--color-bg);
  color: var(--color-text);
}
```

</details>

---

## Notes

* Custom properties are resolved late
* They store tokens, not values
* Powerful for theming and composition
* Fully participate in cascade & layers

## Caveats

* Cycles invalidate entire declarations
* Overuse can cause large style recalcs
* Debugging requires computed styles
* Not a replacement for preprocessor variables
