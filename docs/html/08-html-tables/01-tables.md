---
sidebar_position: 1
---

# 8.1 Tables

## Definitions

- **Table**: A structured set of data made up of rows and columns (tabular data).
- **Row**: A horizontal series of cells in a table.
- **Column**: A vertical series of cells in a table.
- **Cell**: The intersection of a row and a column, containing the actual data.
- **Header Cell**: A special cell used to label a row or column, typically rendered in bold and centered.
- **Caption**: A title or brief explanation appended to an article, illustration, or table.

## Beginner Level Introduction

### Creating Tables

HTML tables allow web developers to arrange data into rows and columns. They should **only** be used for tabular data (like schedules, financial reports, pricing tiers) and **never** for webpage layout.

To create a table, you use the `<table>` element. 
Inside the table, you define rows using the `<tr>` (table row) element. 
Inside each row, you define data cells using the `<td>` (table data) element.

```html
<table>
  <tr>
    <td>Apple</td>
    <td>$1.00</td>
  </tr>
  <tr>
    <td>Banana</td>
    <td>$0.50</td>
  </tr>
</table>
```
By default, HTML tables have no borders, no padding, and no styling. They will look like plain text squished together until you apply CSS.

## Deep Dive

### Headers (`<th>`)

A table usually needs headers to describe what the data in the columns (or rows) represents. You define header cells using the `<th>` (table header) element instead of `<td>`.

Browsers treat `<th>` elements differently:
- The text is bold.
- The text is horizontally centered.
- Screen readers announce the header text when a user navigates into a corresponding data cell, which is crucial for accessibility.

```html
<tr>
  <th>Fruit</th>
  <th>Price</th>
</tr>
```

### Captions (`<caption>`)

You can add a title to your table using the `<caption>` element. 
The `<caption>` tag must be inserted **immediately** after the `<table>` tag. It is visually centered above the table by default and helps users quickly understand the table's purpose.

```html
<table>
  <caption>Fresh Produce Prices</caption>
  <!-- table rows go here -->
</table>
```

### Table Sections (`<thead>`, `<tbody>`, `<tfoot>`)

For better semantics and to allow for advanced CSS styling and JavaScript manipulation, tables should be divided into logical sections.

- `<thead>`: Groups the header content in a table.
- `<tbody>`: Groups the body content (the main data) in a table.
- `<tfoot>`: Groups the footer content in a table (like totals or summaries).

Using these elements does not affect the visual layout by default, but it allows browsers to enable scrolling of the `<tbody>` independently of the header and footer, or to print the header and footer on every page if a large table spans multiple printed pages.

## Examples

<details>
<summary><strong>Example: A Well-Structured Semantic Table</strong></summary>

```html
<!-- The main table container -->
<table>
  
  <!-- The title of the table -->
  <caption>Employee Salary Report - Q1 2024</caption>

  <!-- The Header Section -->
  <thead>
    <tr>
      <!-- 
        Special Attribute: scope 
        - Helps screen readers understand if this header applies 
          to the "col" (column) or the "row".
      -->
      <th scope="col">Name</th>
      <th scope="col">Department</th>
      <th scope="col">Salary</th>
    </tr>
  </thead>

  <!-- The Main Data Section -->
  <tbody>
    <tr>
      <!-- We can also use <th> for row headers! -->
      <th scope="row">John Doe</th>
      <td>Engineering</td>
      <td>$80,000</td>
    </tr>
    <tr>
      <th scope="row">Jane Smith</th>
      <td>Marketing</td>
      <td>$75,000</td>
    </tr>
  </tbody>

  <!-- The Footer Section (Summaries/Totals) -->
  <tfoot>
    <tr>
      <th scope="row" colspan="2">Total Payroll</th>
      <td>$155,000</td>
    </tr>
  </tfoot>

</table>
```

</details>
