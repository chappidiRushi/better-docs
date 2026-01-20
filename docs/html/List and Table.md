---

id: lists-tables

title: Lists & Tables

sidebar_position: 5
--------------


# Lists & Tables

> Structured grouping of related items and tabular data in HTML

---

## Ordered Lists (`<ol>`)

```txt
Sequential list where order has semantic meaning
```

```html
<ol>
  <li>Item</li>
</ol>
```

<details>
<summary>Examples</summary>

```html
<ol start="3" reversed>
  <li>Third</li>
  <li>Second</li>
</ol>
```

```html
<ol type="1"> <!-- 1 | a | A | i | I -->
  <li>Step</li>
</ol>
```

```txt
Attributes:
- start: number
- reversed: boolean
- type: 1 | a | A | i | I
```

</details>

---

## Unordered Lists (`<ul>`)

```txt
List of items where order is not semantically relevant
```

```html
<ul>
  <li>Item</li>
</ul>
```

<details>
<summary>Examples</summary>

```html
<ul>
  <li>Apple</li>
  <li>Orange</li>
</ul>
```

```html
<ul>
  <li>Parent
    <ul>
      <li>Child</li>
    </ul>
  </li>
</ul>
```

```txt
Notes:
- Marker styling is CSS-only
- Nesting conveys hierarchy
```

</details>

---

## Description Lists (`<dl>`)

```txt
Groups of terms and their descriptions
```

```html
<dl>
  <dt>Term</dt>
  <dd>Description</dd>
</dl>
```

<details>
<summary>Examples</summary>

```html
<dl>
  <dt>HTML</dt>
  <dd>Markup language</dd>
  <dt>CSS</dt>
  <dd>Styling language</dd>
</dl>
```

```html
<dl>
  <dt>Term</dt>
  <dd>Desc 1</dd>
  <dd>Desc 2</dd>
</dl>
```

```txt
Rules:
- Multiple <dt> or <dd> allowed
- Not for key-value tables
```

</details>

---

## Tables (`<table>`)

```txt
Two-dimensional data representation with rows and columns
```

```html
<table>
  <tr>
    <td>Cell</td>
  </tr>
</table>
```

<details>
<summary>Examples</summary>

```html
<table>
  <caption>Stats</caption>
  <thead>
    <tr>
      <th scope="col">Name</th>
      <th scope="col">Score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">A</th>
      <td>10</td>
    </tr>
  </tbody>
</table>
```

```html
<table>
  <colgroup>
    <col span="2">
  </colgroup>
</table>
```

```txt
Elements:
- caption (optional)
- thead | tbody | tfoot
- tr | th | td
```

</details>

---

## Notes

* Lists convey semantics beyond styling
* Tables are for data, not layout
* th defines headers for accessibility

## Caveats

* Avoid tables for page layout
* Missing scopes reduce accessibility
* Lists auto-close on invalid nesting
