---

id: html-foundations

title: HTML Foundations

sidebar_position: 1
-----------

# HTML Foundations

> Core concepts behind HTML, how browsers interpret it, and how documents are structured

---

## What HTML Is & How the Web Works

```txt
HTML is a declarative markup language interpreted by browsers to create documents over HTTP
```

<details>
<summary>Examples</summary>

```txt
Request  → GET /index.html
Response → text/html
Browser  → parse → DOM → render
```

```html
<!doctype html> <!-- signals standards mode -->
<html>
  <body>Hello Web</body>
</html>
```

</details>

---

## HTML Tags & Elements

```txt
Tags define elements; elements are nodes in the DOM tree
```

```html
<tag>content</tag>        <!-- normal element -->
<empty />                <!-- void element (syntax error in HTML) -->
<img>                     <!-- void element (HTML correct) -->
```

<details>
<summary>Examples</summary>

```html
<p>Paragraph</p>          <!-- start + end tag -->
<br>                      <!-- void element -->

<section>
  <article>Content</article>
</section>
```

```js
// DOM representation
document.querySelector('p') instanceof HTMLElement; // true
```

</details>

---

## Attributes & Attribute Values

```txt
Name–value pairs that modify element behavior or semantics
```

```html
<element attr="value"></element>
```

<details>
<summary>Examples</summary>

```html
<a href="/" target="_blank" rel="noopener">Link</a>
<!-- href: URL -->
<!-- target: _self | _blank | _parent | _top -->
<!-- rel: space-separated tokens -->
```

```html
<input type="checkbox" checked disabled>
<!-- boolean attributes: present | absent -->
```

```html
<div data-id="1" data-flag="true"></div>
<!-- data-* → string values, exposed via dataset -->
```

```js
div.dataset.id;    // "1" (string)
div.dataset.flag;  // "true"
```

</details>

---

## Comments

```txt
Ignored by parser; not exposed to DOM APIs
```

<details>
<summary>Examples</summary>

```html
<!-- single-line comment -->

<!--
  multi-line
  comment
-->
```

```html
<!-- invalid -->
<!-- -- broken -- -->
```

```js
// DOM still contains Comment nodes
Node.COMMENT_NODE; // 8
```

</details>

---

## Notes

* HTML is **error-tolerant by design**
* Void elements must not have end tags
* Attribute values are strings unless specified

## Caveats

* Self-closing syntax is ignored in HTML
* Comments may leak sensitive data in source
* Invalid markup is auto-corrected silently
