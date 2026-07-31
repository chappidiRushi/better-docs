---
sidebar_position: 2
---

# 13.2 SVG

## Definitions

- **SVG (Scalable Vector Graphics)**: An XML-based vector image format for two-dimensional graphics with support for interactivity and animation.
- **Vector Graphic**: An image created directly from geometric shapes (points, lines, curves, and polygons) defined on a Cartesian plane, rather than a grid of pixels.
- **Resolution Independent**: An image that can be scaled up or down to any size without losing quality or becoming pixelated.

## Beginner Level Introduction

### SVG Introduction

SVG stands for Scalable Vector Graphics. It is used to define graphics for the Web.

Unlike JPEG, PNG, or Canvas (which are all raster/pixel-based), SVG is vector-based. This means if you zoom in on an SVG image, or print it on a massive billboard, it will remain perfectly crisp and sharp.

You can embed SVG code directly into your HTML document using the `<svg>` tag.

```html
<svg width="100" height="100">
  <circle cx="50" cy="50" r="40" stroke="green" stroke-width="4" fill="yellow" />
</svg>
```

## Deep Dive

### SVG Shapes and Paths

Inside the `<svg>` container, you define the graphics using specific XML tags. 
Common SVG shapes include:
- `<rect>`: Rectangle
- `<circle>`: Circle
- `<ellipse>`: Ellipse
- `<line>`: A straight line
- `<polyline>`: A shape made of straight lines connected together
- `<polygon>`: A closed shape made of straight lines

The most powerful SVG element is the **`<path>`**. It uses a specialized syntax in its `d` (data) attribute to draw any complex shape imaginable, combining straight lines, bezier curves, and arcs. Most vector design tools (like Adobe Illustrator or Figma) export icons as SVG `<path>` elements.

### Colors and Styling

Because SVG is essentially just XML markup, you can style SVG elements using standard CSS! You can apply classes to SVG shapes, change their colors on hover, or animate them.

Instead of `background-color` and `color`, SVG uses:
- `fill`: The color inside the shape.
- `stroke`: The color of the line drawn around the shape.
- `stroke-width`: The thickness of the line.

### SVG Animation

SVG elements can be animated in three ways:
1. **CSS Animations**: You can animate `fill`, `transform`, and `opacity` just like HTML elements.
2. **SMIL (Synchronized Multimedia Integration Language)**: SVG has native animation tags like `<animate>` and `<animateTransform>`.
3. **JavaScript**: Libraries like GSAP (GreenSock) are frequently used to create highly complex, timeline-based SVG animations.

### SVG Accessibility

Because SVGs are code, they are inherently more accessible than a `.jpg` image of a graph.
- You can add `<title>` and `<desc>` elements directly inside the `<svg>` to describe the graphic to screen readers.
- Text rendered inside an SVG (`<text>`) is selectable by the user and readable by search engines.

### SVG vs Canvas

- **SVG**: Best for logos, icons, UI elements, and simple illustrations. Highly accessible, styling is done with CSS, resolution-independent. Performance degrades if there are thousands of shapes on screen.
- **Canvas**: Best for games, complex data visualizations with thousands of particles, and pixel manipulation. Faster for high object counts, but not accessible and resolution-dependent.

## Examples

<details>
<summary><strong>Example: Drawing Basic SVG Shapes</strong></summary>

```html
<!-- The SVG container -->
<svg width="400" height="110">
  
  <!-- A Rectangle -->
  <!-- 
    Special Attributes:
    - x, y: Position from the top-left of the svg container.
    - rx, ry: Rounds the corners.
  -->
  <rect width="300" height="100" style="fill:rgb(0,0,255);stroke-width:3;stroke:rgb(0,0,0)" />
  
  <!-- A Line -->
  <!-- 
    Special Attributes:
    - x1, y1: Starting coordinates.
    - x2, y2: Ending coordinates.
  -->
  <line x1="0" y1="0" x2="200" y2="200" style="stroke:rgb(255,0,0);stroke-width:2" />
  
  <!-- A Polygon (Star) -->
  <polygon points="100,10 40,198 190,78 10,78 160,198" style="fill:lime;stroke:purple;stroke-width:5;fill-rule:nonzero;" />
  
</svg>
```

</details>

<details>
<summary><strong>Example: The SVG Path (Drawing a heart)</strong></summary>

```html
<svg width="100" height="100" viewBox="0 0 24 24">
  <!-- 
    Special Attribute: d (data)
    - M: Move to (starting point)
    - A: Draw an Arc
    - Q: Quadratic bezier curve
    - This specific path data is commonly exported from icon libraries like FontAwesome or Heroicons.
  -->
  <path d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z" 
        fill="red"/>
</svg>
```

</details>

<details>
<summary><strong>Example: Accessible and Selectable SVG Text</strong></summary>

```html
<svg width="300" height="100">
  <!-- Title for screen readers -->
  <title>A blue company logo</title>
  
  <rect width="100" height="50" fill="blue" />
  
  <!-- 
    Special SVG Element: <text>
    - The text inside is real text, not pixels. You can highlight it with your mouse.
  -->
  <text x="10" y="30" fill="white" font-family="Arial" font-size="20">TechCo</text>
</svg>
```

</details>
