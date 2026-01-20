---
id: accessibility-responsiveness

title: Accessibility & Responsiveness

sidebar_position: 9
--------

# Accessibility & Responsiveness

> Building HTML that is usable, perceivable, and adaptable across devices and assistive technologies

---

## Accessibility Basics

```txt
Accessibility ensures content is operable and understandable for all users
```

<details>
<summary>Examples</summary>

```txt
Core principles (POUR):
- Perceivable
- Operable
- Understandable
- Robust
```

```html
<button>Click</button> <!-- native elements are accessible by default -->
```

</details>

---

## Alt Text

```txt
Text alternative for non-text content (<img>)
```

```html
<img src="photo.jpg" alt="User profile photo">
```

<details>
<summary>Examples</summary>

```html
<img src="chart.png" alt="Sales increased 20% in Q4">
```

```html
<img src="decorative.png" alt=""> <!-- decorative image -->
```

```txt
Rules:
- alt is required
- Empty alt hides image from AT
```

</details>

---

## Labels

```txt
Provide accessible names for form controls
```

```html
<label for="email">Email</label>
<input id="email" type="email">
```

<details>
<summary>Examples</summary>

```html
<label>
  Name
  <input type="text">
</label>
```

```txt
Rules:
- Every input needs a label
- Placeholder is not a label
```

</details>

---

## ARIA Introduction

```txt
ARIA adds semantic roles when native HTML is insufficient
```

```html
<div role="button" tabindex="0" aria-pressed="false"></div>
```

<details>
<summary>Examples</summary>

```html
<nav aria-label="Primary">
  <a href="/">Home</a>
</nav>
```

```txt
Rules:
- Use native HTML first
- No ARIA is better than bad ARIA
```

</details>

---

## Responsive HTML Structure

```txt
HTML that adapts structurally to different viewports
```

<details>
<summary>Examples</summary>

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

```html
<header>
  <nav></nav>
</header>
<main>
  <section></section>
</main>
```

```txt
Practices:
- Use semantic containers
- Avoid fixed dimensions in markup
```

</details>

---

## Notes

* Semantic HTML provides accessibility for free
* Responsive layout is mostly CSS-driven
* ARIA does not fix bad markup

## Caveats

* Overusing ARIA can reduce accessibility
* Missing alt text breaks screen reader flow
* Responsiveness fails without viewport meta
