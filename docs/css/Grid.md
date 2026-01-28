---

id: css-grid-advanced
title: Grid Layout (Advanced)
sidebar_position: 12
--------------------

# Grid Layout (Advanced)

> Grid defines a two-dimensional formatting context with an explicit track-based sizing algorithm. It separates sizing from placement, resolves rows and columns independently, and supports complex alignment, auto-placement, and subgrid inheritance.

---

## Explicit vs Implicit Grids

> Explicit grids are defined by the author using `grid-template-*`. Implicit grids are generated automatically when items are placed outside the explicit grid.

<details>
<summary>Examples</summary>

```css
.container {
  display: grid;
  grid-template-columns: 200px 1fr; /* explicit columns */
  grid-template-rows: auto 1fr;     /* explicit rows */
}

.item {
  grid-column: 3;                   /* forces implicit column creation */
}

/* Visual (columns):

   Explicit tracks:
   ┌────────200────────┬────1fr────┐
   │        col 1      │   col 2   │
   └───────────────────┴───────────┘

   After placing item in column 3:
   ┌────────200────────┬────1fr────┬─auto─┐
   │ col 1             │ col 2     │ col3 │ ← implicit
   └───────────────────┴───────────┴──────┘

   Notes:
   - Implicit tracks use grid-auto-columns / rows
   - Explicit grid is resolved first
*/
```

</details>

---

## Track Sizing Algorithm

> Multi-phase algorithm that resolves track sizes using min/max sizing functions, intrinsic contributions, and available space.

<details>
<summary>Examples</summary>

```css
.container {
  grid-template-columns: minmax(100px, 1fr) auto 200px;
}

/* Visual sizing phases:

   Step 1: Min track sizes
   ┌──100──┬──auto──┬──200──┐

   Step 2: Intrinsic resolution
   ┌──150──┬──300───┬──200──┐  ← content grows auto track

   Step 3: Free space distribution
   ┌──300──┬──300───┬──200──┐  ← fr absorbs remaining space

   Track types:
   - Fixed: <length>
   - Flexible: fr
   - Intrinsic: auto | min-content | max-content
*/
```

</details>

---

## `fr` Unit Behavior

> `fr` represents a fraction of remaining free space after fixed and intrinsic tracks are resolved.

<details>
<summary>Examples</summary>

```css
.container {
  grid-template-columns: 200px 1fr 2fr;
}

/* Visual:
   Container: 800px
   Fixed: 200px
   Remaining: 600px

   1fr = 200px
   2fr = 400px

   ┌────200────┬──200──┬────400────┐
*/

/* Notes:
   - fr is not percentage
   - fr tracks can collapse if no free space
   - min-width can override fr distribution
*/
```

</details>

---

## Auto-Placement Algorithm

> Places grid items that do not have explicit positions by filling grid cells according to `grid-auto-flow`.

<details>
<summary>Examples</summary>

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-flow: row dense; /* row | column | dense */
}

/* Visual (row auto-flow):

   Initial placement:
   ┌──A──┬──B──┬──C──┐
   ├──D──┼─────┼─────┤
   └─────┴─────┴─────┘

   With dense backfilling:
   ┌──A──┬──B──┬──C──┐
   ├──D──┼──E──┼─────┤
   └─────┴─────┴─────┘

   Notes:
   - dense fills holes
   - may reorder visual layout
*/
```

</details>

---

## Alignment

> Alignment properties control how tracks and items are aligned once sizing and placement are resolved.

<details>
<summary>Examples</summary>

```css
.container {
  justify-items: stretch;      /* start | end | center | stretch */
  align-items: center;

  justify-content: space-between; /* aligns grid tracks */
  align-content: center;
}

.item {
  justify-self: end;
  align-self: start;
}

/* Visual:

justify-items vs justify-content

Container:
┌──────────────────────────┐
│ [□]    [□]    [□]        │ ← justify-content: space-between
└──────────────────────────┘

Item self-alignment:
┌───────┐
│   □   │ ← align-self: start
│       │
└───────┘
*/
```

</details>

---

## Subgrid

> `subgrid` allows a nested grid to inherit track definitions from its parent grid.

<details>
<summary>Examples</summary>

```css
.parent {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;
}

.child {
  display: grid;
  grid-template-columns: subgrid; /* inherits parent columns */
}

/* Visual:

Parent grid:
┌──1fr──┬──2fr──┬──1fr──┐
│       │       │       │
└───────┴───────┴───────┘

Child subgrid aligns exactly:
┌──1fr──┬──2fr──┬──1fr──┐
│  □    │   □   │   □   │
└───────┴───────┴───────┘

Notes:
- Track lines are shared
- No independent sizing
*/
```

</details>

---

## Notes

* Grid is two-dimensional
* Sizing happens before placement
* Auto-placement is deterministic but non-trivial
* `fr` resolves after intrinsic sizing

## Caveats

* Grid is not content-first
* `dense` can reorder visuals
* Subgrid support varies historically
