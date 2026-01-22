---

id: lists-and-grouping
title: Lists and Grouping Content
sidebar_position: 9
-------------------

# Lists and Grouping Content

> Elements used to group related content, represent collections, associate captions, and separate thematic sections.

---

## Unified Example (All Grouping Elements)

```html
<!-- Unordered list: order is not meaningful -->
<ul type="disc"> <!-- type: disc | circle | square (obsolete but supported) -->
  <li>Item A</li> <!-- li: list item, must be child of ul/ol/menu -->
  <li>Item B</li>
</ul>

<!-- Ordered list: order is meaningful -->
<ol start="3" reversed type="1"> <!-- start: number, reversed: boolean, type: 1 | a | A | i | I -->
  <li value="3">Third</li> <!-- value: explicit ordinal override -->
  <li value="2">Second</li>
</ol>

<!-- Definition list: name–value groupings (not Q&A lists) -->
<dl>
  <dt>HTML</dt> <!-- dt: term/name -->
  <dd>Markup language</dd> <!-- dd: description/value -->

  <dt>DOM</dt>
  <dd>Document Object Model</dd>
</dl>

<!-- Figure: self-contained content with optional caption -->
<figure>
  <img src="diagram.png" alt="Parsing flow"> <!-- img: phrasing content inside figure -->
  <figcaption>HTML parsing pipeline</figcaption> <!-- figcaption: first or last child only -->
</figure>

<!-- Horizontal rule: thematic break, not a visual line -->
<hr> <!-- hr: section-level thematic shift, exposed to accessibility APIs -->

<!-- Div: no semantics, grouping for styling or scripting only -->
<div data-role="layout"> <!-- div: last-resort generic container -->
  <p>Content</p>
</div>
```

---

## Notes for Hardcore Developers

* `<li>` elements may contain **flow content**, including nested lists
* `<ol reversed>` affects numbering order, not DOM order
* `<dl>` supports **multiple `<dd>` per `<dt>` and vice versa** (grouping semantics)
* `<figure>` can be moved without affecting document meaning
* `<hr>` participates in the document outline as a **section separator**
* `<div>` has **no implicit ARIA role**; prefer semantic elements first

---

## Mental Model (Interview One-Liner)

> **Lists model collections, figures model self-contained units, `<hr>` models thematic breaks, and `<div>` exists only when no semantic alternative applies.**
