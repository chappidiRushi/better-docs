---

id: css-selectors-deep-dive
title: Selectors
sidebar_position: 5
-------------------

# Selectors (Deep Dive)

> Selector syntax, specificity, and the internal matching model used by browser engines.

---

## Simple & Compound Selectors

> Building blocks of selectors; compound selectors are conjunctions applied to a single element.

<details>
<summary>Examples</summary>

```css
/* Simple selectors */
div {}              /* type */
#app {}             /* ID */
.button {}          /* class */
[disabled] {}       /* attribute */
:hover {}           /* pseudo-class */

/* Compound selector (AND) */
button.primary.large[disabled]:hover {}
/* matches ONE element satisfying ALL conditions */

/* Complex selector (with combinators) */
nav > ul li a {}
```

</details>

---

## Attribute Selectors

> Match elements based on attribute presence or value using defined operators.

<details>
<summary>Examples</summary>

```css
/* Presence */
[hidden] {}

/* Exact match */
[type="text"] {}

/* Whitespace-separated list */
[class~="active"] {}   /* space-separated tokens */

/* Hyphen-separated (lang) */
[lang|="en"] {}        /* en, en-US */

/* Prefix / suffix / substring */
[href^="https"] {}     /* starts with */
[href$=".pdf"] {}      /* ends with */
[href*="docs"] {}      /* contains */

/* Case sensitivity */
[data-id="AbC" i] {}   /* i | s */
```

</details>

---

## Pseudo-Classes

> Dynamic or structural state selectors evaluated at match time.

<details>
<summary>Examples</summary>

```css
/* State */
button:disabled {}
input:focus-visible {}

/* Structural */
li:first-child {}
li:nth-child(2n + 1) {}

/* Tree-relative */
:root {}
:empty {}

/* Logical */
:not(.active) {}
:has(img) {}           /* relational */
```

</details>

---

## Pseudo-Elements

> Target abstract sub-parts of elements, generated during layout.

<details>
<summary>Examples</summary>

```css
::before {}
::after {}
::first-letter {}
::first-line {}
::selection {}

/* Tree-abiding */
li::marker {}

/* Custom highlight */
::highlight(search) {}
```

</details>

---

## :is() / :where() / :has()

> Functional pseudo-classes for grouping, scoping, and relational matching.

<details>
<summary>Examples</summary>

```css
/* :is() — max specificity of arguments */
:is(h1, h2, h3).title {}

/* :where() — zero specificity */
:where(header, footer) a {}

/* :has() — relational, parent matching */
.card:has(img) {}

/* Nested usage */
section:is(.dark, .light):has(h2) {}
```

</details>

---

## Specificity Math

> Weighting system used to resolve conflicts between competing selectors.

<details>
<summary>Examples</summary>

```css
/* (a, b, c) = (IDs, classes/attrs/pseudo-classes, types) */

#id {}                /* (1,0,0) */
.button {}            /* (0,1,0) */
button {}             /* (0,0,1) */

button.primary {}     /* (0,1,1) */
#app .btn:hover {}    /* (1,2,0) */

/* :where() adds zero */
:where(#id .x) {}     /* (0,0,0) */

/* :is() adopts max */
:is(#id, .x) {}       /* (1,0,0) */
```

</details>

---

## Selector Matching Internals

> How engines evaluate selectors efficiently against the DOM.

<details>
<summary>Examples</summary>

```css
/* Matching is right-to-left */
nav ul li a.active {}
/* engine starts at `a.active`, then walks ancestors */

/* Fast rejection */
button.primary {}
/* tag + class narrows candidate set early */

/* :has() introduces upward dependency */
article:has(img) {}
/* requires subtree evaluation, impacts invalidation */
```

</details>

---

## Combinators

> Define relationships between elements in a selector sequence.

<details>
<summary>Examples</summary>

```css
A B {}     /* descendant */
A > B {}   /* child */
A + B {}   /* adjacent sibling */
A ~ B {}   /* general sibling */

/* Column combinator (tables) */
col || td {}
```

</details>

---

## Universal & Namespace Selectors

> Match any element, optionally constrained by namespace.

<details>
<summary>Examples</summary>

```css
* {}                /* universal */
svg|* {}            /* any SVG element */
*|div {}            /* div in any namespace */
|div {}             /* no-namespace div */
```

</details>

---

## Structural Variants

> Position-based selectors with element-type awareness.

<details>
<summary>Examples</summary>

```css
li:nth-child(odd) {}
li:nth-of-type(odd) {}

/* Difference */
/* nth-child counts all siblings */
/* nth-of-type counts same-type siblings */
```

</details>

---

## Forgiving Selector Lists

> Selector lists that ignore invalid selectors instead of dropping the rule.

<details>
<summary>Examples</summary>

```css
:is(.valid, :unknown, .also-valid) {}
:where(.a, .b, :bad) {}

/* Normal selector list would invalidate entire rule */
```

</details>

---

## UI & Input Pseudo-Classes

> User interaction and form control state selectors.

<details>
<summary>Examples</summary>

```css
:enabled {}
:disabled {}
:checked {}
:indeterminate {}
:required {}
:optional {}
:valid {}
:invalid {}
:in-range {}
:out-of-range {}
```

</details>

---

## Tree-Scoped & Document State Selectors

> Selectors dependent on document state or navigation.

<details>
<summary>Examples</summary>

```css
:target {}        /* URL fragment */
:scope {}         /* scoping root */
:defined {}       /* custom elements */
```

</details>

---

## Shadow DOM Selectors

> Selectors participating in shadow tree scoping.

<details>
<summary>Examples</summary>

```css
:host {}
:host(.active) {}
:host-context(.dark) {}

::slotted(img) {}
```

</details>

---

## Notes

* Compound selectors are evaluated as logical AND
* Selector lists are logical OR
* Matching is selector-driven, not rule-driven
* Specificity is calculated per selector, not per rule

## Caveats

* `:has()` affects style invalidation and performance
* Pseudo-elements do not participate in DOM matching
* Invalid selectors drop the entire rule
