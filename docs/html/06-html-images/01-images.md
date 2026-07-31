---
sidebar_position: 1
---

# 6.1 HTML Images

## Definitions

- **Image Element (`<img>`)**: The HTML element used to embed an image in a web page.
- **Alternative Text (`alt`)**: Text used to describe an image if it cannot be displayed or for screen readers.
- **Aspect Ratio**: The proportional relationship between an image's width and its height.
- **Responsive Image**: An image that scales nicely to fit any browser size.
- **Image Optimization**: The process of reducing the file size of an image without sacrificing quality, to improve page load times.

## Beginner Level Introduction

### The Image Element

Images can improve the design and the appearance of a web page. 
In HTML, images are defined with the `<img>` tag. 

The `<img>` tag is empty, it contains attributes only, and does not have a closing tag.

To display an image on a page, you need to use the `src` attribute. `src` stands for "source". The value of the `src` attribute is the URL of the image you want to display.

```html
<img src="url">
```

### The `alt` Attribute

The required `alt` attribute specifies an alternate text for an image, if the image cannot be displayed. This might happen if:
- The user has a slow connection.
- The `src` URL is incorrect or the image was deleted.
- The user uses a screen reader.

```html
<img src="img_girl.jpg" alt="Girl in a jacket">
```

**Rule of thumb**: If an image is purely decorative (like a background pattern), use an empty alt attribute (`alt=""`). This tells screen readers to ignore it safely. If it contains information, provide a descriptive `alt`.

## Deep Dive

### Image Sizes (Width and Height)

You can use the `style` attribute to specify the width and height of an image in pixels, or you can use the HTML `width` and `height` attributes.

```html
<!-- Using HTML attributes (values are always in pixels) -->
<img src="img_girl.jpg" alt="Girl in a jacket" width="500" height="600">

<!-- Using inline CSS -->
<img src="img_girl.jpg" alt="Girl in a jacket" style="width:500px; height:600px;">
```

**Best Practice**: Always specify *both* the width and height attributes (even if you resize it later with CSS). If width and height are not set, the browser does not know the size of the image until it finishes downloading. This causes **Cumulative Layout Shift (CLS)**, where the text on your page suddenly jumps down to make room for the image. By providing dimensions, the browser reserves the exact space needed immediately.

### Responsive Images

A responsive image will automatically adjust to fit the size of the screen.

To make an image responsive, you typically use CSS to set the `width` to `100%` (so it scales up and down) and `height` to `auto` (to preserve the aspect ratio).

Alternatively, you can set `max-width: 100%`. This prevents the image from scaling *larger* than its original size, but allows it to scale down if the screen is smaller.

### Image Optimization

Web images account for the majority of downloaded bytes on a typical web page.
- **Format**: Use WebP or AVIF instead of JPEG or PNG whenever possible. They offer superior compression.
- **Size**: Don't upload a 4000px wide photo if it will only ever be displayed at 500px wide. Resize the actual file before uploading.

## Examples

<details>
<summary><strong>Example: The Standard Image Tag</strong></summary>

```html
<!-- 
  Special Attributes:
  - src: The path to the image. (Can be relative or absolute).
  - alt: Descriptive text for accessibility.
  - width / height: Explicit dimensions to prevent layout shift.
-->
<img src="images/company-logo.webp" 
     alt="Example Company Logo" 
     width="250" 
     height="100">
```

</details>

<details>
<summary><strong>Example: Responsive Image Styling</strong></summary>

```html
<style>
  .responsive-img {
    /* The image will never be wider than its container */
    max-width: 100%;
    
    /* The height will adjust automatically to maintain aspect ratio */
    height: auto; 
  }
</style>

<!-- 
  By applying this class, the image will look great on a 4K monitor 
  and a tiny smartphone screen.
-->
<img src="large-photo.jpg" alt="A beautiful landscape" class="responsive-img" width="1920" height="1080">
```

</details>

<details>
<summary><strong>Example: Decorative Images vs Informative Images</strong></summary>

```html
<!-- Informative: Needs alt text -->
<img src="chart.png" alt="Bar chart showing a 20% increase in sales" width="400" height="300">

<!-- 
  Decorative: Use empty alt text. 
  If you omit the alt attribute entirely, some screen readers will 
  read the filename out loud, which is annoying ("image_divider_line_final_v2.png").
  alt="" prevents this.
-->
<img src="divider.png" alt="" width="100%" height="10">
```

</details>
