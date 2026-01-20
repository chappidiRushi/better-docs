---

id: core-html

title: Core HTML Specification

sidebar_position: 1
--------------

# Core HTML Specification

> Authoritative definition of HTML semantics, parsing, DOM integration, and browser behavior

---

## HTML Living Standard (WHATWG)

```txt
Single continuously updated HTML specification maintained by WHATWG
```

<details>
<summary>Examples</summary>

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8"> <!-- defined by Living Standard -->
    <title>LS</title>
  </head>
  <body>
    <main>Hello</main>
  </body>
</html>
```

</details>

---

## Parsing Algorithm & Error Recovery

```txt
Deterministic tokenizer + tree construction with mandatory error recovery rules
```

<details>
<summary>Examples</summary>

```html
<p><div>broken

<!-- Browser output -->
<p></p>
<div>broken</div>
```

```html
<table>
  <tr><td>A<td>B
</table>
<!-- Implicit tags auto-inserted by parser -->
```

</details>

---

## Document Object Model (DOM) Fundamentals

```txt
Tree-based object representation of parsed HTML with live mutation & scripting hooks
```

<details>
<summary>Examples</summary>

```js
// Node types
Node.ELEMENT_NODE;   // 1
Node.TEXT_NODE;      // 3
Node.DOCUMENT_NODE;  // 9

// Live DOM mutation
document.body.appendChild(document.createElement('section'));
```

```js
// Traversal APIs
el.parentNode;              // Node | null
el.children;                // HTMLCollection
el.querySelectorAll('p');   // NodeList
```

</details>

---

## Content Models

```txt
Rules defining which elements are permitted as children of other elements
```

---

### Metadata Content

```txt
Elements that describe the document
```

<details>
<summary>Examples</summary>

```html
<head>
  <meta charset="utf-8">   <!-- metadata -->
  <meta name="viewport">   <!-- metadata -->
  <title>Doc</title>         <!-- metadata -->
  <link rel="stylesheet">  <!-- metadata -->
</head>
```

</details>

---

### Flow Content

```txt
Most elements allowed inside <body>
```

<details>
<summary>Examples</summary>

```html
<body>
  <section>
    <p>Text</p>
    <ul><li>Item</li></ul>
    <article>Article</article>
  </section>
</body>
```

</details>

---

### Sectioning Content

```txt
Elements that define document outline
```

<details>
<summary>Examples</summary>

```html
<section>
  <h1>Title</h1>
  <article>
    <h2>Sub</h2>
  </article>
</section>
```

</details>

---

### Phrasing Content

```txt
Inline-level text and text-level semantics
```

<details>
<summary>Examples</summary>

```html
<p>
  <strong>Bold</strong>
  <em>Em</em>
  <a href="#">Link</a>
  <span data-x="1">Span</span>
</p>
```

</details>

---

### Embedded Content

```txt
External or replaced resources
```

<details>
<summary>Examples</summary>

```html
<img src="a.png" alt="a">       <!-- image -->
<iframe src="/app"></iframe>     <!-- nested browsing context -->
<video src="v.mp4" controls></video>
<canvas width="300" height="150"></canvas>
```

</details>

---

### Interactive Content

```txt
Elements designed for user interaction
```

<details>
<summary>Examples</summary>

```html
<button type="button">Click</button>
<a href="/">Link</a>
<input type="text">   <!-- text | checkbox | radio | file | etc -->
<select>
  <option>A</option>
</select>
```

</details>

---

## Notes

* HTML spec defines **required error handling**, not best-effort
* DOM is **live** — collections update automatically
* Content models are **overlapping**, not exclusive

## Caveats

* Sectioning does **not** create accessibility landmarks by itself
* Parser behavior cannot be polyfilled
* Invalid nesting may silently alter DOM structure
