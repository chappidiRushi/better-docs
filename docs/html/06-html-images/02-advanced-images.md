---
sidebar_position: 2
---

# 6.2 Advanced Images

## Definitions

- **Image Map**: An image with clickable areas, where different areas link to different destinations.
- **Background Image**: An image applied to the background of an HTML element via CSS, rather than being placed in the content flow via an `<img>` tag.
- **Resolution Switching**: Providing multiple versions of the same image at different sizes so the browser can download the smallest one that still looks sharp on the user's screen.
- **Picture Element (`<picture>`)**: A container used to specify multiple `<source>` elements for a specific `<img>` contained in it. Allows for serving different images based on screen size or format support.
- **Lazy Loading**: A technique that defers the loading of non-critical resources (like images far down the page) at page load time. Instead, these resources are loaded at the moment of need.

## Beginner Level Introduction

### Background Images

Sometimes you don't want an image to be a piece of content, but rather a background for text or other elements. You cannot do this with the `<img>` tag. Instead, you use the CSS `background-image` property on a container element (like a `<div>` or the `<body>`).

```html
<div style="background-image: url('stars.jpg');">
  <h2>Look at the stars</h2>
</div>
```

By default, a background image will repeat itself horizontally and vertically if the container is larger than the image.

## Deep Dive

### Image Maps (`<map>` and `<area>`)

The HTML `<map>` tag defines an image map (an image with clickable areas).
The idea is that you can click on different parts of an image to trigger different actions or go to different links.

You bind the image to the map using the image's `usemap` attribute, which points to the `name` attribute of the `<map>`.

Inside the `<map>`, you define clickable `<area>` elements, specifying their shape (rect, circle, poly) and coordinates.

*(Note: Image maps are rarely used in modern web design because they are incredibly difficult to make responsive.)*

### Resolution Switching (`srcset` and `sizes`)

If you want to display the exact same image to all users, but you want to save bandwidth for mobile users by giving them a smaller file, you do not need the `<picture>` tag. You can use the `srcset` and `sizes` attributes directly on a standard `<img>` tag.

- **`srcset`**: A comma-separated list of image URLs, each with a width descriptor (e.g., `400w` meaning the image is 400 pixels wide). This tells the browser *what* images are available.
- **`sizes`**: A comma-separated list of media conditions (e.g., `(max-width: 600px) 100vw`). This tells the browser *how large* the image will be displayed on screen.

The browser looks at the `sizes` attribute, calculates the math, and automatically downloads the most appropriate image from the `srcset` list.

### The Picture Element (`<picture>`)

The `<picture>` element is a powerful tool for **Art Direction** and **Format Support**. It allows you to define multiple sources for an image, and the browser will pick the best one.

1. **Art Direction (Responsive Image Sources)**: You can serve a wide landscape image to desktop users, but serve a cropped, square portrait image to mobile users so the subject remains visible.
2. **Format Support**: You can provide a modern WebP or AVIF image to browsers that support them, while falling back to a standard JPEG for older browsers.

```html
<picture>
  <source srcset="img_avatar.webp" type="image/webp">
  <img src="img_avatar.jpg" alt="Avatar">
</picture>
```
If the browser supports WebP, it loads `img_avatar.webp`. If not, it falls back to the `<img>` tag and loads `img_avatar.jpg`.

### Lazy Loading

Loading dozens of high-resolution images on a page can make the site incredibly slow. If a user never scrolls down to the bottom of the page, downloading the images at the bottom is a waste of data and time.

HTML5 introduced native lazy loading. Simply add `loading="lazy"` to your `<img>` tag. The browser will wait to download the image until the user scrolls close to it.

## Examples

<details>
<summary><strong>Example: Resolution Switching (`srcset`)</strong></summary>

```html
<!-- 
  The browser calculates the screen width and the device pixel ratio (Retina displays).
  If the screen is 320px wide, it will download small.jpg. 
  If it's on a 4K monitor, it will download large.jpg.
-->
<img srcset="small.jpg 400w,
             medium.jpg 800w,
             large.jpg 1200w"
     sizes="(max-width: 600px) 100vw, 
            (max-width: 900px) 50vw, 
            800px"
     src="medium.jpg"
     alt="A beautiful landscape">
```

</details>

<details>
<summary><strong>Example: The Picture Element (Art Direction)</strong></summary>

```html
<!-- The <picture> element wraps <source> tags and one <img> tag -->
<picture>
  
  <!-- 
    Special Attributes:
    - media: A CSS media query. If it evaluates to true, this source is used.
    - srcset: The path to the image for this source.
  -->
  <source media="(min-width: 650px)" srcset="img_food_wide.jpg">
  
  <source media="(min-width: 465px)" srcset="img_food_square.jpg">
  
  <!-- 
    The <img> tag is required! 
    It acts as the fallback if no <source> media queries match, 
    and it is the element that actually renders the image on screen.
  -->
  <img src="img_food_mobile.jpg" alt="A plate of pasta" style="width:auto;">
</picture>
```

</details>

<details>
<summary><strong>Example: Lazy Loading Images</strong></summary>

```html
<!-- 
  Special Attribute: loading
  - "lazy": Defers loading until the image is close to entering the viewport.
  - "eager": (Default) Loads the image immediately.
-->
<img src="heavy-image-1.jpg" alt="Gallery Photo 1" width="800" height="600" loading="lazy">
<img src="heavy-image-2.jpg" alt="Gallery Photo 2" width="800" height="600" loading="lazy">
<img src="heavy-image-3.jpg" alt="Gallery Photo 3" width="800" height="600" loading="lazy">

<p>Because these images have loading="lazy", the page will load instantly, 
and the images will only download as the user scrolls down to them.</p>
```

</details>

<details>
<summary><strong>Example: Image Map (Legacy)</strong></summary>

```html
<!-- 
  Special Attribute on <img>: usemap
  - Points to the name of the <map> (must start with #).
-->
<img src="workplace.jpg" alt="Workplace" usemap="#workmap" width="400" height="379">

<!-- 
  Special Attribute on <map>: name
  - Must match the usemap value on the image.
-->
<map name="workmap">
  <!-- 
    Special Attributes on <area>:
    - shape: rect, circle, poly, default
    - coords: The coordinates defining the clickable area (x1,y1,x2,y2 for a rect).
    - href: Where the click navigates to.
  -->
  <area shape="rect" coords="34,44,270,350" alt="Computer" href="computer.html">
  <area shape="rect" coords="290,172,333,250" alt="Phone" href="phone.html">
  <area shape="circle" coords="337,300,44" alt="Coffee" href="coffee.html">
</map>
```

</details>
