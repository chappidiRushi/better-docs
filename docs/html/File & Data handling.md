---

id: file-data-handling

title: File & Data Handling

sidebar_position: 8
-----------------

# File & Data Handling

> Referencing external resources and embedding custom data in HTML documents

---

## File Paths (Relative vs Absolute)

```txt
URLs used to locate external resources relative to document or origin
```

```html
<img src="/img/logo.png">      <!-- absolute path (origin-based) -->
<img src="img/logo.png">       <!-- relative to current directory -->
<img src="../img/logo.png">    <!-- relative to parent directory -->
```

<details>
<summary>Examples</summary>

```txt
Assume document URL:
https://example.com/docs/page.html
```

```html
<a href="/about">About</a>
<!-- resolves to https://example.com/about -->

<a href="about">About</a>
<!-- resolves to https://example.com/docs/about -->

<a href="../about">About</a>
<!-- resolves to https://example.com/about -->
```

```html
<img src="https://cdn.example.com/img.png"> <!-- absolute URL -->
```

```txt
Rules:
- Leading / = root-relative
- No / = document-relative
- URLs are resolved before request
```

</details>

---

## Data Attributes (`data-*`)

```txt
Custom attributes for embedding non-visible, script-accessible data
```

```html
<div data-id="1" data-user-name="admin"></div>
```

<details>
<summary>Examples</summary>

```html
<button data-action="delete" data-confirm="true">X</button>
<!-- data-* names: kebab-case -->
<!-- values: always strings -->
```

```js
const btn = document.querySelector('button');

btn.dataset.action;     // "delete"
btn.dataset.confirm;    // "true"
btn.dataset.userName;   // camelCase mapping
```

```txt
Rules:
- data-* ignored by browser semantics
- Exposed via HTMLElement.dataset
- No validation or type coercion
```

</details>

---

## Notes

* All URLs are resolved against base URL
* data-* is HTML-only metadata (not CSS/ARIA)
* Prefer structured data APIs when semantics matter

## Caveats

* Invalid paths fail silently
* data-* is not a replacement for state management
* Large data blobs in HTML hurt performance
