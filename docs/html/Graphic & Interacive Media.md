---

id: graphics-interactive-media

title: Graphics & Interactive Media

sidebar_position: 10
--------------

# Graphics & Interactive Media

> Scriptable and declarative graphics primitives for dynamic and scalable visuals

---

## Canvas Basics

```txt
<canvas> provides a bitmap drawing surface controlled via JavaScript
```

```html
<canvas id="c" width="300" height="150"></canvas>
<!-- width | height: number (CSS pixels) -->
```

<details>
<summary>Examples</summary>

```html
<canvas id="c" width="300" height="150"></canvas>
```

```js
const ctx = document.getElementById('c').getContext('2d');
// context: 2d | webgl | webgl2 | bitmaprenderer

ctx.fillStyle = 'red';
ctx.fillRect(10, 10, 100, 50);
```

```txt
Notes:
- Canvas is immediate-mode
- No retained DOM structure
```

</details>

---

## SVG Basics

```txt
<svg> defines resolution-independent, retained-mode vector graphics
```

```html
<svg width="100" height="100"></svg>
<!-- width | height: number | % -->
```

<details>
<summary>Examples</summary>

```html
<svg width="100" height="100" viewBox="0 0 100 100">
  <circle cx="50" cy="50" r="40" fill="blue" />
</svg>
<!-- cx | cy | r: number -->
```

```html
<svg>
  <rect x="10" y="10" width="80" height="80" />
</svg>
```

```txt
Notes:
- SVG elements are part of the DOM
- Styles and events apply normally
```

</details>

---

## Notes

* Canvas suits games, pixel manipulation
* SVG suits icons, charts, scalable UI
* Both can coexist in one document

## Caveats

* Canvas is not accessible by default
* SVG performance degrades with huge DOMs
* Mixing CSS pixels and device pixels causes blur
