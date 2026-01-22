---

id: html-as-a-platform
title: HTML as a Platform
sidebar_position: 4
-------------------

# HTML as a Platform

> HTML is a **declarative application platform**, not just a markup language.

---

## 1. What HTML Is (and Is Not)

**Definition:** HTML is a declarative language that defines **document structure**, **semantics**, and **integration points** for the Web Platform.

HTML **is**:

* A tree construction language (DOM-centric)
* A semantic contract between authors, browsers, assistive tech, search engines
* A host language for CSS, JS, and Web APIs

HTML **is not**:

* A programming language (no control flow, no state)
* A presentation language (delegated to CSS)
* A data serialization format (JSON, XML)

<details>
<summary>Example</summary>

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Platform</title>
  </head>
  <body>
    <main>
      <article>
        <h1>HTML is structural</h1>
        <p>Meaning over appearance.</p>
      </article>
    </main>
  </body>
</html>
```

</details>

---

## 2. HTML vs XML vs XHTML

**Definition:** HTML is **error-tolerant**, XML/XHTML are **error-intolerant**.

Key differences:

* HTML has a custom parser with recovery rules
* XML/XHTML require well-formed markup
* HTML defines semantics; XML defines syntax only

Comparison:

* HTML: forgiving, browser-defined parsing
* XML: strict, fails fast
* XHTML: HTML semantics + XML rules (served as application/xhtml+xml)

<details>
<summary>Example</summary>

```html
<!-- HTML (valid, auto-corrected) -->
<p><div>A</p>

<!-- XML / XHTML (fatal error) -->
<p><div>A</div></p>
```

</details>

---

## 3. HTML Living Standard (WHATWG) vs W3C

**Definition:** HTML is a continuously evolving **Living Standard**, not versioned specs.

WHATWG:

* Single living spec
* Browser-vendor driven
* De facto implementation authority

W3C:

* Snapshots of WHATWG spec
* Advisory / endorsement role
* No longer authoritative for HTML parsing

Reality:

* Browsers implement WHATWG
* Validators follow WHATWG

<details>
<summary>Example</summary>

```text
HTML5 ❌ (obsolete term)
HTML Living Standard ✅
```

</details>

---

## 4. The Web Platform Stack

**Definition:** HTML is the **entry layer** of the Web Platform stack.

Stack layers:

* HTML → structure & semantics
* CSS → layout, paint, compositing
* JavaScript → behavior & orchestration
* DOM → object model
* Web APIs → capabilities (fetch, storage, media, workers)

Execution reality:

* HTML creates DOM
* DOM is mutated by JS
* CSSOM + DOM → render tree

<details>
<summary>Example</summary>

```html
<button id="btn">Click</button>

<script>
  btn.addEventListener('click', () => {
    btn.textContent = 'DOM mutated via JS';
  });
</script>
```

</details>

---

## 5. Conformance, Validators, and Compliance Levels

**Definition:** Conformance defines whether a document matches the HTML specification.

Conformance types:

* Document conformance (author-facing)
* User agent conformance (browser-facing)
* Authoring tool conformance

Validators:

* Check spec compliance
* Do NOT guarantee correct rendering
* Detect non-conforming markup

Compliance levels:

* Conforming document
* Non-conforming but supported (legacy)
* Invalid but recoverable

<details>
<summary>Example</summary>

```html
<img src="a.png">        <!-- non-conforming (missing alt) -->
<img src="a.png" alt=""> <!-- conforming -->
```

</details>

---

## Mental Model (Interview One-Liner)

> **HTML is a fault-tolerant, semantic, declarative platform layer that bootstraps the DOM and orchestrates CSS, JavaScript, and Web APIs — not just markup.**
