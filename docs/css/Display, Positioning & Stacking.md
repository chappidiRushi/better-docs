---

id: css-display-positioning-stacking
title: Display, Positioning & Stacking
sidebar_position: 9
-------------------

# Display, Positioning & Stacking

> Defines how elements generate boxes, participate in layout, establish containing and stacking contexts, and are *ultimately* painted to the screen. These mechanisms bridge layout, compositing, and rendering order.

---

## Display: External vs Internal

> `display` controls both how an element participates in its parent formatting context (external) and how it lays out its children (internal). It is a box-generation and layout-selection primitive.

```css
.item {
  display: block;        /* block | inline | inline-block | list-item | run-in */
}

.flex {
  display: flex;         /* flex | inline-flex */
}

.grid {
  display: grid;         /* grid | inline-grid */
}

.flow-root {
  display: flow-root;    /* creates new BFC explicitly */
}

.table {
  display: table;       /* table | inline-table
                            table-row | table-cell | table-caption */
}

.none {
  display: none;         /* no box generated */
}

.contents {
  display: contents;     /* box removed, children promoted */
}

/* External values: block | inline | run-in
   Internal values: flow | flow-root | flex | grid | table | ruby
*/
```

---

## Positioning Schemes

> Determines how boxes are placed relative to normal flow and which coordinate system they use. Positioning affects layout participation, containing block selection, and stacking behavior.

```css
.item {
  display: block;        /* block | inline | inline-block | list-item | run-in */
}

.flex {
  display: flex;         /* flex | inline-flex */
}

.grid {
  display: grid;         /* grid | inline-grid */
}

.flow-root {
  display: flow-root;    /* creates new BFC explicitly */
}

.table {
  display: table;       /* table | inline-table
                            table-row | table-cell | table-caption */
}

.none {
  display: none;         /* no box generated */
}

.contents {
  display: contents;     /* box removed, children promoted */
}

/* External values: block | inline | run-in
   Internal values: flow | flow-root | flex | grid | table | ruby
*/
```

---

## Containing Blocks by Position

> Positioning scheme determines which ancestor establishes the containing block used for offset, percentage, and size resolution.

```css
.item {
  display: block;        /* block | inline | inline-block | list-item | run-in */
}

.flex {
  display: flex;         /* flex | inline-flex */
}

.grid {
  display: grid;         /* grid | inline-grid */
}

.flow-root {
  display: flow-root;    /* creates new BFC explicitly */
}

.table {
  display: table;       /* table | inline-table
                            table-row | table-cell | table-caption */
}

.none {
  display: none;         /* no box generated */
}

.contents {
  display: contents;     /* box removed, children promoted */
}

/* External values: block | inline | run-in
   Internal values: flow | flow-root | flex | grid | table | ruby
*/
```

---

## Stacking Contexts

> A stacking context defines an isolated z-ordering space. Elements inside cannot interleave with elements outside, regardless of z-index values.

```css
.item {
  display: block;        /* block | inline | inline-block | list-item | run-in */
}

.flex {
  display: flex;         /* flex | inline-flex */
}

.grid {
  display: grid;         /* grid | inline-grid */
}

.flow-root {
  display: flow-root;    /* creates new BFC explicitly */
}

.table {
  display: table;       /* table | inline-table
                            table-row | table-cell | table-caption */
}

.none {
  display: none;         /* no box generated */
}

.contents {
  display: contents;     /* box removed, children promoted */
}

/* External values: block | inline | run-in
   Internal values: flow | flow-root | flex | grid | table | ruby
*/
```

---

## Painting Order

> Defines the order in which boxes, backgrounds, borders, and descendants are drawn within a stacking context.

```css
.item {
  display: block;        /* block | inline | inline-block | list-item | run-in */
}

.flex {
  display: flex;         /* flex | inline-flex */
}

.grid {
  display: grid;         /* grid | inline-grid */
}

.flow-root {
  display: flow-root;    /* creates new BFC explicitly */
}

.table {
  display: table;       /* table | inline-table
                            table-row | table-cell | table-caption */
}

.none {
  display: none;         /* no box generated */
}

.contents {
  display: contents;     /* box removed, children promoted */
}

/* External values: block | inline | run-in
   Internal values: flow | flow-root | flex | grid | table | ruby
*/
```

---

## z-index Resolution

> Controls stacking order of positioned elements within the same stacking context. Only applies to positioned or flex/grid items.

```css
.item {
  display: block;        /* block | inline | inline-block | list-item | run-in */
}

.flex {
  display: flex;         /* flex | inline-flex */
}

.grid {
  display: grid;         /* grid | inline-grid */
}

.flow-root {
  display: flow-root;    /* creates new BFC explicitly */
}

.table {
  display: table;       /* table | inline-table
                            table-row | table-cell | table-caption */
}

.none {
  display: none;         /* no box generated */
}

.contents {
  display: contents;     /* box removed, children promoted */
}

/* External values: block | inline | run-in
   Internal values: flow | flow-root | flex | grid | table | ruby
*/
```

---

## Notes

* Display selects layout algorithm
* Positioning controls flow participation
* Containing blocks resolve geometry
* Stacking contexts isolate z-order

## Caveats

* `z-index` bugs are usually stacking-context bugs
* `display: contents` affects accessibility & hit-testing
* Transforms change both stacking and containing behavior
