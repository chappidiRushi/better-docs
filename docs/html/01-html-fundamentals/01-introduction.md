---
sidebar_position: 1
---

# 1.1 HTML Introduction

## Definitions

- **HTML**: HyperText Markup Language, the standard markup language for creating web pages.
- **HyperText**: Text displayed on a computer display or other electronic devices with references (hyperlinks) to other text that the reader can immediately access.
- **Markup Language**: A system for annotating a document in a way that is syntactically distinguishable from the text, meaning when the document is processed for display, the markup language is not shown, and is only used to format the text.
- **Web Browser**: A software application for accessing information on the World Wide Web. When a user requests a web page, the web browser retrieves the necessary content from a web server and then displays the page on the user's device.
- **W3C (World Wide Web Consortium)**: The main international standards organization for the World Wide Web.
- **WHATWG (Web Hypertext Application Technology Working Group)**: A community of people interested in evolving HTML and related technologies.

## Beginner Level Introduction

### What is HTML?

HTML stands for **H**yper**T**ext **M**arkup **L**anguage. It is the most basic building block of the Web. It defines the meaning and structure of web content. Other technologies besides HTML are generally used to describe a web page's appearance/presentation (CSS) or functionality/behavior (JavaScript).

Think of HTML as the skeleton of a web page. Just like a house needs a frame before you can add walls and paint, a website needs HTML to structure its content—headings, paragraphs, images, links, and more—before it can be styled and made interactive.

### History of HTML

HTML was created by Sir Tim Berners-Lee in late 1991 but was not released officially until 1995 (HTML 2.0). Since then, it has undergone many revisions, evolving to support multimedia, interactivity, and complex layouts.

### HTML Versions

- **HTML 1.0 (1991)**: The first version, very basic.
- **HTML 2.0 (1995)**: Standardized core HTML features.
- **HTML 3.2 (1997)**: Added support for tables, applets, and text flow around images.
- **HTML 4.01 (1999)**: Separated styling from structure (pushed CSS adoption).
- **XHTML 1.0 (2000)**: HTML written as XML. Stricter syntax rules.
- **HTML5 (2014 - Present)**: The current standard. Introduced semantic elements, native video/audio, canvas, and advanced APIs. Currently maintained as a "Living Standard" by the WHATWG.

## Deep Dive

### How Browsers Render HTML

When you visit a web page, the following process happens:

1. **Request**: The browser sends an HTTP request to the server hosting the website.
2. **Response**: The server responds with the HTML document.
3. **Parsing**: The browser reads the HTML document top-to-bottom and constructs a tree-like structure called the **DOM (Document Object Model)**.
4. **CSSOM Construction**: If the HTML links to CSS, the browser parses it to create the **CSS Object Model**.
5. **Render Tree**: The DOM and CSSOM are combined to form the Render Tree, containing only the nodes required to render the page.
6. **Layout/Reflow**: The browser calculates the exact position and size of each element.
7. **Painting**: The browser paints the pixels on the screen.

Understanding this process is crucial for web performance optimization. Malformed HTML can block rendering, leading to a poor user experience.

### HTML Document Structure

Every standard HTML page follows a specific structure, often called boilerplate.

- `<!DOCTYPE html>`: Not an HTML tag, but an instruction to the web browser about what version of HTML the page is written in. For HTML5, it's simply `<!DOCTYPE html>`.
- `<html>`: The root element of an HTML page. All other elements must be descendants of this element.
- `<head>`: Contains meta-information about the HTML page, such as its title, character set, styles, and scripts. This content is *not* displayed directly on the webpage.
- `<body>`: Defines the document's body and is a container for all the visible contents, such as headings, paragraphs, images, hyperlinks, tables, lists, etc.

### HTML Syntax Rules

1. **Tags**: HTML uses elements enclosed in angle brackets (tags) like `<p>`.
2. **Pairs**: Most tags come in pairs: an opening tag `<p>` and a closing tag `</p>`. The closing tag has a forward slash `/`.
3. **Empty Elements**: Some elements don't have closing tags because they don't wrap content (e.g., `<img>`, `<br>`, `<hr>`).
4. **Nesting**: Elements can be nested inside each other. Proper nesting is essential (e.g., `<div><p>Text</p></div>` is correct; `<div><p>Text</div></p>` is wrong).
5. **Lowercase**: While HTML is technically case-insensitive, it is a strict best practice and industry standard to write tags and attributes in lowercase.

### HTML Validation

Because browsers are extremely forgiving, they will often try to render poorly formatted HTML without throwing errors. This can lead to unpredictable results across different browsers. HTML Validation is the process of checking your code against W3C standards using tools like the [W3C Markup Validation Service](https://validator.w3.org/). This ensures cross-browser compatibility and accessibility.

## Examples

<details>
<summary><strong>Example: The First HTML Page (Boilerplate)</strong></summary>

```html
<!-- 
  The DOCTYPE declaration is mandatory.
  It tells the browser to use the HTML5 standard parsing rules.
-->
<!DOCTYPE html>

<!-- 
  The root element.
  Special Attribute: 'lang'
  - Specifies the language of the document.
  - Essential for accessibility (screen readers) and search engines.
-->
<html lang="en">

  <!-- 
    The head element contains metadata.
    Content inside <head> is not visible on the webpage.
  -->
  <head>
    <!-- 
      Special Attribute: 'charset'
      - Specifies the character encoding.
      - 'UTF-8' covers almost all characters and symbols in the world.
    -->
    <meta charset="UTF-8">

    <!-- 
      Special Attributes: 'name' and 'content'
      - Configures the viewport for responsive web design on mobile devices.
    -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <!-- The title appears in the browser tab and search engine results -->
    <title>My First HTML Page</title>
  </head>

  <!-- 
    The body contains the visible content of the page.
  -->
  <body>
    
    <!-- A top-level heading -->
    <h1>Welcome to My Website</h1>
    
    <!-- A standard paragraph -->
    <p>This is my very first paragraph in HTML. Hello World!</p>

  </body>
</html>
```

</details>

<details>
<summary><strong>Example: HTML Comments</strong></summary>

Comments are ignored by the browser and are not displayed on the screen. They are useful for leaving notes for yourself or other developers, or for temporarily disabling code during debugging.

```html
<!DOCTYPE html>
<html lang="en">
<body>

  <!-- This is a single-line HTML comment -->
  <h1>Welcome</h1>

  <!-- 
    This is a multi-line HTML comment.
    You can write as much text as you want here.
    The browser will ignore all of it.
  -->
  <p>Some content here.</p>

  <!-- 
    Comments are often used to "comment out" code you want to hide temporarily:
    <p>This paragraph is currently disabled and won't be visible.</p>
  -->

</body>
</html>
```

</details>
