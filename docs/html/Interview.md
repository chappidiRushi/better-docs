---
id: interview
title: HTML Interview Questions
sidebar_position: 2
--------------------

# Interview Questions & Answers

---

## Fundamentals (1–20)

**What is HTML and what problem does it solve?**
HTML (HyperText Markup Language) is the standard markup language for structuring content on the web. It provides semantic meaning to content so browsers, assistive technologies, and search engines can correctly interpret it.

**What is the difference between HTML and HTML5?**
HTML5 is a major evolution of HTML that introduced semantic elements, native audio/video, canvas, form enhancements, APIs (storage, geolocation), and removed reliance on plugins.

**Is HTML a programming language? Why or why not?**
No. HTML is a markup language. It describes structure and semantics, not logic or control flow.

**What is the purpose of the `<!DOCTYPE>` declaration?**
It triggers standards mode in browsers, ensuring consistent rendering and avoiding quirks mode.

**What happens if `<!DOCTYPE>` is omitted?**
Browsers may enter quirks mode, leading to inconsistent layout and legacy behavior.

**What are void (self-closing) elements?**
Elements that cannot have content or closing tags, e.g. `img`, `input`, `br`, `meta`, `link`.

**Can a browser render invalid HTML?**
Yes. Browsers are fault-tolerant and follow error-correction algorithms defined in the HTML spec.

**What is the DOM?**
The Document Object Model is a tree-based in-memory representation of an HTML document that scripts can manipulate.

**What is the difference between HTML and XHTML?**
XHTML is XML-based, stricter, case-sensitive, and requires well-formed markup. HTML is more forgiving.

**What is the `lang` attribute and why is it important?**
It defines the document language for accessibility, screen readers, and SEO.

**What is character encoding and why is UTF-8 recommended?**
Encoding defines how characters are represented. UTF-8 supports nearly all languages and avoids encoding issues.

**What does the `<meta charset="UTF-8">` tag do?**
It tells the browser how to decode the document’s characters.

**What is semantic HTML?**
Using meaningful tags (`article`, `nav`, `section`) instead of generic ones (`div`, `span`).

**Why is semantic HTML important for accessibility?**
Assistive technologies rely on semantics to convey structure and meaning.

**What is the difference between `div` and `span`?**
`div` is block-level; `span` is inline-level.

**What are global attributes?**
Attributes applicable to all elements, e.g. `id`, `class`, `style`, `hidden`, `data-*`.

**What is the purpose of `data-*` attributes?**
To store custom data accessible via JavaScript without polluting the DOM API.

**How does the browser parse HTML?**
It tokenizes input, builds the DOM tree, applies error recovery, then renders.

**What is the critical rendering path?**
The sequence of steps the browser takes to render a page: HTML → DOM → CSSOM → render tree → layout → paint.

**Can HTML affect performance? How?**
Yes. Excessive DOM nodes, blocking scripts, and poor structure slow rendering.

---

## Advanced & Tricky Concepts (21–50)

**What is the difference between `async` and `defer` in script tags?**
`async` executes as soon as downloaded; `defer` executes after HTML parsing, preserving order.

**What happens if multiple scripts use `async`?**
Execution order is non-deterministic.

**Why is `defer` preferred for most scripts?**
It avoids render blocking while preserving execution order.

**How does the browser handle unknown HTML tags?**
They are treated as inline elements unless styled otherwise.

**What is the difference between `section` and `article`?**
`article` is self-contained and reusable; `section` groups related content.

**Can `main` be nested?**
No. There must be only one `main` element per document.

**What are ARIA roles and when should they be avoided?**
ARIA enhances accessibility, but should not replace semantic HTML when native elements exist.

**What is the difference between `hidden` and `display: none`?**
`hidden` is semantic and can be toggled; `display: none` is purely visual.

**How does HTML handle whitespace?**
Consecutive whitespace is collapsed into a single space.

**What is the impact of deeply nested HTML?**
Slower DOM traversal, layout calculations, and poorer maintainability.

**What is the difference between `id` and `class` beyond uniqueness?**
`id` has higher CSS specificity and should represent unique document identity.

**What is the `template` element used for?**
Holding inert HTML fragments for later cloning via JavaScript.

**What does the `slot` element do?**
Enables content projection in Web Components.

**What are Web Components at a high level?**
A set of standards for encapsulated, reusable custom elements.

**What is the difference between `contenteditable` and form inputs?**
`contenteditable` allows free-form editing without built-in validation or submission semantics.

**How does HTML validation differ from browser error handling?**
Validation checks spec compliance; browsers attempt to render regardless.

**What is the `inert` attribute?**
Prevents interaction and focus within a subtree (useful for modals).

**How do browsers determine focus order?**
DOM order, tabindex values, and interactive elements.

**What problems can excessive tabindex usage cause?**
Broken keyboard navigation and accessibility issues.

**What is the difference between `b` vs `strong`, `i` vs `em`?**
`strong` and `em` convey semantic emphasis; `b` and `i` are purely stylistic.

**Can multiple `<header>` or `<footer>` elements exist?**
Yes, but they must be scoped to their sectioning content.

**What is the purpose of the `picture` element?**
Art-direction and responsive images.

**How does `srcset` differ from `picture`?**
`srcset` handles resolution switching; `picture` handles layout-based selection.

**What is the impact of missing `alt` attributes?**
Accessibility violations and degraded SEO.

**What does the browser do with duplicate IDs?**
Behavior is undefined; DOM APIs may return the first occurrence only.

