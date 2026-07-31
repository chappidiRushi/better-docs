---
sidebar_position: 2
---

# 7.2 HTML Head

## Definitions

- **Meta Tags**: Elements that provide metadata about the HTML document (e.g., description, keywords, author).
- **Charset (Character Set)**: A defined list of characters recognized by the computer hardware and software.
- **Viewport**: The user's visible area of a web page.
- **Open Graph (OG)**: A protocol that enables any web page to become a rich object in a social graph (used by Facebook, Twitter, LinkedIn to generate preview cards).
- **Favicon**: A small icon associated with a particular website, typically displayed in the address bar of a browser or on the browser tab.

## Beginner Level Introduction

The `<head>` element is a container for metadata. Metadata is data about the HTML document. Metadata is not displayed directly on the page.

The following elements can go inside the `<head>`:
- `<title>` (required in every HTML document)
- `<meta>`
- `<link>`
- `<style>`
- `<script>`
- `<base>`

### Title and Meta Tags

The `<title>` tag defines the title of the document. It is shown in the browser's title bar or in the page's tab, and it is the clickable headline in search engine results.

The `<meta>` tag is used to specify character set, page description, keywords, author of the document, and viewport settings.

## Deep Dive

### Charset and Viewport

**Charset:**
You should always define the character encoding for your document. 
```html
<meta charset="UTF-8">
```
UTF-8 is the standard character encoding for the web. It covers almost all of the characters, punctuations, and symbols in the world. If you omit this, browsers may misinterpret special characters (like emojis or accented letters), displaying them as question marks or weird boxes.

**Viewport:**
Before smartphones, web pages were designed only for computer screens. When mobile browsing started, browsers scaled down entire desktop pages to fit on tiny screens, making them unreadable.
To tell the browser to control the page's dimensions and scaling, you must include the viewport meta tag:
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
- `width=device-width`: Sets the width of the page to follow the screen-width of the device.
- `initial-scale=1.0`: Sets the initial zoom level when the page is first loaded.

### SEO Metadata

Search engines read `<meta>` tags to understand what your page is about.

```html
<meta name="description" content="A comprehensive tutorial on HTML.">
```
The `description` is often used by Google as the snippet text under your title in the search results. Keeping this accurate and concise (under 160 characters) is a massive part of SEO.

### Open Graph Metadata

When you paste a link into WhatsApp, Twitter, or iMessage, a nice card pops up with an image, a title, and a description. This is driven by Open Graph meta tags (originally created by Facebook).

```html
<meta property="og:title" content="The Ultimate HTML Guide">
<meta property="og:image" content="https://example.com/banner.jpg">
```

### Favicon

A favicon (favorite icon) is the small logo you see on browser tabs. It is linked using the `<link>` tag.

```html
<link rel="icon" type="image/x-icon" href="/images/favicon.ico">
```
Modern sites often use `.png` or `.svg` files for favicons and provide multiple sizes for different devices (like Apple Touch Icons).

## Examples

<details>
<summary><strong>Example: A Production-Ready Head Section</strong></summary>

```html
<head>
  <!-- Character Encoding (MUST be first) -->
  <meta charset="UTF-8">

  <!-- Responsive Viewport -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- Page Title (Appears in tab and Google Search) -->
  <title>Learn HTML | WebDev Mastery</title>

  <!-- SEO Metadata -->
  <meta name="description" content="Master HTML5 with this comprehensive, deep-dive tutorial.">
  <meta name="author" content="Jane Doe">

  <!-- Open Graph / Social Media Preview Cards -->
  <!-- 
    Special Attribute: property 
    - Used specifically by Open Graph instead of the standard "name" attribute.
  -->
  <meta property="og:title" content="Learn HTML from Scratch">
  <meta property="og:description" content="The ultimate guide to web development.">
  <meta property="og:image" content="https://mywebsite.com/assets/html-course-banner.jpg">
  <meta property="og:url" content="https://mywebsite.com/learn-html">

  <!-- Favicon -->
  <!-- 
    Special Attributes:
    - rel="icon": Tells the browser this is the favicon.
    - type="image/png": Specifies the MIME type of the image.
  -->
  <link rel="icon" type="image/png" href="favicon.png">

  <!-- External Stylesheet -->
  <link rel="stylesheet" href="style.css">
</head>
```

</details>
