---

id: css-box-model-layout-primitives
title: Box Model & Layout Primitives
sidebar_position: 8
-------------------

# Box Model & Layout Primitives

> Core primitives that define box sizing, spacing, coordinate systems, and writing-direction semantics. These concepts underpin all layout algorithms and determine how dimensions, offsets, and flow directions are interpreted by the rendering engine.

---

## Content-box vs Border-box

> Defines how width and height are resolved by determining which parts of the box model are included in size calculations. This choice directly affects intrinsic sizing, overflow behavior, and layout stability.

<details>
<summary>Examples</summary>

```css
.box {
  box-sizing: content-box; /* default
                              width/height apply to content only
                              padding + border add to final size */
  width: 200px;
  padding: 20px;
  border: 10px solid;
}

.alt {
  box-sizing: border-box;  /* width/height include content + padding + border
                              preferred for predictable layouts */
  width: 200px;
  padding: 20px;
  border: 10px solid;
}

/* Notes:
   - box-sizing affects intrinsic size contributions
   - border-box stabilizes flex/grid sizing
*/
```

</details>

---

## Margin Collapsing

> Vertical margin resolution mechanism for block-level boxes in normal flow. Margins may collapse between adjacent boxes, parent/child, or empty blocks, producing a single resolved margin.

<details>
<summary>Examples</summary>

```css
.parent {
  margin-top: 40px;
}

.child {
  margin-top: 20px;
}

/* Result:
   - Margins collapse to max(40px, 20px) => 40px
*/

.no-collapse {
  display: flow-root; /* or overflow: hidden | auto
                         creates BFC, prevents collapse */
}

.empty {
  margin: 30px 0;    /* empty block margins can collapse with siblings */
}

/* Collapse rules:
   - Only vertical margins
   - Only normal-flow block boxes
   - No collapse across BFC, padding, border, or inline content
*/
```

</details>

---

## Padding & Borders

> Padding expands the content box inward, borders define the box edge, and both participate in box sizing, painting, and hit-testing. Neither collapses or overlaps.

<details>
<summary>Examples</summary>

```css
.box {
  padding: 1rem;          /* inner spacing, affects scroll area */
  border: 2px solid red;  /* visual boundary, part of border box */
}

/* Characteristics:
   - Padding contributes to background painting
   - Borders clip background unless background-clip adjusted
   - Neither collapses or overlaps adjacent boxes
*/
```

</details>

---

## Containing Block Formation

> Defines the coordinate system used to resolve percentages, positioning offsets, and size calculations for descendant boxes. Different layout modes create different containing blocks.

<details>
<summary>Examples</summary>

```css
.relative {
  position: relative; /* establishes containing block for abs children */
}

.absolute {
  position: absolute;
  top: 10%;           /* resolved against padding box of containing block */
  left: 20px;
}

.fixed {
  position: fixed;    /* containing block is viewport or transformed ancestor */
}

.transform {
  transform: translateZ(0); /* creates containing block for fixed descendants */
}

/* Rules:
   - position:absolute → nearest positioned ancestor
   - position:fixed → viewport unless transformed ancestor exists
   - percentages resolve against containing block dimensions
*/
```

</details>

---

## Writing Modes

> Determines the inline and block flow directions, affecting layout axes, box orientation, margin directions, and text flow. Writing modes redefine what "width" and "height" mean.

<details>
<summary>Examples</summary>

```css
.vertical {
  writing-mode: vertical-rl; /* horizontal-tb | vertical-rl | vertical-lr */
}

/* Effects:
   - Inline axis becomes vertical
   - Block axis becomes horizontal
   - Affects margin, padding, logical properties
*/
```

</details>

---

## Logical Properties

> Direction-agnostic properties that map to physical sides based on writing mode and text direction. Essential for internationalized and adaptive layouts.

<details>
<summary>Examples</summary>

```css
.box {
  margin-block-start: 1rem; /* maps to top/bottom depending on writing mode */
  margin-inline-end: 2rem;  /* maps to left/right based on direction */

  padding-inline: 1ch;      /* inline-start + inline-end */
  border-block: 2px solid;  /* block-start + block-end */
}

/* Logical vs physical:
   - margin-top != margin-block-start
   - width != inline-size
   - height != block-size
*/
```

</details>

---

## Notes

* Box model primitives apply before layout algorithms
* Containing blocks control percentage and position resolution
* Writing modes redefine axes globally
* Logical properties prevent direction-specific hacks

## Caveats

* Margin collapsing is often misunderstood and context-dependent
* Transforms silently change containing block behavior
* Mixing physical and logical properties can cause bugs
