---
sidebar_position: 3
---

# 7.3 HTML Layout

## Definitions

- **Layout**: The process of arranging visual elements on a web page.
- **Semantic HTML**: The use of HTML markup to reinforce the semantics, or meaning, of the information in web pages rather than merely to define its presentation or look.
- **Header**: The top section of a web page, usually containing a logo and navigation.
- **Footer**: The bottom section of a web page, usually containing copyright info, links, and contact details.

## Beginner Level Introduction

Websites often display content in multiple columns, like a magazine or newspaper. 

In the early days of the web, developers used HTML `<table>` elements to create layouts. This was a terrible practice because tables are meant for tabular data, not design. Later, developers used `<div>` elements heavily (a practice known as "divitis"). 

Today, HTML5 offers **Semantic Layout Elements** that describe the different parts of a web page precisely. 

Using these elements doesn't magically make your site look like a grid (you still need CSS for that), but it makes your HTML highly readable, accessible, and SEO-friendly.

## Deep Dive

### HTML5 Layout Elements

Here are the primary semantic elements used to structure a page:

- `<header>`: Defines a header for a document or a section. It usually contains introductory content or navigational links (like a logo and main menu).
- `<nav>`: Defines a set of navigation links. Not all links should be inside a `<nav>` element—only major navigation blocks.
- `<main>`: Specifies the main, central content of a document. The content inside `<main>` should be unique to the document. It should not contain any content that is repeated across documents such as sidebars, navigation links, copyright information, site logos, and search forms. There must be only one `<main>` element per page.
- `<section>`: Defines a section in a document (e.g., chapters, headers, footers, or any other sections of the document).
- `<article>`: Specifies independent, self-contained content. An article should make sense on its own, and it should be possible to distribute it independently from the rest of the site (e.g., a forum post, a blog post, a news story).
- `<aside>`: Defines some content aside from the content it is placed in (like a sidebar). The aside content should be indirectly related to the surrounding content.
- `<footer>`: Defines a footer for a document or a section. A footer typically contains authorship information, copyright information, contact information, sitemap, back to top links, related documents, etc.

### Why Semantics Matter

Imagine a visually impaired user navigating your site with a screen reader. If your entire layout is built with `<div>` tags, the screen reader has no idea where the navigation menu ends and the main article begins. The user is forced to listen to every single link in the header before getting to the actual content.

When you use semantic tags like `<main>` and `<nav>`, screen readers provide "skip links," allowing users to press a shortcut key to jump instantly to the `<main>` content, vastly improving their experience.

## Examples

<details>
<summary><strong>Example: A Classic Holy Grail Layout Structure</strong></summary>

```html
<!DOCTYPE html>
<html lang="en">
<body>

  <!-- The Header -->
  <header>
    <h1>The Daily News</h1>
    
    <!-- The Navigation Menu -->
    <nav>
      <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/world">World</a></li>
        <li><a href="/tech">Technology</a></li>
      </ul>
    </nav>
  </header>

  <!-- The Main Content Area -->
  <main>
    
    <!-- An independent piece of content -->
    <article>
      <header>
        <h2>Mars Rover Discovers Water</h2>
        <p>Published on Oct 27, 2023</p>
      </header>
      
      <!-- Sub-sections within the article -->
      <section>
        <h3>The Discovery</h3>
        <p>Scientists announced today...</p>
      </section>
      
      <section>
        <h3>Implications for Life</h3>
        <p>This means that...</p>
      </section>
    </article>

    <!-- A second independent piece of content -->
    <article>
      <h2>Stock Market Reaches All Time High</h2>
      <p>Investors are thrilled as...</p>
    </article>

  </main>

  <!-- A Sidebar -->
  <aside>
    <h2>Trending Topics</h2>
    <ul>
      <li><a href="#">#SpaceX</a></li>
      <li><a href="#">#TechStocks</a></li>
    </ul>
  </aside>

  <!-- The Footer -->
  <footer>
    <p>&copy; 2023 The Daily News. All rights reserved.</p>
    <address>Contact us at <a href="mailto:editor@dailynews.com">editor@dailynews.com</a></address>
  </footer>

</body>
</html>
```

</details>
