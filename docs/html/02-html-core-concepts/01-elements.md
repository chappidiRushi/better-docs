---
sidebar_position: 1
---

# 2.1 HTML Elements

## Definitions

- **Element**: A complete building block of HTML, typically consisting of an opening tag, content, and a closing tag.
- **Tag**: The keywords surrounded by angle brackets (e.g., `<html>`) that define the start or end of an element.
- **Node**: An individual part of the Document Object Model (DOM) tree. Every HTML element becomes a node in the DOM.
- **Semantics**: The meaning of a piece of code. Semantic HTML elements clearly describe their meaning to both the browser and the developer (e.g., `<article>` instead of `<div>`).

## Beginner Level Introduction

### What are HTML Elements?

An HTML element is everything from the start tag to the end tag. Elements are the fundamental building blocks of webpages. They tell the browser what kind of content they enclose—whether it's a paragraph, a heading, an image, or a link.

```html
<p>This is a paragraph element.</p>
```
In this example:
- `<p>` is the opening tag.
- `This is a paragraph element.` is the content.
- `</p>` is the closing tag.
- The entire line is the HTML element.

### Opening and Closing Tags

Most HTML elements require both an opening and a closing tag.
- **Opening Tag**: `<tagname>` - Marks the beginning of the element.
- **Closing Tag**: `</tagname>` - Marks the end of the element. Note the forward slash `/`.

If you forget the closing tag, the browser might try to guess where the element ends, which can lead to unexpected and broken layouts.

### Empty Elements (Void Elements)

Some elements do not have any content and therefore do not need a closing tag. These are called **empty elements** or **void elements**.

Examples:
- `<br>`: Line break
- `<hr>`: Horizontal rule (a thematic break/line)
- `<img>`: Image
- `<input>`: Form input field

In modern HTML5, you don't need to close void elements with a trailing slash (like `<br />`), though doing so is perfectly valid and is required in XML/XHTML.

### Nested Elements

HTML elements can be nested, meaning elements can contain other elements. In fact, all HTML documents consist of nested HTML elements.

```html
<body>
  <div>
    <p>This paragraph is nested inside a div, which is nested inside the body.</p>
  </div>
</body>
```

**Rule of nesting**: Always close elements in the reverse order they were opened. The inner-most element must be closed before the outer-most element.

## Deep Dive

### Block Elements vs Inline Elements

Understanding how elements behave by default is critical for CSS styling and layout.

#### Block-level Elements
- Always start on a **new line**.
- Take up the **full width** available (stretching from left to right as far as they can).
- Have a top and bottom margin by default.
- Examples: `<div>`, `<h1>` to `<h6>`, `<p>`, `<ul>`, `<li>`, `<section>`, `<article>`.

#### Inline Elements
- Do **not** start on a new line.
- Take up only as much **width as necessary**.
- Cannot have top or bottom margins/padding applied to them in a way that pushes other elements vertically.
- Examples: `<span>`, `<a>`, `<strong>`, `<em>`, `<img>`.

### HTML Element Categories (Content Models)

HTML5 categorizes elements into broader content models based on their expected usage. This determines what kinds of elements can be nested inside others.

1. **Flow content**: Contains most elements that are used in the body of documents (e.g., `div`, `p`, `span`, `h1`).
2. **Sectioning content**: Elements that define the scope of headings and footers (e.g., `article`, `aside`, `nav`, `section`).
3. **Heading content**: Defines the heading of a section (e.g., `h1`, `h2`).
4. **Phrasing content**: The text of the document and the markup it contains (e.g., `span`, `strong`, `em`, `a`). (Mostly aligns with inline elements).
5. **Embedded content**: Imports another resource or inserts content from another markup language (e.g., `audio`, `video`, `canvas`, `iframe`, `img`).
6. **Interactive content**: Elements intended for user interaction (e.g., `button`, `details`, `input`, `select`, `textarea`).
7. **Metadata content**: Elements that set up the presentation or behavior of the rest of the document, set up links to other documents, or convey out-of-band information (e.g., `base`, `link`, `meta`, `script`, `style`, `title`).

### Custom Elements Introduction

With the advent of Web Components, you can define your own custom HTML elements using JavaScript. These act just like standard HTML tags but encapsulate their own styling and functionality. Custom elements must contain a dash (`-`) in their name to prevent collisions with standard or future HTML tags.

```html
<my-custom-button>Click Me</my-custom-button>
```
*(Defining the logic for this requires JavaScript's `customElements.define()` API).*

## Examples

<details>
<summary><strong>Example: Proper Nesting and Void Elements</strong></summary>

```html
<!-- Example of standard elements vs void elements -->
<div>
  <h2>Block vs Inline</h2>
  <!-- <p> is a block element -->
  <p>
    This is a paragraph. 
    <!-- <strong> is an inline element nested correctly -->
    <strong>This text is bold.</strong> 
    This text is normal.
  </p>
  
  <!-- <hr> is a void element, it needs no closing tag -->
  <hr> 
  
  <!-- <img> is also a void element -->
  <!-- 
    Special Attributes:
    - src: The path to the image
    - alt: Alternative text if image fails to load
  -->
  <img src="logo.png" alt="Company Logo">
</div>
```

</details>

<details>
<summary><strong>Example: Incorrect vs Correct Nesting</strong></summary>

```html
<!-- ❌ INCORRECT NESTING -->
<!-- The <em> tag is opened inside <strong> but closed outside of it -->
<p><strong>This is <em>incorrectly</strong> nested.</em></p>


<!-- ✅ CORRECT NESTING -->
<!-- Elements are closed in the exact reverse order they were opened -->
<p><strong>This is <em>correctly</em> nested.</strong></p>
```

</details>
