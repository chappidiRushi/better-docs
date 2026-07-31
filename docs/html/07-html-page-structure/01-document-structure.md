---
sidebar_position: 1
---

# 7.1 Document Structure

## Definitions

- **DOCTYPE**: A declaration that tells the web browser what version of HTML the page is written in. It must be the very first thing in your HTML document.
- **Root Element**: The top-most element in a tree-like document structure. In HTML, this is the `<html>` tag.
- **DOM (Document Object Model)**: A programming interface for web documents. It represents the page so that programs can change the document structure, style, and content.

## Beginner Level Introduction

Every valid HTML document requires a specific skeleton to function correctly across all browsers. This skeleton is composed of four main parts:

1. `<!DOCTYPE html>`: The document type declaration.
2. `<html>`: The root element wrapping all content.
3. `<head>`: The container for metadata (stuff the user doesn't see directly).
4. `<body>`: The container for the visible content (stuff the user does see).

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Page Title</title>
  </head>
  <body>
    <h1>My First Heading</h1>
    <p>My first paragraph.</p>
  </body>
</html>
```

## Deep Dive

### 1. The `DOCTYPE` Declaration

The `<!DOCTYPE>` declaration is not an HTML tag; it is an instruction to the web browser about what version of HTML the page is written in. 

In older versions of HTML (like HTML 4 or XHTML), the DOCTYPE declaration was incredibly long and complex because it had to refer to a specific Document Type Definition (DTD) on the web.

**HTML5 DOCTYPE:**
```html
<!DOCTYPE html>
```
This simple declaration triggers "standards mode" in all modern browsers, ensuring the page is rendered according to the latest HTML5 and CSS3 specifications. If you omit it, browsers might enter "quirks mode," rendering the page using backwards-compatible (and often broken) rules from the 1990s.

### 2. The `html` Element

The `<html>` element represents the root of an HTML document. All other elements must be descendants of this element.

It is highly recommended to always include the `lang` attribute inside the `<html>` tag. This declares the primary language of the web page.

```html
<html lang="en">
```

Declaring the language is vital for:
- **Screen readers**: They use this attribute to choose the correct pronunciation rules.
- **Search engines**: They use it to serve language-specific search results.
- **Translation tools**: They use it to prompt the user to translate the page if it's not in their native language.

### 3. The `head` Element

The `<head>` element is a container for metadata (data about data). It is placed between the `<html>` tag and the `<body>` tag.

Metadata typically define the document title, character set, styles, scripts, and other meta information. None of the content inside the `<head>` is displayed on the webpage itself (except for the `<title>`, which appears in the browser tab).

### 4. The `body` Element

The `<body>` element defines the document's body. It contains all the contents of an HTML document, such as headings, paragraphs, images, hyperlinks, tables, lists, etc.

There can only be one `<body>` element in an HTML document.

## Examples

<details>
<summary><strong>Example: A Complete Boilerplate</strong></summary>

```html
<!-- 
  The DOCTYPE is case-insensitive, but <!DOCTYPE html> is standard. 
-->
<!DOCTYPE html>

<!-- 
  Special Attribute: lang
  - "en" stands for English. "en-US" would be US English.
-->
<html lang="en">
  
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document Skeleton</title>
  </head>

  <!-- 
    Everything visible goes inside the body.
  -->
  <body>
    <header>
      <h1>Website Header</h1>
    </header>
    
    <main>
      <p>This is the main content of the page.</p>
    </main>
  </body>

</html>
```

</details>
