---
sidebar_position: 3
---

# 10.3 IDs

## Definitions

- **ID Attribute (`id`)**: A global attribute used to define a unique identifier for an HTML element.
- **CSS ID Selector (`#`)**: The syntax used in CSS to target an element based on its ID.
- **Fragment Identifier**: The string of characters (starting with `#`) added to the end of a URL to point to a specific ID on a webpage.

## Beginner Level Introduction

### Unique IDs

The `id` attribute specifies a unique id for an HTML element. 

The value of the `id` attribute **must be unique** within the HTML document. This means you cannot have more than one element with the same `id` on the same page.

While classes are for grouping similar elements together (like giving 10 buttons the class `btn`), IDs are for pinpointing one exact, specific element (like giving the main site navigation the ID `primary-nav`).

```html
<!-- HTML -->
<h1 id="myHeader">My Unique Header</h1>
```

```css
/* CSS */
#myHeader {
  background-color: lightblue;
  color: black;
  text-align: center;
}
```

### Naming Conventions

- ID names are **case-sensitive**.
- ID names must **not start with a number**.
- Use hyphens (`-`) or camelCase (e.g., `main-header`, `mainHeader`).

## Deep Dive

### Anchor Navigation (Bookmark Links)

One of the most powerful native features of the `id` attribute is its ability to create bookmark links. 

If you give an element an `id` (e.g., `<h2 id="contact-us">`), you can create a link anywhere else on the page that jumps directly to that element by using a hash symbol `#` followed by the ID name in the `href`.

```html
<a href="#contact-us">Jump to Contact Form</a>
```

You can even link to that specific section from an entirely different website by appending the hash to the URL: `https://example.com/about.html#contact-us`.

### JavaScript Targeting

Because IDs are guaranteed to be unique on a page, they are the fastest and most reliable way for JavaScript to find a specific element to manipulate it.

```javascript
// JavaScript example
const headerElement = document.getElementById("myHeader");
// The script can now change the text, hide the element, etc.
```

### IDs vs Classes

**When to use a Class:**
- When you want to style multiple elements the same way.
- When you are building reusable components (cards, buttons, alerts).
- For almost all of your CSS styling.

**When to use an ID:**
- When you need a unique anchor link target.
- When you have a major structural element that only appears once per page (like a main header or footer), though semantic tags usually handle this better now.
- When a JavaScript function absolutely relies on finding one specific element.
- For associating labels with form inputs (covered in the Forms section).

*Note: In CSS, IDs have a much higher specificity than classes. If an element has a class defining the color red, and an ID defining the color blue, the ID will win, and the text will be blue.*

## Examples

<details>
<summary><strong>Example: ID and Class on the Same Element</strong></summary>

```html
<style>
  /* Base card styles applied to all cards */
  .card {
    border: 1px solid gray;
    padding: 20px;
    margin: 10px;
  }
  
  /* Specific style overriding the base style for ONLY the featured card */
  #featured-card {
    border: 3px solid gold;
    background-color: lightyellow;
  }
</style>

<!-- 
  Special Attributes: id and class
  - You can safely use both on the same element.
-->
<div id="featured-card" class="card">
  <h2>Premium Plan</h2>
  <p>$99/month</p>
</div>

<div class="card">
  <h2>Basic Plan</h2>
  <p>$9/month</p>
</div>
```

</details>

<details>
<summary><strong>Example: JavaScript and IDs</strong></summary>

```html
<!-- The unique element -->
<h1 id="greeting-title">Hello Stranger</h1>

<!-- A button that triggers a script -->
<button onclick="changeName()">Click to log in</button>

<script>
  function changeName() {
    // JavaScript uses the unique ID to find the exact H1 element
    document.getElementById("greeting-title").innerHTML = "Hello Jane Doe!";
  }
</script>
```

</details>
