---
sidebar_position: 4
---

# 18.4 Browser Support

## Definitions

- **Browser Support**: Whether a specific web browser (Chrome, Safari, Firefox, Edge) is capable of understanding and correctly rendering a specific HTML tag, CSS property, or JavaScript API.
- **Polyfill**: A piece of code (usually JavaScript on the Web) used to provide modern functionality on older browsers that do not natively support it.
- **Can I Use (caniuse.com)**: The industry-standard website for checking browser support for web technologies.

## Beginner Level Introduction

HTML is an evolving standard. New tags and APIs are added constantly. However, browsers are built by different companies (Google, Apple, Mozilla, Microsoft), and they do not all update at the same time or adopt new standards simultaneously.

Just because a new tag like `<dialog>` is officially added to the HTML specification does not mean you can use it immediately. If 20% of your users are using an older version of Safari that doesn't understand the `<dialog>` tag, those users will experience a broken website.

## Deep Dive

### Checking Compatibility

Before using a cutting-edge HTML5 feature, a new CSS layout technique (like Subgrid), or a new JavaScript API, professional web developers always check its compatibility.

The primary resource for this is **[caniuse.com](https://caniuse.com)**. You type in the feature you want to use, and it tells you exactly which browser versions support it, and what percentage of global users have those versions installed.

### Handling Unsupported Features

When an HTML tag is not supported by a browser, the browser's default behavior is to treat it as a generic, unstyled inline element (essentially treating it like a `<span>` with no semantic meaning). 
It will **not** throw an error and crash the page, but the layout might break, or the native functionality (like a date picker popping up) will simply not happen.

**Strategies for dealing with lack of support:**

1. **Graceful Degradation**: Build the modern feature, but ensure the site is still usable (even if it's uglier or requires more clicks) on older browsers. For example, if `<input type="date">` isn't supported, it falls back to a regular text input.
2. **Polyfills**: If a feature is missing, you load a JavaScript library that artificially recreates the missing HTML5 feature.
3. **Feature Detection**: Use JavaScript to check if the browser supports a feature before trying to use it.

## Examples

<details>
<summary><strong>Example: Feature Detection with JavaScript</strong></summary>

```html
<script>
  // Checking if the browser supports the HTML5 Geolocation API
  if ("geolocation" in navigator) {
    // Safe to use
    console.log("Geolocation is supported!");
  } else {
    // Provide a fallback for older browsers
    console.log("Geolocation is NOT supported. Please enter your zip code manually.");
  }
  
  // Checking if the browser supports the <dialog> element
  if (typeof HTMLDialogElement === 'function') {
      console.log("The <dialog> element is supported.");
  }
</script>
```

</details>

<details>
<summary><strong>Example: HTML Fallbacks</strong></summary>

The `<audio>`, `<video>`, and `<canvas>` elements were designed with a built-in fallback mechanism. Any text you place *between* the opening and closing tags will only be displayed if the browser does not understand the tag.

```html
<video src="movie.mp4" controls>
  <!-- 
    This text is completely ignored by modern browsers. 
    It is ONLY seen by ancient browsers (like IE8).
  -->
  Sorry, your browser doesn't support embedded videos. 
  <a href="movie.mp4">Download the video here</a> instead.
</video>
```

</details>
