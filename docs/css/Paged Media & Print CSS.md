---
id: paged-media-print-css
title: Paged Media & Print CSS
sidebar_position: 32
--------------------

# Paged Media & Print CSS

> Paged Media CSS defines how documents are **fragmented into pages**, how margins and headers/footers are generated, and how the cascade behaves when targeting **print and print‑like media**. This model is fundamentally different from continuous layout.

Progression: **page model → margins & boxes → fragmentation rules → cascade differences → real‑world limits**.

---

## `@page`

> `@page` defines the **page box**: size, margins, orientation, and page‑level properties.

Core facts:

* Applies only in paged media (`print`, print preview, PDF renderers)
* Does not select DOM elements
* One page box per printed page

Blueprint:

```css
@page <pseudo>? {
  size: <length>{1,2} | auto | A4 | letter;   /* width height */
  margin: <length-percentage>{1,4};
}
```

```css
@page {
  size: A4 portrait;
  margin: 20mm;
}
```

<details>
<summary>Examples</summary>

```css
@page {
  size: letter landscape;
  margin: 1in;
}

@page :first {
  margin-top: 3cm; /* title page */
}
```

</details>

---

## Page Margin Boxes

> Margin boxes generate **running content** (headers, footers, page numbers) outside the document flow.

Available boxes:

* `@top-left`, `@top-center`, `@top-right`
* `@bottom-left`, `@bottom-center`, `@bottom-right`
* Logical variants exist in specs

Content sources:

* Static strings
* `counter(page)` / `counter(pages)`
* `string-set()` (limited support)

```css
@page {
  @bottom-center {
    content: "Page " counter(page);
  }
}
```

<details>
<summary>Examples</summary>

```css
@page {
  @top-right {
    content: "Confidential";
    font-size: 10pt;
  }

  @bottom-right {
    content: counter(page) " / " counter(pages);
  }
}
```

</details>

---

## Page Breaks & Fragmentation

> Fragmentation controls **where content is split across pages**.

Modern properties:

* `break-before`
* `break-after`
* `break-inside`

Legacy aliases:

* `page-break-before`
* `page-break-after`
* `page-break-inside`

Allowed values:

```css
break-before: auto | avoid | always | page | left | right;
break-inside: auto | avoid;
```

<details>
<summary>Examples</summary>

```css
.chapter {
  break-before: page;   /* force new page */
}

.table {
  break-inside: avoid;  /* keep together */
}
```

</details>

---

## Print Cascade Differences

> The cascade is **media‑filtered** before evaluation, but otherwise unchanged.

Key differences:

* `@media print` applies
* Viewport‑based units may resolve differently
* Interactive states (`:hover`, `:focus`) are meaningless
* Animations and transitions are ignored

```css
@media print {
  a { color: black; text-decoration: underline; }
  .no-print { display: none; }
}
```

<details>
<summary>Examples</summary>

```css
@media print {
  body {
    font-size: 11pt; /* absolute sizing preferred */
  }
}
```

</details>

---

## Real‑World Constraints

> Print CSS support is **inconsistent and UA‑dependent**.

Important realities:

* Browsers implement only a subset of Paged Media spec
* Margin boxes poorly supported outside Chromium
* No reliable page‑count measurement pre‑print
* PDF engines (Prince, Antenna House) differ from browsers

---

## Notes

* Paged media is **fragmentation‑driven**, not flow‑driven
* Absolute units are safer than relative units
* Test across engines
* Expect partial support

## Caveats

* Browser print engines are under‑maintained
* Spec compliance varies widely
* Complex layouts degrade silently
* Do not rely on margin boxes in browsers
