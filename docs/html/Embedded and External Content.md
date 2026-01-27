---

id: embedded-and-external-content
title: Embedded and External Content
sidebar_position: 13
--------------------

# Embedded and External Content

> Elements that embed **independent documents, plugins, or external resources**, often creating new browsing contexts and security boundaries.

---

## Unified Example (Iframes, Embeds, Objects)

```html
<!-- iframe: embeds another browsing context -->
<iframe
  src="https://example.com" <!-- src: URL of embedded document -->
  srcdoc="<p>Hello</p>" <!-- srcdoc: inline HTML document (overrides src) -->
  name="preview" <!-- name: targetable browsing context name -->
  loading="lazy" <!-- loading: eager | lazy -->
  referrerpolicy="no-referrer" <!-- referrerpolicy: controls Referer header -->
  allow="fullscreen; geolocation" <!-- allow: permission policy directives -->
  sandbox="allow-scripts allow-same-origin" <!-- sandbox: space-separated capability flags -->
  width="600" height="400"
></iframe>

<!-- Embedded navigation targeting the iframe -->
<a href="/help" target="preview">Open in iframe</a>

<!-- embed: generic plugin embedding (largely obsolete) -->
<embed
  src="file.pdf" <!-- src: external resource -->
  type="application/pdf" <!-- type: MIME type -->
  width="600" height="400"
>

<!-- object: external resource with fallback content -->
<object
  data="movie.swf" <!-- data: resource URL -->
  type="application/x-shockwave-flash" <!-- type: MIME type -->
  width="600" height="400"
>
  <param name="quality" value="high"> <!-- param: plugin-specific parameter -->
  <p>Fallback content if object fails to load.</p>
</object>
```

---

## Notes for Hardcore Developers

### `<iframe>` Internals & Browsing Contexts

* Each iframe creates a **nested browsing context** with its own:

  * `Window`, `Document`, session history
* Same-origin iframes can access parent via DOM; cross-origin cannot
* `name` enables `target`-based navigation

### Sandboxing

* `sandbox` with no value = **maximum restriction**
* Common flags:

  * `allow-scripts`
  * `allow-same-origin`
  * `allow-forms`
  * `allow-popups`
* Combining `allow-scripts` + `allow-same-origin` removes most isolation

### `srcdoc`

* Inline HTML parsed as a **separate document**
* Treated as same-origin with parent
* Ignored if `src` is present and non-empty

### `<embed>` and `<object>`

* Legacy plugin model (Flash, Java applets)
* Limited accessibility and security support
* `<object>` supports fallback content; `<embed>` does not

---

## Security, Performance, and Pitfalls

* Iframes can be abused for clickjacking without proper sandboxing
* `allow` defines **Permissions Policy**, not user consent
* Lazy-loaded iframes delay network and rendering cost
* Plugin-based embeds are deprecated and often blocked

---

## Mental Model (Interview One-Liner)

> **Embedded content creates isolated browsing contexts with explicit security boundaries; `<iframe>` is the modern primitive, while `<embed>` and `<object>` exist mainly for legacy compatibility.**
