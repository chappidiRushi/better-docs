---
sidebar_position: 1
---

# 4.1 HTML Colors

## Definitions

- **Color Value**: The specific code or name used to tell a browser what color to display.
- **RGB (Red, Green, Blue)**: A color model based on the mixing of red, green, and blue light.
- **HEX (Hexadecimal)**: A 6-digit alphanumeric code used to represent RGB colors.
- **HSL (Hue, Saturation, Lightness)**: A cylindrical-coordinate representation of colors that is often more intuitive for humans than RGB.
- **Alpha Channel**: Represents the opacity (transparency) of a color.
- **Color Contrast**: The difference in light between the font (or foreground) and its background.

## Beginner Level Introduction

Colors in HTML are used to style text, backgrounds, borders, and other elements. You specify colors using CSS properties like `color` (for text) and `background-color` (for backgrounds).

HTML/CSS supports several ways to define colors.

### Color Names

The easiest way to set a color is by using a standard color name. Modern browsers support 140 standard color names.

Examples: `Red`, `Blue`, `Green`, `Tomato`, `DodgerBlue`, `MediumSeaGreen`, `Gray`, `SlateBlue`, `Violet`, `LightGray`.

```html
<h1 style="background-color: DodgerBlue;">Hello World</h1>
<p style="color: Tomato;">This text is Tomato colored.</p>
```

## Deep Dive

While color names are easy, they are extremely limited. In professional web development, we use specific color codes to get exact shades.

### RGB Colors

An RGB color value specifies the intensity of Red, Green, and Blue on a scale from 0 to 255.
- `rgb(0, 0, 0)` is black (zero light).
- `rgb(255, 255, 255)` is white (maximum light).
- `rgb(255, 0, 0)` is pure red.

### HEX Colors

A HEX color is specified with: `#RRGGBB`, where the RR (red), GG (green), and BB (blue) hexadecimal integers specify the components of the color.
- `#000000` is black.
- `#FFFFFF` is white.
- `#FF0000` is pure red.

Hex codes are the most common way developers copy and paste colors from design tools like Figma or Photoshop.

### HSL Colors

HSL stands for Hue, Saturation, and Lightness.
- **Hue**: A degree on the color wheel from 0 to 360. 0 is red, 120 is green, 240 is blue.
- **Saturation**: A percentage value; 0% means a shade of gray, and 100% is the full color.
- **Lightness**: A percentage; 0% is black, 50% is neither light nor dark, 100% is white.

HSL is favored by developers when they need to dynamically calculate lighter or darker shades of a base color.

### RGBA and HSLA (Transparency)

You can add an **Alpha** channel to RGB and HSL to control transparency. The alpha parameter is a number between 0.0 (fully transparent) and 1.0 (fully opaque).

- `rgba(255, 0, 0, 0.5)` - 50% transparent red.
- `hsla(120, 100%, 50%, 0.3)` - 30% opaque green.
*(Note: You can also use 8-digit HEX codes for transparency, like `#FF000080` for 50% red, but it is less readable).*

### Color Accessibility (Contrast Ratios)

When choosing colors, you must ensure that there is enough contrast between the text color and the background color so that visually impaired users (or users in bright sunlight) can read it.
The Web Content Accessibility Guidelines (WCAG) require a contrast ratio of at least **4.5:1** for normal text and **3:1** for large text.

## Examples

<details>
<summary><strong>Example: Applying Different Color Formats</strong></summary>

```html
<!-- Using Color Names -->
<!-- 
  Special Attribute: style
  - Used for inline CSS styling.
-->
<div style="background-color: Tomato; color: white; padding: 10px;">
  Tomato Background
</div>

<!-- Using RGB -->
<div style="background-color: rgb(255, 99, 71); color: white; padding: 10px;">
  rgb(255, 99, 71) Background (Same as Tomato)
</div>

<!-- Using HEX -->
<div style="background-color: #ff6347; color: white; padding: 10px;">
  #ff6347 Background (Same as Tomato)
</div>

<!-- Using HSL -->
<div style="background-color: hsl(9, 100%, 64%); color: white; padding: 10px;">
  hsl(9, 100%, 64%) Background (Same as Tomato)
</div>
```

</details>

<details>
<summary><strong>Example: Using Transparency (RGBA)</strong></summary>

```html
<!-- A solid background image -->
<div style="background-image: url('background.jpg'); padding: 50px;">

  <!-- 
    A child div with a semi-transparent black background.
    This creates a "glass" or "overlay" effect, making the white text readable
    against a busy background image.
  -->
  <div style="background-color: rgba(0, 0, 0, 0.6); color: white; padding: 20px;">
    <h2>Readable Title</h2>
    <p>This text is highly visible because of the RGBA background overlay.</p>
  </div>

</div>
```

</details>
