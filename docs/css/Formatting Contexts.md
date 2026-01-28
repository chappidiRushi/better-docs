---

id: css-formatting-contexts
title: Formatting Contexts
sidebar_position: 7
-------------------

# Formatting Contexts

> Formatting contexts define how boxes are laid out, how they interact with siblings, and how margins, floats, positioning, and intrinsic sizing are resolved. They act as layout-scoping mechanisms that determine which layout algorithm applies and where layout effects are allowed to escape.

---

## Block Formatting Context (BFC)

> An independent block-level layout environment that isolates floats, margin-collapsing behavior, and block layout calculations. A BFC establishes a new block formatting root, limiting how descendant boxes affect surrounding layout.

<details>
<summary>Examples</summary>

```css
/* Elements that establish a BFC */
.container {
  overflow: hidden;      /* hidden | auto | scroll */
  display: flow-root;    /* explicit BFC */
}

.float {
  float: left;
}

.box {
  margin-top: 20px;      /* no margin collapse across BFC boundary */
}

/* Effects of BFC:
   - Contains floats
   - Prevents margin collapsing
   - Isolates layout calculations
*/
```

</details>

---

## Inline Formatting Context

> Layout model governing inline-level boxes and text flow within line boxes. It controls line breaking, glyph shaping, baseline alignment, bidi reordering, and how inline and inline-block elements participate in text layout.

<details>
<summary>Examples</summary>

```css
p {
  font-size: 16px;
  line-height: 1.5;
}

span {
  vertical-align: baseline; /* baseline | middle | top | bottom */
}

/* Inline formatting behavior:
   - Line boxes created per line
   - Inline boxes participate in baseline alignment
   - Whitespace and text shaping applied
*/
```

</details>

---

## Flex Formatting Context

> One-dimensional layout context that distributes space along a main axis using the flex layout algorithm. It replaces block and inline flow for its children and resolves sizes via flex factors, min/max constraints, and intrinsic sizing.

<details>
<summary>Examples</summary>

```css
.flex {
  display: flex;                 /* establishes flex formatting context */
  flex-direction: row;           /* row | column */
  justify-content: space-between;
  align-items: center;
}

.item {
  flex: 1 1 auto;                /* grow | shrink | basis */
}

/* Flex context rules:
   - No margin collapsing
   - Flex items establish independent formatting contexts
   - Sizes resolved via flex algorithm
*/
```

</details>

---

## Grid Formatting Context

> Two-dimensional layout context that aligns items into rows and columns using explicit and implicit grid tracks. Grid layout decouples visual placement from source order and resolves sizing before item positioning.

<details>
<summary>Examples</summary>

```css
.grid {
  display: grid;                     /* establishes grid formatting context */
  grid-template-columns: 1fr 2fr;
  grid-auto-rows: minmax(100px, auto);
}

.item {
  grid-column: 1 / span 2;
}

/* Grid context rules:
   - No margin collapsing
   - Tracks resolved before item placement
   - Grid items form new formatting contexts
*/
```

</details>

---

## Table Formatting Context

> Layout context that follows the CSS table model, including anonymous box generation and specialized sizing rules. Table layout uses its own intrinsic sizing algorithm distinct from block and flex layouts.

<details>
<summary>Examples</summary>

```css
table {
  display: table;
  border-collapse: collapse;
}

td {
  display: table-cell;
}

/* Table context behavior:
   - Anonymous table boxes may be generated
   - Table layout algorithm controls column sizing
   - Margins behave differently than block layout
*/
```

</details>

---

## Independent Formatting Contexts

> Contexts that fully isolate layout calculations, painting, and sometimes stacking behavior from the rest of the document. These contexts are often used to contain layout side effects and improve rendering performance.

<details>
<summary>Examples</summary>

```css
/* New independent formatting contexts */
.box {
  contain: layout;          /* layout | paint | size | strict */
}

.abs {
  position: absolute;       /* establishes formatting context for children */
}

.fixed {
  position: fixed;
}

/* Effects:
   - No interaction with outside layout
   - Improves performance via isolation
*/
```

</details>

---

## Other Context-Creating Mechanisms

> Additional CSS features that implicitly create formatting boundaries or specialized layout contexts. These contexts apply domain-specific layout rules that differ from normal block or inline flow.

<details>
<summary>Examples</summary>

```css
/* Multicolumn */
.columns {
  column-count: 3;          /* creates multicol formatting context */
}

/* Ruby layout */
ruby { display: ruby; }

/* SVG formatting context */
svg { display: inline-block; }
```

</details>

---

## Notes

* Formatting contexts define **layout isolation boundaries**
* Many modern layout modes suppress margin collapsing
* New contexts often imply new block formatting roots
* Context creation affects float containment and painting order

## Caveats

* Not all formatting contexts create stacking contexts
* Some contexts are implicit and easy to trigger accidentally
* `contain` can change intrinsic sizing behavior
