---

id: sectioning-and-document-structure
title: Sectioning and Document Structure
sidebar_position: 8
-------------------

# Sectioning and Document Structure

> Sectioning elements define **semantic structure, outline, and accessibility landmarks**, not layout.

---

## 1. Sectioning Content Model

**Definition:** A set of elements that create sections and contribute to the document outline.

Sectioning content:

* `<main>`
* `<section>`
* `<article>`
* `<aside>`
* `<nav>`

Non-sectioning structural elements (do **not** affect outline):

* `<div>`, `<span>`
* `<header>`, `<footer>`

Effects:

* Resets heading scope
* Influences accessibility tree
* Defines semantic regions (not visual boxes)

```html
<main>
  <section>
    <h1>Scoped Heading</h1>
  </section>
</main>
<!-- each sectioning element creates its own outline context -->
```

---

## 2. `<main>`

**Definition:** Represents the dominant content of the document.

Rules:

* Only one per document
* Must not be descendant of `article`, `aside`, `nav`, `section`
* Implicit ARIA role: `main`

```html
<main id="content">
  <h1>Primary Content</h1>
</main>
<!-- landmark: main -->
```

---

## 3. `<section>`

**Definition:** Thematic grouping of content, typically with a heading.

Rules:

* Should have an accessible name (usually a heading)
* No inherent landmark role

```html
<section>
  <h2>Features</h2>
  <p>Details</p>
</section>
<!-- use when content needs a heading -->
```

---

## 4. `<article>`

**Definition:** Self-contained, independently distributable content.

Use cases:

* Blog posts
* Comments
* Cards

Properties:

* Can be nested
* Implicit ARIA role: `article`

```html
<article>
  <h2>Post Title</h2>
  <p>Body</p>
</article>
<!-- content should make sense if reused standalone -->
```

---

## 5. `<aside>`

**Definition:** Tangential or complementary content.

Semantics:

* Related but not primary
* Implicit ARIA role: `complementary`

```html
<aside>
  <h2>Related</h2>
  <p>Links</p>
</aside>
<!-- sidebars, callouts, pull quotes -->
```

---

## 6. `<nav>`

**Definition:** Section containing major navigation links.

Rules:

* Not all links belong in `<nav>`
* Implicit ARIA role: `navigation`

```html
<nav aria-label="Primary">
  <a href="/home">Home</a>
  <a href="/docs">Docs</a>
</nav>
<!-- aria-label required when multiple navs exist -->
```

---

## 7. `<header>` and `<footer>`

**Definition:** Introductory and concluding content for a section.

Rules:

* Can appear inside any sectioning content
* Can contain headings, nav, metadata
* **Do not** create new sections

Landmark behavior:

* Top-level `<header>` → `banner`
* Top-level `<footer>` → `contentinfo`
* Scoped header/footer → no landmark role

```html
<article>
  <header>
    <h2>Title</h2>
    <p>Meta</p>
  </header>

  <footer>
    <p>Author</p>
  </footer>
</article>
<!-- header/footer are contextual unless top-level -->
```

---

## 8. Heading Algorithm (and Why It’s Controversial)

**Definition:** HTML defines an outline algorithm based on sectioning elements.

Reality:

* Browsers do not expose the outline
* Screen readers ignore the algorithm
* Headings (`h1–h6`) define hierarchy in practice

Guideline:

* Use headings explicitly
* Do not rely on implicit outline

```html
<section>
  <h1>Top</h1>
  <section>
    <h2>Nested</h2>
  </section>
</section>
<!-- explicit heading levels preferred -->
```

---

## 9. Landmark Roles

**Definition:** Semantic regions exposed to assistive technologies.

Implicit landmarks:

* `<main>` → `main`
* `<nav>` → `navigation`
* `<aside>` → `complementary`
* `<header>` → `banner` (top-level only)
* `<footer>` → `contentinfo` (top-level only)

Notes:

* `<section>` and `<article>` are **not** landmarks by default
* ARIA roles should not override native semantics unless required

```html
<header>
  <h1>Site</h1>
</header>

<main>
  <article>Content</article>
</main>

<footer>
  <p>©</p>
</footer>
<!-- landmarks enable region-based navigation -->
```

---

## 10. Common Expert Pitfalls

* Using `<section>` with no heading (creates meaningless outline nodes)
* Treating `<article>` as a styling hook instead of standalone content
* Wrapping entire pages in `<section>` instead of `<main>`
* Overusing ARIA roles where native semantics already exist

```html
<!-- ❌ anti-pattern -->
<section>
  <div>Content</div>
</section>

<!-- ✅ correct -->
<main>
  <article>
    <h1>Title</h1>
  </article>
</main>
```

---

## Mental Model (Interview One-Liner)

> **Sectioning elements define semantic structure and accessibility landmarks; layout is a CSS concern, and explicit headings—not the outline algorithm—define hierarchy in real-world browsers.**
