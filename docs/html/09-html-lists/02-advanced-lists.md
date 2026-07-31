---
sidebar_position: 2
---

# 9.2 Advanced Lists

## Definitions

- **Nested List**: A list (unordered or ordered) placed inside a list item (`<li>`) of another list.
- **List Marker**: The bullet point, number, or image displayed next to a list item.
- **Navigation Menu**: A UI element built using an unordered list (`<ul>`) and styled with CSS to look like a horizontal or vertical menu bar.

## Beginner Level Introduction

### Nested Lists

Lists can be nested inside one another. This is perfect for creating outlines, sub-categories, or multi-level menus.

To create a nested list, you put a new `<ul>` or `<ol>` **inside** an existing `<li>` element.

```html
<ul>
  <li>Beverages
    <ul>
      <li>Water</li>
      <li>Coffee</li>
    </ul>
  </li>
  <li>Food</li>
</ul>
```
Notice that the nested `<ul>` is placed *before* the closing `</li>` tag of "Beverages". This is a common beginner mistake (putting it after the closing tag breaks the HTML validation).

Browsers automatically change the bullet style of nested unordered lists (e.g., from a solid disc to a hollow circle).

## Deep Dive

### Custom List Styling

HTML attributes like `type="square"` on `<ul>` are obsolete in HTML5. You must use the CSS `list-style-type` property to change list markers.

Common values for `list-style-type`:
- `disc` (default for ul)
- `circle`
- `square`
- `none` (removes the marker entirely)
- `decimal` (default for ol)
- `lower-alpha`, `upper-alpha`
- `lower-roman`, `upper-roman`

You can even use an image as a custom bullet point using `list-style-image: url('sqpurple.gif');`.

### Navigation Menus

Almost every navigation menu on the web is actually an HTML unordered list styled with CSS. 

Why use a list instead of a bunch of `<a>` tags inside a `<div>`? Because a menu *is* semantically a list of links. This provides context to search engines and screen readers.

To turn a list into a horizontal menu:
1. Remove the bullets (`list-style-type: none;`).
2. Remove default padding and margins.
3. Make the list items display horizontally (e.g., `display: inline-block;` or `display: flex;`).

### List Accessibility

When screen readers encounter a properly formatted list (`<ul>` or `<ol>`), they announce "List with X items" before reading the first item. If you fake a list using `<div>` tags and manual bullet characters (&bull;), visually impaired users lose this critical context.

## Examples

<details>
<summary><strong>Example: Proper List Nesting</strong></summary>

```html
<h3>Table of Contents</h3>
<ol>
  <li>Introduction</li>
  <li>Chapter 1
    <!-- The nested list goes INSIDE the <li> for Chapter 1 -->
    <ol>
      <li>Section 1.1</li>
      <li>Section 1.2
        <ul>
          <li>A minor detail</li>
          <li>Another detail</li>
        </ul>
      </li>
    </ol>
  </li>
  <li>Conclusion</li>
</ol>
```

</details>

<details>
<summary><strong>Example: Creating a Navigation Bar from a List</strong></summary>

```html
<!-- CSS to style the list -->
<style>
  .nav-menu {
    list-style-type: none; /* Remove bullets */
    margin: 0;             /* Remove default margin */
    padding: 0;            /* Remove default padding */
    overflow: hidden;
    background-color: #333; /* Dark background */
  }

  .nav-menu li {
    float: left; /* Make items horizontal */
  }

  .nav-menu li a {
    display: block;
    color: white;
    text-align: center;
    padding: 14px 16px;
    text-decoration: none; /* Remove underline */
  }

  .nav-menu li a:hover {
    background-color: #111; /* Change color on hover */
  }
</style>

<!-- HTML Structure (Wrapped in semantic <nav> tag) -->
<nav>
  <ul class="nav-menu">
    <li><a href="#home">Home</a></li>
    <li><a href="#news">News</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#about">About</a></li>
  </ul>
</nav>
```

</details>
