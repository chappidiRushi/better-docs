---
sidebar_position: 1
---

# 16.1 HTML5 Semantics

## Definitions

- **Semantic HTML**: HTML tags that introduce meaning to the web page rather than just presentation. For example, `<p>` indicates a paragraph, whereas `<b>` only indicates bold text.
- **HTML5**: The latest major revision of the HTML standard, which introduced many new semantic elements to replace generic `<div>` tags.
- **Screen Reader**: Assistive technology that reads the text displayed on the computer screen with a speech synthesizer. Semantic HTML is crucial for screen readers.

## Beginner Level Introduction

Before HTML5 was released, web developers used the `<div>` tag for almost everything. 
A website's structure looked like this:
`<div id="header">`
`<div id="nav">`
`<div class="article">`
`<div id="footer">`

This was fine for styling with CSS, but it was terrible for machines trying to understand the content. Search engines (like Google) and screen readers couldn't tell which `<div>` contained the main article and which contained a useless sidebar advertisement.

HTML5 introduced **Semantic Elements**—tags with meaning.

```html
<header>
<nav>
<article>
<footer>
```
Now, when a machine reads the code, it knows exactly what each section represents.

## Deep Dive

### Key Semantic Elements in HTML5

- `<header>`: The introductory content for a page or a section (often contains a logo and heading).
- `<nav>`: A section containing navigation links.
- `<main>`: The dominant content of the `<body>`. There should only be one `<main>` per page.
- `<article>`: A self-contained composition that could logically be syndicated independently (e.g., a blog post, a news story).
- `<section>`: A generic standalone section of a document, which doesn't have a more specific semantic tag to represent it. Often contains a heading.
- `<aside>`: Content tangentially related to the content around it (e.g., a sidebar, call-out box, or advertising).
- `<footer>`: A footer for its nearest ancestor sectioning content (often contains copyright data or links to related documents).
- `<figure>` and `<figcaption>`: Used to wrap an image, illustration, or code snippet along with its caption, keeping them semantically linked together.

### Why Semantics Matter

1. **Accessibility**: This is the primary reason. Visually impaired users navigate websites via screen readers. Semantic tags allow them to skip directly to the `<main>` content, bypassing the `<nav>` links.
2. **SEO (Search Engine Optimization)**: Search engine crawlers prioritize content inside `<article>` and `<main>` tags over content inside `<aside>` tags when determining what a page is about.
3. **Maintainability**: Code written with semantic tags is significantly easier for other human developers to read and understand than a document made entirely of nested `<div>` tags.

### Deprecated Presentation Tags

HTML5 also aggressively deprecated tags that were purely for styling, pushing developers to use CSS instead.
- `<font>`, `<center>`, `<strike>`: Do not use these. Use CSS `font-family`, `text-align: center`, and `text-decoration: line-through`.
- `<b>` vs `<strong>`: `<b>` makes text bold. `<strong>` indicates that the text has *strong importance* (and the browser usually renders it bold). You should prefer `<strong>`.
- `<i>` vs `<em>`: `<i>` makes text italic. `<em>` indicates *emphasis*. You should prefer `<em>`.

## Examples

<details>
<summary><strong>Example: A Fully Semantic HTML5 Layout</strong></summary>

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Semantic Tech Blog</title>
</head>
<body>

  <!-- Page Header -->
  <header>
    <h1>The Developer Log</h1>
    <nav>
      <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/archive">Archive</a></li>
        <li><a href="/about">About</a></li>
      </ul>
    </nav>
  </header>

  <!-- The Main Content Area -->
  <main>
    <!-- An independent article -->
    <article>
      <header>
        <h2>Understanding CSS Grid</h2>
        <p>Published on: <time datetime="2023-10-27">October 27, 2023</time></p>
      </header>
      
      <p>CSS Grid is a two-dimensional layout system...</p>
      
      <!-- A figure linking an image and its caption -->
      <figure>
        <img src="grid-diagram.png" alt="Diagram showing CSS Grid tracks">
        <figcaption>Fig. 1: CSS Grid Tracks (Rows and Columns)</figcaption>
      </figure>

      <p>As you can see in the diagram...</p>
    </article>
  </main>

  <!-- Sidebar Content -->
  <aside>
    <h3>About the Author</h3>
    <p>Jane is a front-end developer who loves CSS.</p>
  </aside>

  <!-- Page Footer -->
  <footer>
    <p>&copy; 2023 The Developer Log.</p>
  </footer>

</body>
</html>
```

</details>
