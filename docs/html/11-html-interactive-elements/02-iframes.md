---
sidebar_position: 2
---

# 11.2 Iframes

## Definitions

- **Iframe (Inline Frame)**: An HTML element that loads another HTML document within the current document.
- **Cross-Origin**: Refers to a situation where the webpage and the iframe's content come from different domains (e.g., your site loading a YouTube video).
- **Sandbox**: A security mechanism that restricts the actions an iframe can perform, preventing malicious code from executing or hijacking the parent page.

## Beginner Level Introduction

### Embedding Content

The `<iframe>` tag specifies an inline frame. 

An inline frame is used to embed another document within the current HTML document. You have likely seen iframes used constantly across the web without realizing it. 
Common uses include:
- Embedding YouTube or Vimeo videos.
- Embedding Google Maps.
- Embedding advertising banners.
- Embedding social media widgets (like a Twitter feed).

```html
<iframe src="https://www.example.com"></iframe>
```
The `src` attribute is required. It points to the URL of the page you want to embed.

*(Note: Many popular websites, like google.com or facebook.com, send specific HTTP headers that block other websites from embedding them in an iframe for security reasons).*

## Deep Dive

### iframe Attributes

To make an iframe look good, you need to control its size and borders.

- `width` and `height`: Specify the size of the iframe in pixels.
- `title`: It is **highly recommended** to include a `title` attribute for accessibility. Screen readers use this to identify the content of the iframe.
- `name`: Specifies a name for the iframe, which can be used as the `target` for links (`<a>` tags). If a link targets the iframe, the link's destination will open *inside* the iframe.

```html
<iframe src="demo.html" width="500" height="200" title="A demo page"></iframe>
```

*(Note: In the past, developers used `frameborder="0"` to remove the border around the iframe. This is now obsolete in HTML5. Use CSS `border: none;` instead).*

### Security and Sandbox

Embedding external content inherently carries risks. If the embedded site contains malicious JavaScript, it could potentially attempt to read data from your users.

HTML5 introduced the `sandbox` attribute for iframes. When you add the `sandbox` attribute, it applies a strict set of restrictions on the iframe's content:
- It cannot run JavaScript.
- It cannot submit forms.
- It cannot open new windows/popups.

```html
<iframe src="https://example.com" sandbox></iframe>
```

You can selectively lift these restrictions by adding specific values to the sandbox attribute (e.g., `sandbox="allow-scripts allow-forms"`).

### Responsive Iframes

Making an iframe responsive (so it scales perfectly on mobile devices while maintaining its aspect ratio, like a 16:9 YouTube video) is notoriously tricky. You cannot simply set `width="100%"` and `height="auto"`.

The standard CSS hack involves wrapping the iframe in a container `<div>` with a specific `padding-top` percentage, and setting the iframe to `position: absolute`.

## Examples

<details>
<summary><strong>Example: Standard YouTube Embed</strong></summary>

```html
<!-- 
  This is the exact code YouTube provides when you click "Share -> Embed" 
-->
<iframe 
  width="560" 
  height="315" 
  src="https://www.youtube.com/embed/dQw4w9WgXcQ" 
  title="YouTube video player" 
  style="border: none;"
  /* 
    Special Attributes:
    - allow: Specifies a Feature Policy (what hardware/APIs the iframe can access).
    - allowfullscreen: A boolean allowing the video to be opened in full screen.
  */
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
  allowfullscreen>
</iframe>
```

</details>

<details>
<summary><strong>Example: Using an iframe as a Link Target</strong></summary>

```html
<!-- 
  Special Attribute on <iframe>: name
  - We name the iframe "my_iframe".
-->
<iframe src="page1.html" name="my_iframe" width="100%" height="300px" title="Target Frame"></iframe>

<p>
  <!-- 
    Special Attribute on <a>: target
    - Because the target matches the iframe's name, clicking this link 
      will load page2.html INSIDE the iframe above, not the main window.
  -->
  <a href="page2.html" target="my_iframe">Click to load Page 2 inside the frame</a>
</p>
```

</details>

<details>
<summary><strong>Example: A Responsive 16:9 Iframe Wrapper</strong></summary>

```html
<style>
  /* The container */
  .responsive-iframe-container {
    position: relative;
    width: 100%;
    /* 56.25% is the aspect ratio of 16:9 (9 divided by 16) */
    padding-bottom: 56.25%; 
    height: 0;
  }

  /* The iframe inside the container */
  .responsive-iframe-container iframe {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border: 0;
  }
</style>

<div class="responsive-iframe-container">
  <iframe src="https://www.youtube.com/embed/tgbNymZ7vqY" title="Responsive Video"></iframe>
</div>
```

</details>
