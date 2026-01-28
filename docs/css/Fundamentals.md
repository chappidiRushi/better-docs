---

id: fundamentals
title: Fundamentals
sidebar_position: 3
-------------------

# CSS Language Fundamentals

> Parsing model, rule structure, error recovery, and value resolution

---

## CSS Parsing & Tokenization

> Conversion of raw CSS text into tokens (ident, function, number, delimiter, etc.) and parsed into rule structures. Tokenization is streaming, context‑aware, and independent of the DOM.

<details>
<summary>Examples</summary>

```css
/* Tokens */
.foo { color: red; }
/* ident(.foo) brace ident(color) colon ident(red) semicolon */

/* Function token */
div { transform: translate(10px, 20px); }

/* Number + unit tokens */
p { margin: 1.5rem; }
```

</details>

---

## Rulesets & Declarations

> Structural units where selectors map to one or more property–value declarations. Rules are either qualified rules or at‑rules and form the core of the CSSOM.

<details>
<summary>Examples</summary>

```css
/* Qualified rule */
.button.primary {
  background-color: blue;
  padding: 0.5rem 1rem;
}

/* Declaration breakdown */
/* property: background-color | value: blue */
```

</details>

---

## Error Handling & Invalid CSS

> Browser behavior where invalid declarations are ignored while valid ones continue to apply. Error recovery is local and never aborts stylesheet parsing.

<details>
<summary>Examples</summary>

```css
.box {
  color: red;
  width: 100px;
  invalid-prop: 10;   /* dropped */
  height: 50px;      /* still applied */
}

/* Invalid value */
p { margin: foo; }    /* entire declaration dropped */
```

</details>

---

## Forward‑Compatible Parsing

> CSS design principle that skips unknown or future syntax without breaking parsing. Enables safe feature evolution across browser versions.

<details>
<summary>Examples</summary>

```css
/* Future property ignored by old browsers */
div {
  color: red;
  text-wrap: balance; /* ignored if unsupported */
}

/* Unknown selector */
:has(> img) {
  border: 1px solid red; /* rule dropped if selector unsupported */
}
```

</details>

---

## Shorthand Expansion

> Process where a shorthand property expands into multiple longhand properties at computed‑value time. Missing sub‑values reset to their initial values.

<details>
<summary>Examples</summary>

```css
/* margin shorthand */
div { margin: 10px 20px; }
/* expands to:
   margin-top: 10px;
   margin-right: 20px;
   margin-bottom: 10px;
   margin-left: 20px;
*/

/* background shorthand */
.box {
  background: red url(bg.png) no-repeat center / cover;
}
```

</details>

---

## Inheritance Rules

> Rules that determine which properties inherit computed values from their parent element. Inheritance occurs before cascade and only for inheritable properties.

<details>
<summary>Examples</summary>

```css
/* Inherited */
body { color: #333; font-family: system-ui; }
p { /* color + font-family inherited */ }

/* Not inherited */
.container { border: 1px solid red; }
.child { /* border not inherited */ }

/* Force inheritance */
button {
  font: inherit;        /* keyword: inherit | initial | unset | revert */
}
```

</details>

---

## CSS Value Resolution Stages

> Ordered pipeline transforming specified values into used and actual values. Each stage may depend on inheritance, layout, and formatting context.

<details>
<summary>Examples</summary>

```css
/* Specified value */
div { width: 50%; }

/* Computed value depends on parent */
.container { width: 400px; }
div { width: 50%; } /* computed: 200px */

/* Used value after layout */
img { width: auto; height: auto; }
```

</details>

---

## Initial, Computed, Used, Actual Values

> Different stages of value interpretation during style and layout calculation. Distinguishes author intent from layout‑resolved results.

<details>
<summary>Examples</summary>

```css
/* initial */
div { all: initial; }

/* computed */
p { font-size: 1em; } /* resolved against parent */

/* used */
.box { width: auto; } /* resolved during layout */
```

</details>

---

## At-Rules Parsing

> Special rules controlling CSS behavior, grouping, or conditional application. At‑rules may contain nested rules or act as standalone statements.

<details>
<summary>Examples</summary>

```css
@media (min-width: 768px) {
  .grid { display: grid; }
}

@supports (display: subgrid) {
  .layout { display: grid; grid-template-columns: subgrid; }
}

@layer utilities {
  .hidden { display: none; }
}
```

</details>

---

## CSS Grammar & Value Types

> Formal grammar defining valid value shapes for each property. Grammars are specified using CSS value definition syntax and enforced during parsing.

<details>
<summary>Examples</summary>

```css
/* <length> | <percentage> */
div { width: 100px; }
div { width: 50%; }

/* <color> */
p { color: hsl(120 50% 50% / 0.8); }

/* keyword | function */
.box { display: grid; }
.box { transform: rotate(45deg); }
```

</details>

---

## Notes

* Parsing is **error‑tolerant by design**
* Unknown declarations are ignored, not fatal
* Shorthands override all related longhands
* Inheritance happens **before** cascade resolution

## Caveats

* Invalid selectors drop the **entire rule**, not just a declaration
* Shorthand resets unspecified sub‑properties
* `inherit` differs from `unset` on non‑inherited properties
