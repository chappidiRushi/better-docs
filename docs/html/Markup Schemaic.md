---

id: markup-semantics

title: Markup Semantics & Structure

sidebar_position: 3
--------------


# Markup Semantics & Structure

> Correct meaning, structure, accessibility, and machine-readable metadata in HTML

---

## Advanced Semantic Correctness

```txt
Using elements according to their intended meaning, not appearance
```

<details>
<summary>Examples</summary>

```html
<nav>
  <ul>
    <li><a href="/">Home</a></li>
  </ul>
</nav> <!-- navigation landmark -->

<main>
  <article>
    <header>
      <h1>Title</h1>
    </header>
    <p>Content</p>
  </article>
</main>
```

```html
<!-- Incorrect but common -->
<div onclick="go()">Button</div> <!-- should be <button> -->
```

</details>

---

## Sectioning Algorithm & Outline Pitfalls

```txt
Implicit document outline based on sectioning content and headings
```

<details>
<summary>Examples</summary>

```html
<section>
  <h1>A</h1>
  <section>
    <h2>B</h2>
  </section>
</section>
```

```html
<!-- Pitfall: skipped heading levels -->
<h1>Main</h1>
<h4>Sub</h4> <!-- outline still valid, but confusing -->
```

```txt
Notes:
- Outline algorithm is deprecated in practice
- Screen readers rely on heading order, not outline
```

</details>

---

## Accessible Name & Description Computation

```txt
Algorithm that determines what assistive tech announces for an element
```

<details>
<summary>Examples</summary>

```html
<button aria-label="Close"></button> <!-- name from aria-label -->

<input id="q">
<label for="q">Search</label> <!-- name from <label> -->

<img src="x.png" alt="Diagram"> <!-- name from alt -->
```

```html
<button aria-labelledby="t d">
  <span id="t">Save</span>
  <span id="d" hidden>Draft</span>
</button>
```

```txt
Priority (simplified):
1. aria-labelledby
2. aria-label
3. associated <label>
4. element text / alt
```

</details>

---

## Microdata vs RDFa vs JSON-LD

```txt
Ways to embed structured data for machines (SEO, knowledge graphs)
```

---

### Microdata

```txt
HTML attribute-based, tightly coupled to markup
```

<details>
<summary>Examples</summary>

```html
<div itemscope itemtype="https://schema.org/Person">
  <span itemprop="name">Ada</span>
  <meta itemprop="jobTitle" content="Engineer"> <!-- string | URL | number -->
</div>
```

</details>

---

### RDFa

```txt
Attribute-based, vocabulary-agnostic, expressive
```

<details>
<summary>Examples</summary>

```html
<div vocab="https://schema.org/" typeof="Person">
  <span property="name">Ada</span>
  <span property="jobTitle">Engineer</span>
</div>
```

</details>

---

### JSON-LD

```txt
Detached JSON graph, preferred by search engines
```

<details>
<summary>Examples</summary>

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "Ada",
  "jobTitle": "Engineer"
}
</script>
```

</details>

---

## Notes

* Semantics affect **accessibility, SEO, and maintainability**
* JSON-LD is **out-of-band** and safest to refactor
* ARIA does **not** fix bad semantics

## Caveats

* Overusing ARIA can reduce accessibility
* Sectioning elements do not create landmarks without roles
* Microdata/RDFa increase DOM coupling
