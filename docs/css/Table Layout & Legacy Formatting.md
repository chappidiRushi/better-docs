---

id: css-table-layout
title: Table Layout & Legacy Formatting
sidebar_position: 13
--------------------

# Table Layout & Legacy Formatting

> CSS tables define a specialized formatting context with a distinct box tree, intrinsic sizing–driven layout algorithms, and legacy behaviors that differ fundamentally from modern layout systems like Flexbox and Grid.

---

## Table Box Tree

> Table layout generates a non-DOM-aligned internal box tree consisting of table, row group, row, column, and cell boxes. Many boxes may be anonymous.

<details>
<summary>Examples</summary>

```css
.table {
  display: table;              /* table formatting context */
}

.tr {
  display: table-row;
}

.td {
  display: table-cell;
}

/* Visual box tree:

DOM:
<div class="table">
  <div class="tr">
    <div class="td"></div>
  </div>
</div>

Generated box tree:

┌─ table box ───────────────────┐
│ ┌─ table-row-group (anon) ──┐ │
│ │ ┌─ table-row ───────────┐ │ │
│ │ │ ┌─ table-cell ──────┐ │ │ │
│ │ │ └───────────────────┘ │ │ │
│ │ └───────────────────────┘ │ │
│ └───────────────────────────┘ │
└────────────────────────────────┘

Notes:
- Column boxes are generated implicitly
- DOM structure ≠ formatting structure
*/
```

</details>

---

## Auto vs Fixed Table Layout

> `table-layout` controls whether column widths are derived from content (auto) or author-defined constraints (fixed).

<details>
<summary>Examples</summary>

````css
table {
  table-layout: auto;  /* default */
  /* fixed | auto */
}

/* AUTO layout:

- Reads all cells
- Computes min/max intrinsic widths
- Distributes space after full analysis
- Layout is content-dependent

┌────content────┬────────content────────┐
│ short          │ very very long text   │
└────────────────┴───────────────────────┘
*/

```css
table {
  table-layout: fixed;
  width: 600px;
}

/* FIXED layout:

- Column widths resolved from:
  1. col / first row widths
  2. remaining space equally
- Ignores later content
- Faster, predictable

┌──200──┬──200──┬──200──┐
│ text  │ text  │ text  │
└───────┴───────┴───────┘
*/
````

</details>

---

## Anonymous Table Boxes

> Missing table elements are automatically generated to satisfy the table box model.

<details>
<summary>Examples</summary>

```css
.cell {
  display: table-cell;
}

/* HTML:
<div class="cell">A</div>
<div class="cell">B</div>

Generated structure:

┌─ table (anon) ───────────────┐
│ ┌─ table-row (anon) ───────┐ │
│ │ ┌─ cell ┐ ┌─ cell ┐     │ │
│ │ └───────┘ └───────┘     │ │
│ └─────────────────────────┘ │
└──────────────────────────────┘

Notes:
- Anonymous boxes affect styling & borders
- Border-collapse depends on these boxes
*/
```

</details>

---

## Intrinsic Sizing Behavior

> Tables are intrinsically sized by default; column widths depend on cell min/max content contributions.

<details>
<summary>Examples</summary>

```css
td {
  white-space: nowrap; /* affects max-content width */
}

/* Sizing process:

1. Compute min-content width per cell
2. Compute max-content width per cell
3. Column min = max(min-content of cells)
4. Column max = max(max-content of cells)
5. Distribute available width

Visual:

min-content:
┌─50─┬─80─┐

max-content:
┌─200──────┬─300────────┐

Final width depends on table width
*/
```

</details>

---

## Tables vs Grid (Key Differences)

> Tables are content-first and constraint-last; Grid is constraint-first and content-second.

<details>
<summary>Examples</summary>

```css
/* TABLE */
table {
  table-layout: auto;
}

/* GRID */
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
}

/* Comparison:

Tables:
- Implicit structure
- Intrinsic sizing
- Row/column coupling
- Layout depends on content

Grid:
- Explicit tracks
- Deterministic sizing
- Independent axes
- Content adapts to grid
*/
```

</details>

---

## Notes

* Tables establish a unique formatting context
* Width resolution is column-based, not item-based
* Anonymous boxes are common and impactful
* Auto layout is expensive but precise

## Caveats

* Tables are not responsive-friendly by default
* Mixing table and non-table children causes anonymous wrappers
* Percentage heights are unreliable without explicit table height
