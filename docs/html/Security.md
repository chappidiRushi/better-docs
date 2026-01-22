---

id: security
title: Security
sidebar_position: 20
--------------------

# HTML and Security

> How HTML participates in the browser security model: origin isolation, resource access, injection defense, and sandboxing.

---

## Unified Example (Security Controls in HTML)

```html
<!-- SAME-ORIGIN & CORS -->
<script src="https://cdn.example.com/app.js" crossorigin="anonymous"></script> <!-- anonymous | use-credentials -->

<!-- CSP META (fallback only) -->
<meta http-equiv="Content-Security-Policy"
      content="default-src 'self'; script-src 'self'; object-src 'none'">

<!-- IFRAME SANDBOX -->
<iframe src="https://third-party.example"
        sandbox="allow-scripts allow-forms"  <!-- granular capability flags -->
        referrerpolicy="no-referrer">
</iframe>

<!-- CLICKJACKING DEFENSE (server headers preferred) -->
<style>
  body { frame-ancestors: 'none'; } /* illustrative only */
</style>

<!-- TRUSTED TYPES (JS-enforced, declared via CSP) -->
<script>
  // unsafe assignment blocked when Trusted Types enforced
  // element.innerHTML = userInput; // TypeError
</script>
```

---

## Same-Origin Policy

* Core browser isolation model
* Origin = scheme + host + port
* Restricts:

  * DOM access
  * JS execution context
  * Storage (cookies, localStorage)

**Allowed across origins:**

* Navigation
* Embedding (with restrictions)
* Resource fetching (subject to CORS)

> One-liner: *Origins isolate execution and data by default.*

---

## CORS Basics

* Opt-in relaxation of SOP
* Controlled via HTTP headers

**Key headers:**

* `Access-Control-Allow-Origin`
* `Access-Control-Allow-Methods`
* `Access-Control-Allow-Headers`
* `Access-Control-Allow-Credentials`

**HTML attributes involved:**

* `crossorigin="anonymous | use-credentials"`

> One-liner: *CORS is a permission system layered on top of SOP.*

---

## Content Security Policy (CSP)

* Declarative injection defense
* Delivered via HTTP header (preferred) or meta tag

**Common directives:**

* `default-src`
* `script-src`
* `style-src`
* `img-src`
* `frame-ancestors`

**Inline control:**

* `nonce-*`
* `hash-*`

> One-liner: *CSP restricts what the browser is allowed to execute or load.*

---

## XSS and HTML Injection

* Occurs when untrusted input becomes executable

**Vectors:**

* Inline scripts
* Event handler attributes (`onclick`)
* `innerHTML`

**Mitigations:**

* Output encoding
* CSP
* Trusted Types

> One-liner: *XSS is a failure to separate data from code.*

---

## Clickjacking

* UI redress attack via framing

**Defenses:**

* `X-Frame-Options: DENY | SAMEORIGIN`
* CSP `frame-ancestors`
* `<iframe sandbox>` restrictions

> One-liner: *Clickjacking exploits visual trust, not code execution.*

---

## Sandbox Mechanisms

### `<iframe sandbox>`

* Starts with zero privileges
* Flags selectively re-enable capabilities:

  * `allow-scripts`
  * `allow-same-origin`
  * `allow-forms`
  * `allow-popups`

### `sandbox` Attribute on Links

```html
<a href="https://example.com" rel="noopener noreferrer">Link</a>
```

> One-liner: *Sandboxing removes ambient authority.*

---

## Trusted Types

* DOM-based XSS mitigation

* Enforced via CSP:

  * `require-trusted-types-for 'script'`

* Disallows string assignment to sensitive sinks:

  * `innerHTML`
  * `outerHTML`
  * `insertAdjacentHTML`

> One-liner: *Trusted Types make unsafe DOM APIs uncallable by default.*

---

## Often-Missed Details (Hardcore)

* `<base>` can be abused for injection
* `javascript:` URLs are blocked by CSP
* SVG introduces scriptable surfaces
* `noopener` prevents opener-based attacks
* DOM clobbering can bypass naive defenses

---

## Mental Model (Interview Grade)

> **HTML security is about minimizing ambient authority: origins isolate, CSP constrains execution, sandboxing removes privilege, and Trusted Types eliminate entire bug classes.**
