---
sidebar_position: 2
---

# 3.2 HTML Quotations

## Definitions

- **Blockquote**: A section that is quoted from another source, usually rendered as an indented block of text.
- **Inline Quote**: A short quotation that is embedded within a standard paragraph.
- **Citation**: A reference to a published or unpublished source, providing the origin of the quoted material.
- **Abbreviation**: A shortened form of a word or phrase (e.g., HTML, WHO).

## Beginner Level Introduction

### Blockquote

If you want to quote a large section of text from another website or source, use the `<blockquote>` element. 

Browsers typically indent `<blockquote>` elements automatically to visually separate them from the surrounding text.

```html
<p>Here is a quote from WWF's website:</p>
<blockquote>
For 60 years, WWF has worked to help people and nature thrive. As the world's leading conservation organization, WWF works in nearly 100 countries.
</blockquote>
```

### Short Quotes

For shorter quotes that fit within a normal paragraph, use the `<q>` tag.

Browsers normally insert quotation marks around the quotation automatically when using the `<q>` tag.

```html
<p>WWF's goal is to: <q>Build a future where people live in harmony with nature.</q></p>
```

## Deep Dive

### Citations (`<cite>`)

The `<cite>` tag is used to define the title of a creative work (e.g., a book, a poem, a song, a movie, a painting, a sculpture, etc.). A person's name is *not* the title of a work.

Browsers usually display `<cite>` elements in italic.

```html
<p><cite>The Scream</cite> by Edvard Munch. Painted in 1893.</p>
```

**Linking the Quote Source:**
Both `<blockquote>` and `<q>` elements accept a special attribute called `cite`. The `cite` attribute holds a URL that points to the original source of the quote. This is not visible to the user but is useful for search engines and screen readers.

### Abbreviations (`<abbr>`)

The `<abbr>` tag defines an abbreviation or an acronym. Marking up abbreviations can give useful information to browsers, translation systems, and search engines.

It is best practice to use the global `title` attribute along with `<abbr>`. When a user hovers their mouse over the abbreviation, the full expanded text will appear as a tooltip.

```html
<p>The <abbr title="World Health Organization">WHO</abbr> was founded in 1948.</p>
```

### Address Element (`<address>`)

The `<address>` tag defines the contact information for the author/owner of a document or an article. The contact information can be an email address, URL, physical address, phone number, social media handle, etc.

The text in the `<address>` element usually renders in italic, and browsers will always add a line break before and after it.

## Examples

<details>
<summary><strong>Example: Blockquote with Citation URL</strong></summary>

```html
<p>According to the MDN Web Docs:</p>
<!-- 
  Special Attribute: cite
  - A URL indicating the source document or message for the information quoted. 
  - Not visible on screen.
-->
<blockquote cite="https://developer.mozilla.org/en-US/docs/Web/HTML">
  <p>HTML is the most basic building block of the Web. It defines the meaning and structure of web content.</p>
</blockquote>
```

</details>

<details>
<summary><strong>Example: Abbreviations and Citations</strong></summary>

```html
<p>I am reading <cite>The Lord of the Rings</cite> by J.R.R. Tolkien.</p>
<!-- <cite> formats the title of the book in italics. -->

<p>I learned HTML at the <abbr title="World Wide Web Consortium">W3C</abbr> website.</p>
<!-- 
  Special Attribute: title
  - Provides the full expansion of the abbreviation on hover.
-->
```

</details>

<details>
<summary><strong>Example: Contact Address</strong></summary>

```html
<address>
  Written by <a href="mailto:webmaster@example.com">Jon Doe</a>.<br> 
  Visit us at:<br>
  Example.com<br>
  Box 564, Disneyland<br>
  USA
</address>
<!-- 
  <address> semantically wraps the contact info.
  <br> is used to format the postal address properly.
-->
```

</details>
