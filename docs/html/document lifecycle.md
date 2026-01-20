---

id: document-lifecycle

title: Document Lifecycle & Loading

sidebar_position: 2
--------------
# Document Lifecycle & Loading

> How browsers parse, execute, render, and load documents

---

## Browser Rendering Pipeline

```txt
HTML → DOM → CSSOM → Render Tree → Layout → Paint → Composite
```

<details>
<summary>Examples</summary>

```txt
HTML parsed        → DOM nodes
CSS parsed         → CSSOM rules
DOM + CSSOM        → Render Tree
Layout             → geometry (reflow)
Paint              → pixels
Composite          → layers on screen
```

</details>

---

## Critical Rendering Path

```txt
Minimal sequence of resources required to render initial content
```

<details>
<summary>Examples</summary>

```html
<head>
  <link rel="stylesheet" href="style.css"> <!-- render‑blocking -->
  <script src="app.js"></script>             <!-- parser‑blocking -->
</head>
```

```txt
Optimize by:
- inline critical CSS
- defer non‑critical JS
- reduce blocking requests
```

</details>

---

## HTML Parsing vs Script Execution

```txt
Parser pauses on synchronous scripts; resumes after execution
```

<details>
<summary>Examples</summary>

```html
<p>before</p>
<script>
  document.body.append('script'); // DOM available so far
</script>
<p>after</p>
```

```html
<script src="block.js"></script> <!-- blocks parsing -->
```

</details>

---

## Script Loading: normal / defer / async / module

```txt
Execution timing and dependency model for scripts
```

```html
<script src="a.js"></script>                     <!-- parser‑blocking -->
<script defer src="b.js"></script>              <!-- after DOM, ordered -->
<script async src="c.js"></script>              <!-- ASAP, unordered -->
<script type="module" src="m.js"></script>    <!-- deferred, scoped -->
```

<details>
<summary>Examples</summary>

```html
<!-- defer: preserves order -->
<script defer src="1.js"></script>
<script defer src="2.js"></script>

<!-- async: no order guarantee -->
<script async src="a.js"></script>
<script async src="b.js"></script>

<!-- module -->
<script type="module">
  import x from './x.js'; // static, deferred
</script>
```

</details>

---

## Preload Scanner

```txt
Speculative parser that fetches resources ahead of main parser
```

<details>
<summary>Examples</summary>

```html
<!-- discovered before DOM is built -->
<link rel="stylesheet" href="style.css">
<script src="app.js"></script>
<img src="hero.png">
```

```html
<!-- explicit preload -->
<link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin>
<!-- as: script | style | image | font | fetch -->
```

</details>

---

## Notes

* Rendering and parsing are **interleaved**
* `defer` == safe default for most scripts
* Modules are **deferred by default**

## Caveats

* `async` can execute before DOM is usable
* CSS blocks rendering but not DOM parsing
* Preload does **not** guarantee execution order
