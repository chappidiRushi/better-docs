---
sidebar_position: 4
---

# 2.4 HTML Paragraphs

## Definitions

- **Paragraph**: A block of text containing one or more sentences. In HTML, it is the primary element used for grouping blocks of text.
- **Line Break**: A forced carriage return in the text, pushing subsequent text to the next line without starting a new paragraph.
- **Horizontal Rule**: A thematic break between paragraph-level elements, usually rendered as a horizontal line.
- **Preformatted Text**: Text that is displayed exactly as it is written in the HTML file, preserving all spaces and line breaks.
- **Whitespace Collapsing**: The behavior of web browsers where multiple spaces or line breaks in the HTML code are reduced to a single space when displayed on screen.

## Beginner Level Introduction

### Paragraph Element

The `<p>` element defines a paragraph. It is a block-level element, meaning it always starts on a new line, and browsers automatically add some margin (whitespace) before and after it to separate it from other elements.

```html
<p>This is a paragraph.</p>
<p>This is another paragraph.</p>
```

When you write text inside a `<p>` tag, the browser will automatically wrap the text based on the width of the user's screen or the container the paragraph is in.

### Line Breaks

Sometimes you want a line break (a new line) without starting a new paragraph. For example, when writing a poem or a postal address. To do this, use the `<br>` element.

The `<br>` tag is an empty element (it has no closing tag).

```html
<p>This is line one.<br>This is line two.</p>
```

## Deep Dive

### Whitespace Rules (Whitespace Collapsing)

One of the most common points of confusion for HTML beginners is how browsers handle spaces and enter/return key presses in the code editor.

If you add extra spaces or line breaks inside your HTML code, the browser will ignore them. **Multiple spaces are collapsed into a single space.**

```html
<!-- This: -->
<p>
  This paragraph
  contains   a lot of
  spaces.
</p>

<!-- Will display exactly the same as this: -->
<p>This paragraph contains a lot of spaces.</p>
```

### Preformatted Text (`<pre>`)

If you want the browser to respect exact spaces and line breaks just as you typed them in your code editor, use the `<pre>` element.

The `<pre>` element is typically used for displaying computer code, ASCII art, or poetry. The text inside is usually rendered in a fixed-width (monospace) font.

```html
<pre>
   This text
   preserves
      spaces and
   line breaks.
</pre>
```

### Horizontal Rules (`<hr>`)

The `<hr>` element represents a thematic break between paragraph-level elements (for example, a change of scene in a story, or a shift of topic within a section). 

Visually, it is usually displayed as a horizontal line drawn across the page. Like `<br>`, it is an empty element.

```html
<p>Topic 1 paragraph.</p>
<hr>
<p>Topic 2 paragraph.</p>
```

## Examples

<details>
<summary><strong>Example: Paragraphs vs Line Breaks</strong></summary>

```html
<!-- Paragraphs add semantic meaning and vertical margins -->
<p>John Doe</p>
<p>123 Main Street</p>
<p>Anytown, USA</p>

<!-- Line breaks keep the text grouped together without extra margins -->
<p>
  Jane Doe<br>
  456 Oak Avenue<br>
  Springfield, USA
</p>
```

</details>

<details>
<summary><strong>Example: Using Preformatted Text for Code</strong></summary>

```html
<p>Here is an example of a simple JavaScript function:</p>

<!-- 
  Special Attribute on <pre> or <code> (usually class):
  - Often used in combination with <code> to semantically define computer code.
-->
<pre><code class="language-javascript">
function greet(name) {
    if (name) {
        console.log("Hello, " + name + "!");
    } else {
        console.log("Hello, stranger!");
    }
}
</code></pre>
```

</details>
