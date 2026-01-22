---

id: tables-full-spec
title: Tables
sidebar_position: 14
--------------------

# Tables

> Tables represent **two‑dimensional data** with an explicit formatting and accessibility model. They are **not layout tools**.

---

## Unified Example (Complete Table Model)

```html
<table>
  <caption>Quarterly Revenue</caption> <!-- caption: table title, first child only -->

  <!-- Column grouping -->
  <colgroup>
    <col span="1" style="background:#f5f5f5"> <!-- col: applies to columns, no semantics -->
    <col span="2">
  </colgroup>

  <!-- Table header group -->
  <thead>
    <tr>
      <th scope="col">Quarter</th> <!-- th: header cell -->
      <th scope="col">Product A</th> <!-- scope: col | row | colgroup | rowgroup -->
      <th scope="col">Product B</th>
    </tr>
  </thead>

  <!-- Table body group -->
  <tbody>
    <tr>
      <th scope="row">Q1</th>
      <td value="120000">$120k</td> <!-- td: data cell -->
      <td value="90000">$90k</td>
    </tr>
    <tr>
      <th scope="row">Q2</th>
      <td>$150k</td>
      <td>$110k</td>
    </tr>
  </tbody>

  <!-- Table footer group -->
  <tfoot>
    <tr>
      <th scope="row">Total</th>
      <td>$270k</td>
      <td>$200k</td>
    </tr>
  </tfoot>
</table>
```

---

## Notes for Hardcore Developers

### Table Formatting Model

* Browser creates an internal **table grid**, independent of DOM order
* Missing sections (`thead`, `tbody`, `tfoot`) are **implicitly generated**
* `<tfoot>` may appear before `<tbody>` in source but renders last

### Header Associations

* `scope` is sufficient for **simple tables**
* Complex tables require `headers` + `id` mapping

```html
<th id="h-q1">Q1</th>
<td headers="h-q1 h-prod-a">$120k</td>
```

### Accessibility Mapping

* Tables expose row/column relationships to assistive tech
* `<caption>` is announced before table content
* Avoid empty headers; they break association algorithms

### Styling vs Semantics

* CSS can reorder visuals, but **semantic order remains tabular**
* Never use tables for layout; use CSS Grid/Flexbox instead
* `col`/`colgroup` affect presentation only, not accessibility

---

## Common Expert Pitfalls

* Using tables for page layout
* Omitting `<caption>` for data tables
* Relying on visual alignment instead of header associations
* Mixing `scope` and `headers` incorrectly

---

## Mental Model (Interview One-Liner)

> **HTML tables define a semantic data grid with explicit header associations and accessibility mappings; layout is incidental, structure is essential.**
