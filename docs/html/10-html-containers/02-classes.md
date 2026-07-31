---
sidebar_position: 2
---

# 10.2 Classes

## Definitions

- **Class Attribute (`class`)**: A global attribute used to define a class (or multiple classes) for an HTML element.
- **CSS Class Selector (`.`)**: The syntax used in CSS to target elements that possess a specific class.
- **Reusability**: The ability to write a piece of code (like a CSS class) once and apply it to many different elements across a website.

## Beginner Level Introduction

### Creating Classes

The `class` attribute is used to specify a class for an HTML element. 
Multiple HTML elements can share the same class. This is the primary mechanism for styling websites efficiently.

Instead of writing `style="background: red; color: white;"` on twenty different buttons, you create a class named `btn-danger` in your CSS, and simply add `class="btn-danger"` to your HTML elements.

```html
<!-- HTML -->
<div class="city">
  <h2>London</h2>
  <p>London is the capital of England.</p>
</div>

<div class="city">
  <h2>Paris</h2>
  <p>Paris is the capital of France.</p>
</div>
```

```css
/* CSS */
.city {
  background-color: tomato;
  color: white;
  padding: 10px;
}
```

### Naming Conventions

- Class names are **case-sensitive** (e.g., `City` and `city` are different).
- Class names must **not start with a number**.
- Use hyphens (`-`) or underscores (`_`) to separate words (e.g., `main-header`, `nav_item`). Avoid spaces (spaces are used to apply multiple classes).

## Deep Dive

### Multiple Classes

An HTML element can have more than one class name. To specify multiple classes, separate the class names with a space.

```html
<div class="city prominent">
```
In this example, the `<div>` element will receive the styles defined in the `.city` class *and* the styles defined in the `.prominent` class.

This is the foundation of utility-first CSS frameworks like Tailwind CSS, where you build layouts by applying many small, single-purpose classes to elements.

### CSS Targeting (Specificity)

You can use classes to target specific elements in CSS very precisely.

If you have a `<p>` tag and a `<span>` tag that both share the class `important`, you can target them individually in CSS.

```css
/* Targets ANY element with class="important" */
.important {
  font-weight: bold;
}

/* Targets ONLY <p> elements with class="important" */
p.important {
  color: red;
}
```

### JavaScript Targeting

Classes are not just for CSS. JavaScript frequently uses classes to find and manipulate elements on the page.

```javascript
// JavaScript example
const cities = document.getElementsByClassName("city");
// 'cities' now holds a list of all elements with the class "city"
```

## Examples

<details>
<summary><strong>Example: Applying Multiple Classes</strong></summary>

```html
<style>
  /* Base button styling */
  .btn {
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
  
  /* Specific color themes */
  .btn-primary { background-color: blue; color: white; }
  .btn-danger { background-color: red; color: white; }
  
  /* Specific size modifiers */
  .btn-large { font-size: 20px; padding: 15px 30px; }
</style>

<!-- 
  Special Attribute: class
  - Notice the space separation. 
  - This button gets the base .btn styles, the .btn-danger color, and the .btn-large size.
-->
<button class="btn btn-danger btn-large">Delete Account</button>
<button class="btn btn-primary">Save Changes</button>
```

</details>

<details>
<summary><strong>Example: Reusing Classes Across Different Elements</strong></summary>

```html
<style>
  .highlight-text {
    background-color: yellow;
    font-style: italic;
  }
</style>

<!-- The exact same class applied to a heading, a paragraph, and a span -->
<h2 class="highlight-text">Important Notice</h2>

<p class="highlight-text">Please read carefully.</p>

<p>You must sign the document by <span class="highlight-text">Friday at 5PM</span>.</p>
```

</details>
