---
sidebar_position: 3
---

# 8.3 Advanced Tables

## Definitions

- **colspan**: An attribute used to make a cell span across multiple columns.
- **rowspan**: An attribute used to make a cell span across multiple rows.
- **colgroup**: An element used to group columns together for formatting purposes.
- **col**: An element used within a `colgroup` to define specific styles for one or more columns.

## Beginner Level Introduction

Sometimes data doesn't fit perfectly into a simple grid. You might have a title that needs to stretch across the entire top of the table, or a category name that applies to three different rows.

HTML provides the `colspan` and `rowspan` attributes to handle these complex layouts. They allow a single cell (`<th>` or `<td>`) to take up the space of multiple cells.

## Deep Dive

### Colspan (Spanning Columns)

To make a cell span more than one column, use the `colspan` attribute. The value should be the number of columns you want the cell to span.

When you use `colspan="2"`, that cell takes up its own space *and* the space of the cell to its right. Therefore, you must delete one `<td>` from that row to keep the table balanced.

```html
<tr>
  <th colspan="2">Name</th>
  <!-- We deleted a <th> here because the previous one spans 2 columns -->
  <th>Age</th>
</tr>
```

### Rowspan (Spanning Rows)

To make a cell span more than one row, use the `rowspan` attribute. The value should be the number of rows you want the cell to span.

When you use `rowspan="2"`, the cell pushes down into the row below it. Therefore, in the *next* row, you must have one fewer `<td>` element.

### Colgroup and Col

If you want to apply a background color to an entire column, doing it cell-by-cell is tedious. The `<colgroup>` and `<col>` elements solve this.

The `<colgroup>` must be a child of a `<table>` element, after any `<caption>` elements and before any `<thead>`, `<tbody>`, `<tfoot>`, and `<tr>` elements.

```html
<table>
  <colgroup>
    <!-- Styles the first column -->
    <col style="background-color: lightgray;">
    <!-- Styles the next 2 columns -->
    <col span="2" style="background-color: lightblue;">
  </colgroup>
  <!-- Table rows below... -->
</table>
```
Note: You can only apply a very limited set of CSS properties to `<col>` elements (background, border, visibility, and width).

### Accessibility Best Practices for Complex Tables

Complex tables (using rowspan and colspan) are notoriously difficult for screen readers to interpret. 

To help assistive technologies:
1. **Always use `<th>` for headers.**
2. **Use the `scope` attribute** on headers (`scope="row"` or `scope="col"`).
3. **For highly complex tables, use the `id` and `headers` attributes.** You give every `<th>` a unique `id`, and then you add the `headers` attribute to the `<td>` cells, listing the IDs of all headers that apply to that cell.

## Examples

<details>
<summary><strong>Example: Using Colspan</strong></summary>

```html
<table border="1">
  <tr>
    <!-- 
      Special Attribute: colspan
      - This cell spans 2 columns.
    -->
    <th colspan="2">Contact Details</th>
  </tr>
  <tr>
    <td>Email</td>
    <td>Phone</td>
  </tr>
  <tr>
    <td>john@example.com</td>
    <td>555-1234</td>
  </tr>
</table>
```

</details>

<details>
<summary><strong>Example: Using Rowspan</strong></summary>

```html
<table border="1">
  <tr>
    <th>Name:</th>
    <td>Bill Gates</td>
  </tr>
  <tr>
    <!-- 
      Special Attribute: rowspan
      - This header cell spans 2 rows down.
    -->
    <th rowspan="2">Telephone:</th>
    <td>55577854</td>
  </tr>
  <tr>
    <!-- Notice there is only one <td> in this row because the 
         rowspan from the row above pushed into this space. -->
    <td>55577855</td>
  </tr>
</table>
```

</details>

<details>
<summary><strong>Example: Colgroup for Column Styling</strong></summary>

```html
<table border="1">
  <colgroup>
    <!-- 
      Special Attribute: span
      - Applies the style to the first 2 columns.
    -->
    <col span="2" style="background-color:red">
    <col style="background-color:yellow">
  </colgroup>
  <tr>
    <th>ISBN</th>
    <th>Title</th>
    <th>Price</th>
  </tr>
  <tr>
    <td>3476896</td>
    <td>My first HTML</td>
    <td>$53</td>
  </tr>
</table>
```

</details>
