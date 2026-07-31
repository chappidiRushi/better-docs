---
sidebar_position: 2
---

# 4.2 HTML CSS

## Definitions

- **CSS (Cascading Style Sheets)**: A stylesheet language used to describe the presentation of a document written in HTML.
- **Selector**: A pattern used to select the element(s) you want to style.
- **Property**: The specific aspect of the element you want to change (e.g., `color`, `font-size`).
- **Value**: The specific setting you want to apply to the property (e.g., `red`, `16px`).
- **Responsive Design**: The practice of building web pages that detect the visitor's screen size and orientation and change the layout accordingly.

## Beginner Level Introduction

HTML is responsible for the structure and content of a web page. **CSS** is responsible for the design and layout. 

Without CSS, every webpage would look like a plain, unformatted Word document from 1995. CSS allows you to change fonts, colors, spacing, positioning, and add animations.

There are three ways to add CSS to an HTML document:
1. **Inline CSS**: Using the `style` attribute inside HTML elements.
2. **Internal CSS**: Using a `<style>` element in the `<head>` section.
3. **External CSS**: Using a `<link>` element to link to an external `.css` file.

## Deep Dive

### 1. Inline CSS

An inline style may be used to apply a unique style for a single element. It is defined using the `style` attribute.

```html
<h1 style="color:blue; text-align:center;">This is a heading</h1>
```
*Best Practice:* Inline CSS should generally be avoided. It makes HTML code messy, harder to read, and impossible to reuse styles across multiple elements.

### 2. Internal CSS

An internal stylesheet may be used if one single HTML page has a unique style. The internal style is defined inside the `<style>` element, inside the `<head>` section.

```html
<head>
<style>
  body { background-color: linen; }
  h1 { color: maroon; }
</style>
</head>
```

### 3. External CSS

With an external stylesheet, you can change the look of an entire website by changing just one file! This is the professional standard for web development.

You create a separate file (e.g., `styles.css`), and link it in the `<head>` of your HTML document using the `<link>` tag.

```html
<head>
  <link rel="stylesheet" href="styles.css">
</head>
```

### CSS Selectors

To style HTML elements, CSS needs a way to "find" or "select" them. 

1. **Element Selector**: Selects HTML elements based on the element name. (e.g., `p { color: red; }`)
2. **Class Selector (`.`)**: Selects elements with a specific class attribute. A class can be used on multiple elements. (e.g., `.highlight { background: yellow; }`)
3. **ID Selector (`#`)**: Selects an element with a specific id attribute. An ID must be unique on the page. (e.g., `#main-header { font-size: 2em; }`)

### CSS Variables (Custom Properties)

Modern CSS allows you to declare variables. This is incredibly useful for defining a theme (like primary colors) in one place and reusing it everywhere.

```css
:root {
  --primary-color: #3498db;
}
button {
  background-color: var(--primary-color);
}
```

### Responsive Styling

Modern websites must look good on mobile phones, tablets, and desktops. We achieve this using CSS **Media Queries**, which apply different styles based on the screen width.

## Examples

<details>
<summary><strong>Example: Linking an External Stylesheet</strong></summary>

**`index.html`**
```html
<!DOCTYPE html>
<html>
<head>
  <!-- 
    Special Attributes on <link>:
    - rel: Specifies the relationship between the current document and the linked document ("stylesheet").
    - href: The URL/path to the CSS file.
  -->
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <h1 id="main-title">My Website</h1>
  <p class="intro-text">Welcome to my site.</p>
  <p class="intro-text">This is styled externally.</p>
</body>
</html>
```

**`styles.css`**
```css
/* ID Selector */
#main-title {
  color: navy;
  text-align: center;
}

/* Class Selector */
.intro-text {
  font-family: Arial, sans-serif;
  color: gray;
}
```

</details>

<details>
<summary><strong>Example: Responsive Design with Media Queries</strong></summary>

```html
<style>
  /* Default style (Mobile First approach) */
  .container {
    width: 100%;
    background-color: lightgreen;
  }

  /* When the screen width is 600px or larger, apply these styles */
  @media (min-width: 600px) {
    .container {
      width: 50%; /* Take up half the screen on tablets/desktops */
      background-color: lightblue;
      margin: 0 auto; /* Center it */
    }
  }
</style>

<div class="container">
  <h2>Resize your browser window!</h2>
  <p>The background color and width will change based on screen size.</p>
</div>
```

</details>
