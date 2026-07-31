---
sidebar_position: 1
---

# 13.1 Canvas

## Definitions

- **Canvas (`<canvas>`)**: An HTML element which can be used to draw graphics via scripting (usually JavaScript).
- **Context**: An object that provides the methods and properties for drawing on the canvas (e.g., `2d` or `webgl`).
- **Path**: A list of points, connected by segments of lines that can be of different shapes, curved or not, of different width and of different color.
- **Raster Graphic**: A dot matrix data structure representing a generally rectangular grid of pixels (what canvas outputs).

## Beginner Level Introduction

### Canvas Introduction

The HTML `<canvas>` element is used to draw graphics, on the fly, via JavaScript. 

The `<canvas>` element itself is only a container for graphics. You **must** use JavaScript to actually draw the graphics. Think of the `<canvas>` tag as a blank piece of paper, and JavaScript as the pen.

Canvas has several methods for drawing paths, boxes, circles, text, and adding images.

```html
<canvas id="myCanvas" width="200" height="100"></canvas>
```
By default, a canvas has no border and no content.

## Deep Dive

### Drawing Shapes and Colors

To draw on the canvas, you first need to grab the element using JavaScript and then get its **rendering context**. The `2d` context provides all the standard drawing tools.

```javascript
var c = document.getElementById("myCanvas");
var ctx = c.getContext("2d");
```

Once you have the context (`ctx`), you can set fill colors, stroke colors, and draw shapes like rectangles (`fillRect`) or complex paths using lines and arcs.

### Text and Images

You can also draw text directly onto the canvas using `fillText()` or `strokeText()`. You can set the font, alignment, and color just like you would with CSS, but it is rendered as pixels on the canvas, not as selectable HTML text.

Similarly, you can use `drawImage()` to render an existing HTML `<img>` onto the canvas, which is useful for image manipulation or building game sprites.

### Animations and Games

Because canvas allows you to draw pixels rapidly using JavaScript, it is the primary technology used for browser-based 2D HTML5 games and high-performance animations.

Animations are created by:
1. Clearing the canvas (`clearRect`).
2. Drawing the shapes in their new positions.
3. Repeating this process 60 times a second using `requestAnimationFrame`.

### Canvas vs. CSS

You can draw simple shapes (like circles and squares) using pure CSS on standard `<div>` elements. 
- **Use CSS** if the graphic is static, simple, or part of the UI layout.
- **Use Canvas** if you need complex drawing, pixel manipulation, data visualization (charts), or high-speed game rendering.

## Examples

<details>
<summary><strong>Example: Drawing a Rectangle and a Circle</strong></summary>

```html
<!-- 
  Special Attributes:
  - width / height: Sets the resolution of the canvas (not just the CSS size).
-->
<canvas id="shapesCanvas" width="200" height="200" style="border:1px solid #000000;"></canvas>

<script>
  // 1. Find the canvas element
  const c = document.getElementById("shapesCanvas");
  
  // 2. Get the 2D drawing context
  const ctx = c.getContext("2d");

  // --- Draw a Red Rectangle ---
  ctx.fillStyle = "#FF0000"; // Set the fill color
  // fillRect(x, y, width, height)
  ctx.fillRect(0, 0, 150, 75); 

  // --- Draw a Blue Circle ---
  ctx.beginPath(); // Start a new path
  // arc(x, y, radius, startAngle, endAngle)
  ctx.arc(95, 50, 40, 0, 2 * Math.PI);
  ctx.fillStyle = "blue"; // Change color
  ctx.fill(); // Fill the circle path
</script>
```

</details>

<details>
<summary><strong>Example: Drawing Text</strong></summary>

```html
<canvas id="textCanvas" width="300" height="100" style="border:1px solid #c3c3c3;"></canvas>

<script>
  const canvas = document.getElementById("textCanvas");
  const context = canvas.getContext("2d");
  
  // Set the font style
  context.font = "30px Arial";
  
  // Draw filled text
  // fillText(text, x, y)
  context.fillText("Hello World", 10, 50);
  
  // Draw outlined text
  context.font = "20px Verdana";
  context.strokeText("Outlined Text", 10, 80);
</script>
```

</details>