**What is HTML parsing re-entrancy?**
Scripts can interrupt parsing, modify the DOM, then resume parsing.

**Why are tables discouraged for layout?**
Poor accessibility, inflexible design, and semantic misuse.

**What is the difference between `noscript` behaviors?**
It renders only when scripting is disabled or unsupported.

**How does HTML support internationalization (i18n)?**
Via `lang`, `dir`, Unicode, and semantic markup.

**What is the biggest HTML misconception among senior developers?**
Treating HTML as trivial instead of as a performance-, accessibility-, and semantics-critical layer.

---

## Expert-Level Edge Cases (51–70)

**Can HTML alone cause security issues?**
Yes. Poor use of attributes (e.g., `target="_blank"` without `rel`) can introduce vulnerabilities.

**What is reverse tabnabbing?**
A security issue where a new tab can manipulate the opener window.

**Why should `rel="noopener noreferrer"` be used?**
To prevent access to `window.opener` and referrer leakage.

**How does the browser recover from malformed nesting?**
By auto-closing tags according to the HTML parsing algorithm.

**What is the difference between DOM order and visual order?**
CSS can change visual order, but assistive tech follows DOM order.

**What are the performance implications of inline HTML event handlers?**
Tight coupling, harder maintenance, and worse separation of concerns.

**What is the HTML Living Standard?**
A continuously updated specification maintained by WHATWG.

**Why was XHTML abandoned for mainstream web development?**
Strict parsing caused catastrophic failures on minor errors.

**How does HTML influence SEO beyond meta tags?**
Through semantics, structure, headings, links, and accessibility.

**What is the role of HTML in modern frameworks?**
It remains the output target and accessibility foundation.

**What happens if multiple `main` elements exist?**
The document becomes invalid and assistive tech may misinterpret structure.

**What is the difference between `role="button"` and `<button>`?**
Native buttons provide keyboard, focus, and accessibility for free.

**Why should heading levels not be skipped?**
Screen readers rely on logical heading hierarchy.

**What is the impact of empty headings?**
Accessibility noise and poor document structure.

**What does HTML do that CSS and JS cannot replace?**
Provide intrinsic meaning and document semantics.

**What is the most future-proof HTML practice?**
Use semantic elements and minimal assumptions about presentation.

**Can HTML be considered an API?**
Yes—browsers expose HTML semantics through the DOM and accessibility tree.

**Why do browsers still support deprecated tags?**
Backward compatibility with legacy content.

**What is the accessibility tree?**
A parallel structure derived from the DOM for assistive technologies.

**What separates good HTML from great HTML?**
Intentional semantics, accessibility-first thinking, and performance awareness.

---

## Deep Cuts & Browser-Level Gotchas

**How does the HTML parser interact with the CSS parser?**
HTML parsing can pause when encountering blocking stylesheets because layout depends on CSSOM construction.

**Why does placing CSS in the `<head>` matter?**
It avoids flash of unstyled content and reduces layout thrashing during initial render.

**What happens when you manipulate the DOM during parsing?**
It can invalidate the parser’s state and force re-evaluation of insertion points.

**What is the difference between the DOM tree and the render tree?**
The render tree excludes non-visible nodes and includes computed styles.

**Why is `innerHTML` dangerous beyond XSS?**
It destroys existing nodes, event listeners, and can trigger full subtree re-parsing.

**What is parser-blocking vs render-blocking?**
Parser-blocking halts DOM construction; render-blocking delays painting.

**Why do browsers normalize certain HTML inputs?**
To ensure interoperability and consistent DOM APIs across engines.

**What is the difference between live and static DOM collections?**
Live collections update automatically; static ones do not.

**Why can HTML order affect CSS specificity resolution?**
Later elements can override earlier cascade decisions depending on selector scope.

**What is the cost of excessive custom elements?**
Lifecycle callbacks and shadow DOM can introduce measurable overhead.

---

## Accessibility & Semantics (Advanced)

**Why is using `div` with ARIA often an anti-pattern?**
Native elements already encode expected behaviors and accessibility.

**What breaks when visual order differs from DOM order?**
Keyboard navigation and screen reader flow become inconsistent.

**Why is `aria-hidden` dangerous when misused?**
It can completely remove content from assistive technologies.

**How does the accessibility tree differ from the DOM tree?**
It reflects user-perceivable structure, not raw markup.

**Why are landmarks critical for large documents?**
They allow quick navigation for assistive tech users.

**What is the accessibility impact of CSS-generated content?**
Pseudo-elements are often ignored by screen readers.

---

## Performance & Architecture

**How does DOM size affect Time to Interactive (TTI)?**
Larger DOMs increase style recalculation and layout cost.

**Why is HTML streaming important?**
It allows progressive rendering before the full document downloads.

**What is speculative parsing?**
Browsers prefetch resources before the DOM is fully constructed.

**Why does removing unused HTML matter?**
It reduces memory footprint and layout computation.

**How does HTML influence hydration cost in frameworks?**
Poor structure increases reconciliation complexity.

---

## Senior-Level Trick Questions

**Can HTML be deterministic?**
Not entirely—error recovery introduces browser-dependent behavior.

**Is valid HTML always accessible HTML?**
No. Validity does not guarantee usable semantics.

**Can semantic HTML ever hurt performance?**
Rarely, but misuse can increase DOM depth.

**What is the most common HTML mistake in SPAs?**
Ignoring document-level semantics like headings and landmarks.

**Why is HTML knowledge still tested at staff level?**
Because it underpins accessibility, performance, SEO, and UX at scale.
