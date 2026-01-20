---

id: text-content-elements

title: Text & Content Elements

sidebar_position: 3
---------------

# Text & Content Elements

> Core textual semantics, inline formatting, and character encoding in HTML

---

## Headings (`<h1>`–`<h6>`)

```txt
Section titles defining document structure and navigation hierarchy
```

```html
<h1>Main</h1> <!-- highest rank -->
<h2>Sub</h2>
<h3>Section</h3>
<h4>Subsection</h4>
<h5>Minor</h5>
<h6>Lowest</h6>
```

<details>
<summary>Examples</summary>

```html
<h1>Page Title</h1>
<section>
  <h2>Features</h2>
  <h3>Feature A</h3>
</section>
```

```txt
Rules:
- Order matters more than numeric level
- Do not skip for styling
```

</details>

---

## Paragraphs (`"<p>"`)

```txt
Block-level container for phrasing content
```

```html
<p>Text content</p>
```

<details>
<summary>Examples</summary>

```html
<p>
  This is a paragraph with <a href="#">inline elements</a>.
</p>
```

```html
<!-- Invalid nesting (auto-corrected) -->
<p><div>broken</div></p>
```

```txt
Notes:
- Cannot contain block-level elements
- Implicitly closed by parser
```

</details>

---

## Text Formatting

```txt
Inline elements that add emphasis or meaning without changing structure
```

```html
<strong>Important</strong>   <!-- strong importance -->
<em>Emphasis</em>             <!-- stress emphasis -->
<b>Bold</b>                   <!-- stylistic offset -->
<i>Italic</i>                 <!-- alternate voice -->
<mark>Highlight</mark>        <!-- relevance -->
<small>Fine print</small>     <!-- side comments -->
<code>code</code>             <!-- code fragment -->
```

<details>
<summary>Examples</summary>

```html
<p>
  <strong>Error:</strong>
  <code>404</code>
  <small>(not found)</small>
</p>
```

```txt
Semantics:
- <strong>/<em> convey meaning
- <b>/<i> are purely presentational
```

</details>

---

## HTML Entities & Special Characters

```txt
Escaped sequences representing reserved or non-ASCII characters
```

```html
&amp;   <!-- & -->
&lt;   <!-- < -->
&gt;   <!-- > -->
&quot; <!-- " -->
&apos; <!-- ' -->
&nbsp;<!-- non-breaking space -->
```

<details>
<summary>Examples</summary>

```html
<p>5 &lt; 10 &amp;&amp; 10 &gt; 5</p>
<p>Copyright &copy; 2026</p>
<p>Price: 10&nbsp;USD</p>
```

```txt
Forms:
- Named (&copy;)
- Numeric (&#169; | &#xA9;)
```

</details>

---

## Notes

* Headings are primary navigation for assistive tech
* Formatting elements are phrasing content
* UTF-8 minimizes entity usage

## Caveats

* Using headings for styling breaks accessibility
* Excessive entities reduce readability
* b/i do not imply importance
