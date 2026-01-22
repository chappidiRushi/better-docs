---

id: links-and-navigation
title: Links and Navigation
sidebar_position: 11
-------------------

# Links and Navigation

> Hyperlinks define navigation, relationships between resources, and user interaction with the web graph.

---

## Unified Example (All Link & Navigation Concepts)

```html
<!-- Anchor element: creates a hyperlink when href is present -->
<a
  href="/docs/intro.html#installation" <!-- href: URL (absolute | relative | scheme-relative | fragment-only) -->
  target="_blank" <!-- target: _self | _blank | _parent | _top | frame-name -->
  rel="noopener noreferrer external" <!-- rel: space-separated link types -->
  download="guide.pdf" <!-- download: boolean | filename override -->
  hreflang="en-US" <!-- hreflang: language of linked resource -->
  type="text/html" <!-- type: MIME type hint -->
  referrerpolicy="no-referrer" <!-- referrerpolicy: same values as Fetch -->
  ping="/metrics" <!-- ping: space-separated URLs for POST beacons -->
>
  Installation Guide
</a>

<!-- URL resolution: relative URLs resolved against document base URL -->
<a href="../assets/manual.pdf">Relative URL</a>
<a href="https://example.com">Absolute URL</a>
<a href="#top">Fragment-only navigation</a>

<!-- Fragment identifiers: target element with matching id -->
<h2 id="top">Top Section</h2>

<!-- Navigation landmark -->
<nav aria-label="Primary"> <!-- nav: major navigation block -->
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/docs">Docs</a></li>
    <li><a href="/about">About</a></li>
  </ul>
</nav>

<!-- Non-navigational anchor (no href): not focusable, no link semantics -->
<a role="button" tabindex="0">Pseudo Button</a>

<!-- Focus and accessibility -->
<a href="/skip" class="skip-link">Skip to content</a>
```

---

## Notes for Hardcore Developers

* `<a>` without `href` **is not a link** (no keyboard focus, no link role)
* URL resolution follows RFC 3986 against the document’s base URL (`<base>` affects this)
* Fragment navigation updates the URL and scrolls to the target without reloading
* `rel="noopener"` prevents `window.opener` access (tab-nabbing mitigation)
* `rel="noreferrer"` suppresses the `Referer` header and implies `noopener`
* `download` is ignored for cross-origin URLs unless CORS allows it
* `ping` sends POST requests after navigation (often blocked by privacy tools)
* `nav` creates a navigation landmark; do not overuse it for every link group

---

## Common Expert Pitfalls

* Using `<a>` as a button instead of `<button>`
* Forgetting `rel="noopener"` on `target="_blank"`
* Over-nesting links inside interactive elements (invalid content model)
* Using fragments as state management without handling focus

---

## Mental Model (Interview One-Liner)

> **Links define the web’s navigation graph; `<a>` semantics depend on `href`, URLs resolve against base context, and security, focus, and accessibility are first-class concerns—not afterthoughts.**
