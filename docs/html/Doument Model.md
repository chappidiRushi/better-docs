---

id: html-document-model
title: HTML Document Model
sidebar_position: 5
-------------------

# HTML Document Model

> How browsers turn bytes into a structured, live document tree.

---

## 1. The HTML Document Lifecycle

**Definition:** The ordered phases a document goes through from request to interactive and complete states.

Lifecycle phases:

* Navigation & fetch
* Parsing (HTML → DOM)
* Subresource discovery (CSS, JS, images)
* DOMContentLoaded
* Render-ready
* Load event
* Idle / interactive updates

Observable states:

* `document.readyState`: `loading | interactive | complete`

<details>
<summary>Example</summary>

```html
<script>
  document.addEventListener('DOMContentLoaded', () => {
    // DOM built, subresources may still load
  });

  window.addEventListener('load', () => {
    // All subresources loaded
  });
</script>
```

</details>

---

## 2. Document Types (`<!DOCTYPE>`) and Standards Mode

**Definition:** The doctype selects the browser's parsing and rendering mode.

Modern doctype:

* `<!DOCTYPE html>` → Standards Mode

Effects:

* Enables full CSS box model
* Activates modern layout algorithms
* Disables legacy error quirks

Doctype characteristics:

* Case-insensitive
* Must appear before `<html>`
* Not part of the DOM tree

<details>
<summary>Example</summary>

```html
<!DOCTYPE html>
<html lang="en">
  <head></head>
  <body></body>
</html>
```

</details>

---

## 3. Quirks Mode and Almost Standards Mode

**Definition:** Legacy compatibility modes triggered by specific doctypes.

Modes:

* Standards Mode
* Almost Standards Mode
* Quirks Mode

Triggers:

* Missing doctype → Quirks
* Legacy doctypes → Quirks / Almost Standards

Key differences:

* Box model behavior
* Table layout handling
* CSS sizing inconsistencies

<details>
<summary>Example</summary>

```html
<!-- Triggers Quirks Mode -->
<html>
  <body>Legacy layout</body>
</html>
```

</details>

---

## 4. Document Tree Construction

**Definition:** HTML tokens are incrementally converted into a DOM tree using insertion modes.

Core concepts:

* Streaming parser
* Insertion modes (`in body`, `in head`, etc.)
* Error recovery rules
* Foster parenting (tables)

Properties:

* Tree mutates during parsing
* Scripts can observe partial DOM

<details>
<summary>Example</summary>

```html
<p><div>A</p>

<!-- Parsed DOM -->
<p></p>
<div>A</div>
```

</details>

---

## 5. The DOM vs Source Markup

**Definition:** Source markup is static text; DOM is a live, normalized object graph.

Differences:

* DOM may differ structurally from source
* Whitespace normalization
* Implicit elements inserted
* Attribute defaults applied

Implication:

* `innerHTML` ≠ original source

<details>
<summary>Example</summary>

```html
<!-- Source -->
<table>
  <tr><td>A</td></tr>
</table>

<!-- DOM auto-inserts <tbody> -->
```

</details>

---

## 6. Node Types and Tree Relationships

**Definition:** DOM is composed of nodes connected via parent/child/sibling relationships.

Primary node types:

* Document (9)
* Element (1)
* Text (3)
* Comment (8)
* DocumentFragment (11)

Tree relationships:

* `parentNode`
* `childNodes`
* `firstChild / lastChild`
* `previousSibling / nextSibling`

<details>
<summary>Example</summary>

```html
<div><!--c-->A</div>
```

```js
const div = document.querySelector('div');
// div.childNodes:
// [ Comment, Text ]
```

</details>

---

## Mental Model (Interview One-Liner)

> **The HTML document model is a streaming, error-tolerant process that constructs a live DOM tree whose structure may differ from source markup and evolves throughout the page lifecycle.**
