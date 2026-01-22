---

id: metadata-and-document-head
title: Metadata and Document Head
sidebar_position: 7
-------------------

# Metadata and Document Head

> The `<head>` is the **control plane** for encoding, identity, policy, resource loading, and performance.

---

## 1. `<html>` Element

**Definition:** Root container for the entire document.

Key attributes:

* `lang` → document language (BCP 47, e.g. `en`, `en-US`, `fr`)
* `dir` → text direction (`ltr | rtl | auto`)
* `xmlns` → XML namespace (XHTML only, ignored in HTML)

```html
<html lang="en" dir="ltr">
<!-- lang: used by screen readers, spellcheck, translation -->
<!-- dir: affects bidi text rendering -->
```

---

## 2. `<head>` Element

**Definition:** Non-rendered container for metadata and resource declarations.

Notes:

* Parsed before `<body>`
* Auto-created if missing
* Influences parser, preload scanner, security policies

```html
<head>
  <meta charset="utf-8">   <!-- character encoding -->
  <title>App</title>        <!-- tab title / accessibility label -->
</head>
```

---

## 3. `<meta>`

**Definition:** Declares document metadata or HTTP-equivalent directives.

```html
<meta charset="utf-8">
<!-- charset: UTF-8 | legacy encodings (utf-8 recommended) -->

<meta name="viewport"
      content="width=device-width, initial-scale=1, maximum-scale=5">
<!-- width: device-width | number -->
<!-- initial-scale / maximum-scale: number -->

<meta name="description" content="Hardcore HTML">
<!-- description: SEO snippet -->

<meta name="robots" content="index, follow">
<!-- index | noindex , follow | nofollow -->

<meta http-equiv="Content-Security-Policy"
      content="default-src 'self'">
<!-- CSP delivered via HTML instead of HTTP header -->

<meta http-equiv="Referrer-Policy" content="strict-origin">
<!-- referrer behavior for outgoing requests -->
```

---

## 4. `<title>` and Accessibility

**Definition:** Document title shown in tabs, history, and assistive tech.

Rules:

* Only text content
* One per document
* Whitespace collapsed

```html
<title>Dashboard – Admin</title>
<!-- Used by screen readers as page name -->
<!-- Used by browser history and bookmarks -->
```

---

## 5. `<base>`

**Definition:** Sets base URL and default link target for relative URLs.

```html
<base href="https://cdn.example.com/" target="_blank">
<!-- href: base URL for all relative URLs -->
<!-- target: _self | _blank | _parent | _top | frame-name -->
```

---

## 6. `<link>`

**Definition:** Declares relationships, styles, icons, and resource hints.

```html
   <link rel="stylesheet" href="app.css" media="screen">
<!-- rel: stylesheet -->
<!-- media: screen | print | media-query -->

<link rel="icon" href="favicon.ico">
<!-- icon types: favicon | apple-touch-icon -->

<link rel="preload" href="app.js" as="script" crossorigin>
<!-- rel: preload | prefetch | preconnect | dns-prefetch -->
<!-- as: script | style | image | font | fetch -->
<!-- crossorigin: anonymous | use-credentials -->

<link rel="manifest" href="manifest.webmanifest">
<!-- PWA manifest -->
```

---

## 7. `<style>`

**Definition:** Inline CSS embedded in the document.

```html
<style media="screen" nonce="abc123">
/* media: screen | print | media-query */
/* nonce: CSP-controlled inline style */
body { margin: 0; }
</style>
```

---

## 8. `<script>` in Head

**Definition:** Declares executable JavaScript with loading rules.

```html
<script src="app.js"></script>
<!-- classic script: blocks parsing -->

<script defer src="app.js"></script>
<!-- defer: non-blocking, ordered, runs after DOM built -->

<script async src="app.js"></script>
<!-- async: non-blocking, unordered, runs ASAP -->

<script type="module" src="main.mjs"></script>
<!-- type: module | text/javascript -->

<script type="importmap">
{
  "imports": {
    "react": "/react.mjs"
  }
}
</script>
<!-- importmap: module specifier resolution -->
```

---

## 9. Resource Hints and Performance

**Definition:** Controls how and when the browser fetches resources.

```html
<link rel="dns-prefetch" href="//cdn.example.com">
<!-- DNS lookup only -->

<link rel="preconnect" href="https://api.example.com">
<!-- DNS + TCP + TLS -->

<link rel="prefetch" href="next-page.js">
<!-- low priority, future navigation -->

<link rel="preload" href="font.woff2" as="font" crossorigin>
<!-- high priority, required for render -->
```

---

## 10. Encoding, Policies, and Early Hints

**Definition:** Early metadata that affects browser behavior before rendering.

```html
<meta http-equiv="Permissions-Policy"
      content="geolocation=(), camera=(), microphone=()">
<!-- feature policy / permissions policy -->
```

---

## 11. Advanced Meta Directives (Often Missed)

**Definition:** Metadata that subtly affects UX, rendering, or navigation.

```html
<meta name="color-scheme" content="light dark">
<!-- color-scheme: light | dark | light dark -->

<meta name="theme-color" content="#0d1117">
<!-- theme-color: affects browser UI on mobile -->

<meta http-equiv="refresh" content="5; url=/login">
<!-- refresh: seconds; optional redirect URL (discouraged) -->
```

---

## 12. Fetch Priority & Blocking Controls

**Definition:** Explicitly influences request priority and render blocking.

```html
<link rel="stylesheet" href="app.css" blocking="render">
<!-- blocking: render | none (experimental) -->

<link rel="preload" href="hero.jpg" as="image" fetchpriority="high">
<!-- fetchpriority: high | low | auto -->

<script src="analytics.js" fetchpriority="low"></script>
<!-- deprioritize non-critical JS -->
```

---

## 13. `<noscript>` in Head

**Definition:** Fallback metadata or messaging when JavaScript is disabled.

```html
<noscript>
  <meta http-equiv="refresh" content="0; url=/no-js.html">
</noscript>
<!-- executed only when JS is disabled -->
```

---

## 14. Origin Trials & Experimental Features

**Definition:** Enables origin-scoped experimental browser features.

```html
<meta http-equiv="origin-trial" content="TOKEN">
<!-- enables experimental APIs for this origin -->
```

---

## Mental Model (Interview One-Liner)

> **The document head defines how the browser interprets, secures, fetches, and executes a page before anything is rendered.**
