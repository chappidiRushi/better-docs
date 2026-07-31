---
sidebar_position: 3
---

# 2.3 HTML Headings

## Definitions

- **Heading**: A title or subtitle that you want to display on a webpage.
- **Heading Hierarchy**: The logical order of headings, ranging from `<h1>` (most important) to `<h6>` (least important).
- **SEO (Search Engine Optimization)**: The practice of increasing the quantity and quality of traffic to your website through organic search engine results. Proper heading structure is a key factor in SEO.
- **Accessibility (a11y)**: The practice of making your websites usable by as many people as possible, including those with disabilities who may rely on screen readers.

## Beginner Level Introduction

### Heading Elements

HTML headings are defined with the `<h1>` to `<h6>` tags. 

Headings are used to establish a document's outline. Just like reading a book or a newspaper, you rely on large, bold text to understand what a section is about. 

- `<h1>` defines the most important heading (usually the title of the page).
- `<h6>` defines the least important heading.

```html
<h1>This is heading 1</h1>
<h2>This is heading 2</h2>
<h3>This is heading 3</h3>
```

By default, browsers render headings with larger, bolder fonts compared to standard paragraph text. They also add margin space before and after the heading.

## Deep Dive

### `h1` to `h6` Hierarchy

The numbers 1 through 6 represent the level in the document's hierarchy. 
You should never skip heading levels. For example, don't jump from an `<h1>` to an `<h3>` just because you want the text to look smaller. This breaks the logical outline of the page.

If you need a heading to *look* smaller but still be semantically an `<h2>`, you should use CSS to change its visual size, not change the HTML tag.

**Correct Hierarchy:**
```html
<h1>Main Topic</h1>
  <h2>Subtopic A</h2>
    <h3>Detail of Subtopic A</h3>
  <h2>Subtopic B</h2>
```

**Incorrect Hierarchy:**
```html
<h1>Main Topic</h1>
  <h3>Subtopic A (Skipped h2!)</h3>
```

### SEO-Friendly Headings

Search engines like Google use your headings to index the structure and content of your web pages.

- **Rule of Thumb**: Use only one `<h1>` per page. This should represent the main subject/title of the whole page.
- Use `<h2>` elements for main sections, `<h3>` for subsections, and so on.
- Include relevant keywords in your headings, as search engines give more weight to text inside heading tags than standard paragraph text.

### Accessibility Considerations

Screen readers (software used by visually impaired users to read web pages aloud) rely heavily on heading structure.

Users often use keyboard shortcuts to jump from heading to heading to quickly skim a page's content, similar to how a sighted user scans a page visually. If your heading hierarchy is broken or if you use bold `<p>` tags instead of proper heading tags, screen reader users will have a terrible time navigating your site.

## Examples

<details>
<summary><strong>Example: Proper Heading Structure</strong></summary>

```html
<!-- The main title of the page. Only use one per page. -->
<h1>A Complete Guide to Web Development</h1>

<!-- A major section -->
<h2>Frontend Development</h2>
<p>Frontend deals with what the user sees.</p>

  <!-- A subsection of Frontend -->
  <h3>HTML</h3>
  <p>The skeleton of the web.</p>
  
  <!-- Another subsection of Frontend -->
  <h3>CSS</h3>
  <p>The styling of the web.</p>

<!-- Another major section -->
<h2>Backend Development</h2>
<p>Backend deals with servers and databases.</p>

  <!-- A subsection of Backend -->
  <h3>Node.js</h3>
  <p>JavaScript on the server.</p>
```

</details>

<details>
<summary><strong>Example: Styling Headings (Separating Semantics from Presentation)</strong></summary>

Sometimes you want an `<h2>` to look visually identical to an `<h1>` without breaking the document outline.

```html
<!-- HTML Structure -->
<h1>Main Article Title</h1>

<!-- 
  Special Attribute: class
  - We use the class to change the appearance via CSS, 
    but the HTML remains semantically correct as an <h2>.
-->
<h2 class="make-it-huge">Secondary Title</h2>

<!-- CSS Styling (usually placed in a separate file or <style> block) -->
<style>
  h1 {
    font-size: 2rem;
  }
  .make-it-huge {
    /* Making the h2 look bigger than the h1 visually */
    font-size: 3rem; 
  }
</style>
```

</details>
