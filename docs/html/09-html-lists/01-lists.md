---
sidebar_position: 1
---

# 9.1 HTML Lists

## Definitions

- **Unordered List (`<ul>`)**: A collection of items that do not have a numerical or alphabetical sequence. Typically rendered with bullet points.
- **Ordered List (`<ol>`)**: A collection of items that follow a specific sequence. Typically rendered with numbers or letters.
- **List Item (`<li>`)**: An individual entry within an ordered or unordered list.
- **Description List (`<dl>`)**: A list of terms, with a description of each term.

## Beginner Level Introduction

Lists are used everywhere on the web, from simple grocery lists to complex navigation menus. HTML offers three primary types of lists: Unordered, Ordered, and Description lists.

### Unordered Lists (`<ul>`)

An unordered list starts with the `<ul>` tag. Each list item starts with the `<li>` tag. 
By default, the list items will be marked with bullets (small black circles).

```html
<ul>
  <li>Coffee</li>
  <li>Tea</li>
  <li>Milk</li>
</ul>
```

### Ordered Lists (`<ol>`)

An ordered list starts with the `<ol>` tag. Each list item starts with the `<li>` tag.
By default, the list items will be marked with numbers (1, 2, 3...).

```html
<ol>
  <li>First step</li>
  <li>Second step</li>
  <li>Third step</li>
</ol>
```

## Deep Dive

### The `type` Attribute for Ordered Lists

You can change the numbering style of an `<ol>` using the `type` attribute.

- `type="1"`: Numbers (1, 2, 3...) - Default
- `type="A"`: Uppercase letters (A, B, C...)
- `type="a"`: Lowercase letters (a, b, c...)
- `type="I"`: Uppercase Roman numerals (I, II, III...)
- `type="i"`: Lowercase Roman numerals (i, ii, iii...)

### The `start` and `reversed` Attributes

Sometimes you want an ordered list to start counting from a specific number. You can use the `start` attribute.

```html
<ol start="50">
  <li>Item 50</li>
  <li>Item 51</li>
</ol>
```

If you want a countdown list, use the boolean `reversed` attribute.

```html
<ol reversed>
  <li>Blastoff! (3)</li>
  <li>Engines ready (2)</li>
  <li>All systems go (1)</li>
</ol>
```

### Description Lists (`<dl>`)

A description list is a list of terms, with a description of each term. 
- The `<dl>` tag defines the description list.
- The `<dt>` tag defines the term (name).
- The `<dd>` tag describes each term.

```html
<dl>
  <dt>Coffee</dt>
  <dd>- black hot drink</dd>
  <dt>Milk</dt>
  <dd>- white cold drink</dd>
</dl>
```
Browsers usually indent `<dd>` elements to visually separate the description from the term.

## Examples

<details>
<summary><strong>Example: Standard Ordered and Unordered Lists</strong></summary>

```html
<h2>My Shopping List (Unordered)</h2>
<ul>
  <li>Apples</li>
  <li>Bananas</li>
  <li>Bread</li>
</ul>

<h2>Recipe Steps (Ordered)</h2>
<!-- 
  Special Attribute: type 
  - Changes the list marker to uppercase letters.
-->
<ol type="A">
  <li>Preheat oven to 350 degrees.</li>
  <li>Mix the dry ingredients.</li>
  <li>Bake for 30 minutes.</li>
</ol>
```

</details>

<details>
<summary><strong>Example: A Glossary using a Description List</strong></summary>

```html
<h2>Web Glossary</h2>
<!-- The list container -->
<dl>
  <!-- The term -->
  <dt>HTML</dt>
  <!-- The description -->
  <dd>HyperText Markup Language, used for structuring web pages.</dd>
  
  <dt>CSS</dt>
  <dd>Cascading Style Sheets, used for styling web pages.</dd>
  
  <dt>JS</dt>
  <dd>JavaScript, used for adding interactivity to web pages.</dd>
</dl>
```

</details>
