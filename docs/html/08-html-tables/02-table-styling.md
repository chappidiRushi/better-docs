---
sidebar_position: 2
---

# 8.2 Table Styling

## Definitions

- **Border**: The line surrounding the table and its cells.
- **Padding**: The space between the cell content and its borders.
- **Spacing**: The space between the cells themselves.
- **Alignment**: How the text is positioned horizontally (left, center, right) or vertically (top, middle, bottom) within a cell.
- **Responsive Table**: A table that adapts its layout or adds scrollbars on smaller screens to prevent breaking the page layout.

## Beginner Level Introduction

An HTML table without styling looks terrible. The data is crunched together, and there are no lines separating the rows and columns. 

In older versions of HTML, attributes like `border="1"`, `cellpadding="5"`, and `bgcolor="red"` were used directly on the `<table>` tag. **This is now obsolete.** You should always use CSS to style your tables.

```css
/* Basic CSS to make a table readable */
table, th, td {
  border: 1px solid black;
}
```

By default, applying a border to the table, `<th>`, and `<td>` creates a "double border" effect, because the table has its own border, and each cell has its own border.

## Deep Dive

### Borders and Border Collapse

To remove the double border effect and create a clean, single-line grid, use the `border-collapse` CSS property.

```css
table {
  border-collapse: collapse;
}
```

### Sizes, Padding, and Spacing

**Width:** You can set the width of the entire table to `100%` so it expands to fill its container.
**Padding:** Cell padding makes the table easier to read by adding whitespace inside the cells.
**Spacing:** If you *don't* use `border-collapse`, you can adjust the space between cells using `border-spacing`.

```css
th, td {
  padding: 15px;
}
```

### Alignment

By default, `<th>` text is centered and `<td>` text is left-aligned. You can change this using `text-align`.

```css
td {
  text-align: center; /* or left, right */
}
```

### Colors and Zebra Striping

Adding a background color to the header makes it stand out.
For large tables, "zebra striping" (alternating row colors) vastly improves readability. This is done using the `:nth-child()` CSS pseudo-class.

```css
/* Style the header */
th {
  background-color: #04AA6D;
  color: white;
}

/* Zebra stripe the rows */
tr:nth-child(even) {
  background-color: #f2f2f2;
}
```

### Responsive Tables

Tables are notoriously difficult to handle on mobile devices because they cannot easily "wrap" like text. If a table has 10 columns, it will overflow the screen.

The easiest and most common solution is to wrap the table in a container `<div>` with `overflow-x: auto;`. This creates a horizontal scrollbar *only* for the table, leaving the rest of the page layout intact.

## Examples

<details>
<summary><strong>Example: A Beautifully Styled CSS Table</strong></summary>

```html
<!-- CSS Styling -->
<style>
.styled-table {
    border-collapse: collapse;
    margin: 25px 0;
    font-size: 0.9em;
    font-family: sans-serif;
    min-width: 400px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
}

.styled-table thead tr {
    background-color: #009879;
    color: #ffffff;
    text-align: left;
}

.styled-table th,
.styled-table td {
    padding: 12px 15px;
}

.styled-table tbody tr {
    border-bottom: 1px solid #dddddd;
}

.styled-table tbody tr:nth-of-type(even) {
    background-color: #f3f3f3;
}

.styled-table tbody tr:last-of-type {
    border-bottom: 2px solid #009879;
}
</style>

<!-- HTML Structure -->
<table class="styled-table">
    <thead>
        <tr>
            <th>Name</th>
            <th>Points</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Dom</td>
            <td>6000</td>
        </tr>
        <tr>
            <td>Melissa</td>
            <td>5150</td>
        </tr>
    </tbody>
</table>
```

</details>

<details>
<summary><strong>Example: Responsive Table Wrapper</strong></summary>

```html
<!-- 
  The wrapper div is given overflow-x: auto.
  If the table is wider than the screen, a scrollbar appears HERE,
  preventing the entire webpage from scrolling horizontally.
-->
<div style="overflow-x: auto;">
  <table style="width: 1000px;"> <!-- Artificially wide table -->
    <tr>
      <th>Header 1</th>
      <th>Header 2</th>
      <th>Header 3</th>
      <th>Header 4</th>
      <th>Header 5</th>
    </tr>
    <tr>
      <td>Data</td>
      <td>Data</td>
      <td>Data</td>
      <td>Data</td>
      <td>Data</td>
    </tr>
  </table>
</div>
```

</details>
