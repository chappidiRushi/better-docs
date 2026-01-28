---

id: css-cascade-specificity-layers
title: Cascade, Specificity & Layers
sidebar_position: 6
-------------------

# Cascade, Specificity & Layers

> How competing CSS declarations are ordered, compared, and finally chosen by the browser. The cascade defines a deterministic conflict‑resolution algorithm combining origin, importance, layers, specificity, and source order.

---

## Cascade Origins

> Source categories that define where a declaration comes from and its base priority. Origins form the first major axis of the cascade and establish default trust levels between browser, user, and author styles.

<details>
<summary>Examples</summary>

```css
/* Origin priority (low → high):
   1. User agent (browser defaults)
   2. User styles
   3. Author styles
*/

/* User agent */
button { display: inline-block; }

/* Author stylesheet */
button { display: flex; }

/* User stylesheet (e.g. accessibility) */
button { display: block; }
```

</details>

---

## Importance Rules

> `!important` alters normal cascade order within and across origins. Importance creates a parallel cascade path that inverts parts of the origin hierarchy to allow explicit overrides.

<details>
<summary>Examples</summary>

```css
/* Normal author rule */
.btn { color: blue; }

/* Important author rule */
.btn { color: red !important; }

/* Important order (low → high):
   UA !important
   User !important
   Author !important
*/

/* User important overrides author important */
```

</details>

---

## Specificity Resolution

> Numeric comparison used when declarations share origin and importance. Specificity is calculated per selector and compared lexicographically, not summed across selector lists.

<details>
<summary>Examples</summary>

```css
/* (a, b, c) = (IDs, classes/attrs/pseudo-classes, types) */

#app {}              /* (1,0,0) */
.card {}             /* (0,1,0) */
section {}           /* (0,0,1) */

#app .card {}        /* (1,1,0) */
section.card {}      /* (0,1,1) */

/* Inline styles */
/* specificity higher than any selector */
```

</details>

---

## Specificity Edge Cases

> Non-obvious specificity behavior defined by the Selectors spec. Functional pseudo-classes may contribute zero, maximum, or argument-derived specificity depending on their definition.

<details>
<summary>Examples</summary>

```css
/* :where() contributes zero */
:where(#id .x) {}        /* (0,0,0) */

/* :is() takes max argument */
:is(#id, .x) {}          /* (1,0,0) */

/* :not() uses argument specificity */
:not(.active) {}         /* (0,1,0) */

/* Universal selector */
* {}                     /* (0,0,0) */
```

</details>

---

## Inline Styles

> Declarations applied directly on elements via the `style` attribute. Inline styles participate in the cascade as author origin with effectively highest specificity but still obey importance rules.

<details>
<summary>Examples</summary>

```html
<div style="color: red; padding: 10px"></div>
```

```css
/* Inline styles:
   - Override all author styles
   - Can be overridden only by !important user rules
*/
```

</details>

---

## Source Order

> Tie-breaker when all other cascade factors are equal. Source order is evaluated after origin, importance, layers, and specificity have been resolved.

<details>
<summary>Examples</summary>

```css
.box { color: red; }
.box { color: blue; } /* wins (later in source) */

@import "a.css";
@import "b.css";     /* b.css loaded later */
```

</details>

---

## @layer Ordering

> Explicit cascade grouping that sits above specificity and source order. Layers introduce an author-controlled ordering mechanism that precedes selector comparison.

<details>
<summary>Examples</summary>

```css
@layer reset, base, components, utilities;

@layer reset {
  button { all: unset; }
}

@layer components {
  button { padding: 8px; }
}

@layer utilities {
  .p-4 { padding: 1rem; }
}

/* Later layers override earlier layers */
```

</details>

---

## Layered Resets & Design Systems

> Using layers to control large-scale style precedence predictably. Enables scalable architectures where resets, base styles, components, and utilities coexist without specificity escalation.

<details>
<summary>Examples</summary>

```css
@layer reset {
  * { box-sizing: border-box; }
}

@layer base {
  body { font-family: system-ui; }
}

@layer components {
  .card { padding: 1rem; }
}

@layer utilities {
  .mt-4 { margin-top: 1rem; }
}

/* Utilities always win without !important */
```

</details>

---

## Cascade Resolution Algorithm

> Full decision pipeline used by the browser to pick a winning declaration. The algorithm is stable, deterministic, and evaluated independently for each property on each element.

<details>
<summary>Examples</summary>

```text
1. Filter by matching selectors
2. Sort by origin
3. Sort by importance
4. Sort by layer order
5. Compare specificity
6. Apply source order
```

</details>

---

## Notes

* Cascade happens **after selector matching**
* Layers override specificity, not the opposite
* Inline styles bypass selector specificity
* `!important` changes origin ordering

## Caveats

* Overusing `!important` breaks layer design
* Imported styles inherit the layer they are imported into
* Shadow DOM has its own cascade scope
