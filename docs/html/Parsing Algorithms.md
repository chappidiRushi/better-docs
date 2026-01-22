---

id: html-parsing-algorithms
title: Parsing Algorithms
sidebar_position: 6
-------------------

# Parsing Algorithms

> HTML parsing is a **state machine–driven, error-tolerant streaming algorithm**, not a generic grammar parse.

---

## 1. Tokenization Algorithm

**Definition:** Converts a byte-decoded character stream into a stream of tokens using a deterministic state machine.

Key properties:

* Input: Unicode code points
* Output: Tokens (not nodes)
* Driven by insertion states
* Context-sensitive (depends on previous tokens)

Token types:

* Doctype
* Start tag (name, attributes)
* End tag
* Comment
* Character
* EOF

Special states:

* Data
* Tag open
* Attribute name/value
* RawText / RCDATA (script, style, textarea, title)

<details>
<summary>Example</summary>

```html
<div id="x">A</div>
```

```text
StartTag(div, id="x")
Character(A)
EndTag(div)
```

</details>

---

## 2. Tree Construction Algorithm

**Definition:** Consumes tokens and mutates the DOM tree according to the current insertion mode.

Core mechanics:

* Stack of open elements
* Insertion modes (`initial`, `before html`, `in head`, `in body`, etc.)
* Active formatting elements list

Guarantees:

* DOM is always produced
* Invalid markup is normalized

<details>
<summary>Example</summary>

```html
<b><i>A</b>B</i>
```

```text
Parser reconstructs formatting to maintain tree integrity
```

</details>

---

## 3. Error Handling and Error Recovery

**Definition:** HTML defines explicit recovery rules instead of throwing parse errors.

Principles:

* Never abort parsing
* Errors are silent and recoverable
* Browser behavior must be deterministic

Common recoveries:

* Ignored unexpected end tags
* Implicit element creation
* Tag reparenting

<details>
<summary>Example</summary>

```html
<p><div>A</p>
```

```text
<p></p>
<div>A</div>
```

</details>

---

## 4. Foster Parenting

**Definition:** Special tree correction when parsing table-related content.

Applies when:

* Non-table elements appear inside table contexts

Behavior:

* Nodes are reparented outside the table
* Prevents illegal DOM structures

<details>
<summary>Example</summary>

```html
<table>
  <p>A</p>
</table>
```

```text
<p>A</p>
<table></table>
```

</details>

---

## 5. Implied Tags and Omitted End Tags

**Definition:** The parser auto-inserts or closes elements based on content model rules.

Implied elements:

* `<html>`
* `<head>`
* `<body>`

Omitted end tags:

* `<li>`
* `<p>`
* `<td>`
* `<option>`

<details>
<summary>Example</summary>

```html
<ul>
  <li>A
  <li>B
</ul>
```

```text
<li>A</li>
<li>B</li>
```

</details>

---

## 6. Foreign Content (SVG & MathML Integration)

**Definition:** HTML parser switches namespaces when encountering foreign elements.

Namespaces:

* HTML
* SVG
* MathML

Rules:

* Case sensitivity changes
* Attribute name adjustments
* Exit foreign mode on integration points

<details>
<summary>Example</summary>

```html
<svg>
  <foreignObject>
    <div>HTML inside SVG</div>
  </foreignObject>
</svg>
```

</details>

---

## 7. Script Execution During Parsing

**Definition:** Script elements can synchronously pause parsing.

Execution rules:

* Inline scripts execute immediately
* External scripts block unless `defer` / `async`
* DOM is partially constructed

Side effects:

* DOM mutation during parse
* Parser re-entry

<details>
<summary>Example</summary>

```html
<script>
  document.write('<p>Injected</p>');
</script>
```

</details>

---

## 8. Blocking vs Non-Blocking Parsing

**Definition:** Certain resources affect parser progression.

Blocking:

* `<script>` (classic)
* CSSOM construction (render-blocking)

Non-blocking:

* `<script async>`
* `<script defer>`
* Preload scanner

<details>
<summary>Example</summary>

```html
<script src="a.js"></script>        <!-- blocks -->
<script defer src="b.js"></script>  <!-- non-blocking -->
<script async src="c.js"></script>  <!-- non-blocking, unordered -->
```

</details>

---

## Mental Model (Interview One-Liner)

> **HTML parsing is a streaming, state-machine–driven algorithm that guarantees DOM construction via deterministic error recovery, even for invalid markup, while interleaving script execution.**
