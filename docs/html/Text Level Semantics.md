---

id: text-level-semantics
title: Text-Level Semantics
sidebar_position: 9
-------------------

# Text-Level Semantics

> Inline semantics convey **meaning, emphasis, citations, time, and machine-readable intent** without affecting document structure.

---

## Inline vs Block Semantics

**Definition:** Text-level elements participate in the inline formatting context and do not create sections or landmarks.

Notes:

* Inline semantics affect accessibility and meaning, not layout
* Most can appear inside `<p>`
* Many trigger implicit ARIA roles or speech emphasis

---

## Unified Example (All Tags Together)

```html
<p>
  This is a <strong>critical</strong> message <!-- strong: importance, screen readers add emphasis -->
  and this is <b>stylistic bold</b> <!-- b: visual offset only, no semantic importance -->.

  <em>Emphasized text</em> <!-- em: stress emphasis, nesting increases intensity -->
  vs <i>italic text</i> <!-- i: alternate voice, technical term, foreign phrase -->.

  <u>Unarticulated annotation</u> <!-- u: non-textual annotation (e.g., misspelling, proper name) -->
  <s>Deprecated info</s> <!-- s: no longer accurate or relevant -->
  <del datetime="2023-01-01">Old price</del> <!-- del: removal, datetime/cite allowed -->
  <ins datetime="2024-01-01">New price</ins> <!-- ins: insertion, datetime/cite allowed -->

  <small>Fine print</small> <!-- small: side comments, legal text -->
  <mark>highlighted</mark> <!-- mark: relevance in current context -->

  <abbr title="World Wide Web">WWW</abbr> <!-- abbr: title attribute provides expansion -->

  According to <cite>HTML Living Standard</cite> <!-- cite: source title, not author -->,
  <q cite="https://html.spec.whatwg.org/">HTML is a living document</q> <!-- q: inline quotation, cite: source URL -->.
</p>

<blockquote cite="https://html.spec.whatwg.org/"> <!-- blockquote: extended quotation -->
  <p>HTML is the standard markup language for documents designed to be displayed in a web browser.</p>
</blockquote>

<p>
  Use <code>&lt;section&gt;</code> <!-- code: code fragment, monospace -->
  inside <pre><!-- pre: preserves whitespace, disables HTML collapsing -->
&lt;section&gt;
  &lt;h1&gt;Title&lt;/h1&gt;
&lt;/section&gt;
  </pre>

  Press <kbd>Ctrl</kbd> + <kbd>C</kbd> <!-- kbd: user input -->
  to copy, output is <samp>Copied</samp> <!-- samp: program output -->.
</p>

<p>
  Math example: H<sub>2</sub>O <!-- sub: subscript -->,
  area = r<sup>2</sup> <!-- sup: superscript -->
</p>

<p>
  Published on <time datetime="2024-03-01">March 1, 2024</time> <!-- time: machine-readable datetime (ISO 8601) -->
</p>

<p>
  Soft break here<br> <!-- br: line break, not spacing -->
  <wbr> <!-- wbr: optional word break hint -->
</p>

<p>
  Japanese annotation:
  <ruby>
    漢 <rt>kan</rt> <!-- rt: ruby text -->
    字 <rt>ji</rt>
  </ruby>
  <!-- ruby: East Asian pronunciation annotations -->
</p>

<p hidden> <!-- hidden: global attribute, removes from accessibility tree -->
  Hidden text
</p>
```

---

## Mental Model (Interview One-Liner)

> **Text-level semantics enrich meaning and accessibility without affecting document structure; choose elements based on intent, not appearance.**
