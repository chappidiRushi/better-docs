---

id: advanced-html-apis

title: Advanced HTML & APIs

sidebar_position: 11
---------------

# Advanced HTML & APIs

> Built-in browser APIs exposed through HTML and the DOM for advanced interaction and capabilities

---

## HTML APIs Overview

```txt
Browser-native APIs accessible via DOM, enabling device, graphics, and interaction features
```

<details>
<summary>Examples</summary>

```txt
Common HTML-related APIs:
- Canvas API
- Geolocation API
- Drag and Drop API
- Web Storage API
- History API
```

```js
// APIs are usually exposed on window or elements
window.history;
navigator.geolocation;
```

</details>

---

## Canvas API

```txt
JavaScript API for drawing and manipulating bitmap graphics in <canvas>
```

<details>
<summary>Examples</summary>

```html
<canvas id="c" width="200" height="100"></canvas>
```

```js
const ctx = c.getContext('2d');

ctx.fillStyle = 'blue';           // string | gradient | pattern
ctx.strokeStyle = '#000';
ctx.lineWidth = 2;                // number

ctx.beginPath();
ctx.moveTo(10, 10);
ctx.lineTo(190, 90);
ctx.stroke();
```

```txt
Notes:
- Immediate-mode rendering
- State-based drawing context
```

</details>

---

## Geolocation API

```txt
Provides access to the user’s geographic location (permission-based)
```

<details>
<summary>Examples</summary>

```js
navigator.geolocation.getCurrentPosition(success, error, options);
// options:
// enableHighAccuracy: boolean
// timeout: number (ms)
// maximumAge: number (ms)
```

```js
function success(pos) {
  pos.coords.latitude;   // number
  pos.coords.longitude;  // number
}
```

```txt
Rules:
- HTTPS required
- User permission mandatory
```

</details>

---

## Drag and Drop API

```txt
Enables native drag-and-drop interactions between DOM elements
```

<details>
<summary>Examples</summary>

```html
<div draggable="true">Drag</div>
<!-- draggable: true | false -->
```

```js
el.addEventListener('dragstart', e => {
  e.dataTransfer.setData('text/plain', 'payload');
});

el.addEventListener('drop', e => {
  e.preventDefault();
  e.dataTransfer.getData('text/plain');
});
```

```txt
Events:
- dragstart
- dragover
- drop
```

</details>

---

## HTML Templates (`<template>`)

```txt
Defines inert DOM fragments for cloning at runtime
```

```html
<template id="tpl">
  <li class="item"></li>
</template>
```

<details>
<summary>Examples</summary>

```js
const tpl = document.getElementById('tpl');
const node = tpl.content.cloneNode(true);
```

```txt
Rules:
- Content not rendered
- Scripts not executed until cloned
```

</details>

---

## Notes

* APIs are capability-based and permission-gated
* Most APIs are async or event-driven
* HTML provides hooks; JS drives behavior

## Caveats

* API availability varies by browser
* Permissions impact UX
* Misuse can hurt performance or security
