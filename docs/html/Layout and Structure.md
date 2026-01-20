---

id: layout-structure

title: Layout & Structure

sidebar_position: 6
--------------


# Layout & Structure

> Structural grouping, layout primitives, and semantic containers in HTML

---

## Divs & Spans

```txt
Generic containers with no inherent semantic meaning
```

```html
<div></div>   <!-- block-level generic container -->
<span></span> <!-- inline generic container -->
```

<details>
<summary>Examples</summary>

```html
<div class="card">
  <span class="label">New</span>
</div>
```

```txt
Rules:
- Use only when no semantic element fits
- Meaning is entirely CSS/JS-driven
```

</details>

---

## Block vs Inline Elements

```txt
Display behavior that affects document flow
```

```txt
Block: starts on new line, fills available width
Inline: flows within text, no line break
```

<details>
<summary>Examples</summary>

```html
<div>Block</div>
<p>Paragraph is block</p>

<span>Inline</span>
<a href="#">Link</a>
```

```txt
Notes:
- CSS display can override default behavior
- Semantics are independent of display
```

</details>

---

## Semantic HTML Elements

```txt
Elements that convey meaning and document structure
```

<details>
<summary>Examples</summary>

```html
<header></header>
<nav></nav>
<main></main>
<section></section>
<article></article>
<footer></footer>
```

```txt
Benefits:
- Accessibility landmarks
- SEO signals
- Maintainability
```

</details>

---

## `<header>`

```txt
Introductory content or navigational aids for a section
```

<details>
<summary>Examples</summary>

```html
<header>
  <h1>Site</h1>
</header>
```

```txt
Rules:
- Can appear multiple times
- Cannot contain <main>, <footer>
```

</details>

---

## `<nav>`

```txt
Section containing major navigation links
```

<details>
<summary>Examples</summary>

```html
<nav>
  <a href="/">Home</a>
  <a href="/docs">Docs</a>
</nav>
```

```txt
Notes:
- Not all links require <nav>
```

</details>

---

## `<main>`

```txt
Primary content unique to the document
```

<details>
<summary>Examples</summary>

```html
<main>
  <article>Content</article>
</main>
```

```txt
Rules:
- Only one per document
- Cannot be hidden
```

</details>

---

## `<section>`

```txt
Thematic grouping of content with a heading
```

<details>
<summary>Examples</summary>

```html
<section>
  <h2>Features</h2>
  <p>Details</p>
</section>
```

```txt
Rules:
- Should have a heading
- Not for styling only
```

</details>

---

## `<article>`

```txt
Self-contained, reusable content unit
```

<details>
<summary>Examples</summary>

```html
<article>
  <h2>Post</h2>
  <p>Text</p>
</article>
```

```txt
Examples:
- blog post
- comment
- news item
```

</details>

---

## `<footer>`

```txt
Footer for its nearest sectioning ancestor
```

<details>
<summary>Examples</summary>

```html
<footer>
  <small>&copy; 2026</small>
</footer>
```

```txt
Rules:
- Can contain metadata, links
- Cannot contain <main>, <header>
```

</details>

---

## Notes

* Semantics > div-based layout
* Landmarks improve navigation for AT
* Structure is independent of CSS

## Caveats

* Misusing semantics harms accessibility
* Do not replace div blindly
* Multiple landmarks can confuse if misused
