---

id: scripting-and-html
title: Scripting and HTML
sidebar_position: 17
--------------------

# Scripting and HTML

> How JavaScript integrates with HTML parsing, execution order, security, and the event loop.

---

## Unified Example (All Script Forms)

```html
<!-- CLASSIC BLOCKING SCRIPT -->
<script src="app.js"></script> <!-- blocks parsing, executes immediately -->

<!-- DEFERRED SCRIPT -->
<script src="defer.js" defer></script> <!-- executes after parse, in order -->

<!-- ASYNC SCRIPT -->
<script src="async.js" async></script> <!-- executes ASAP, order not guaranteed -->

<!-- INLINE SCRIPT -->
<script>
  console.log('inline'); // executes immediately when encountered
</script>

<!-- MODULE SCRIPT -->
<script type="module" src="main.mjs"></script> <!-- deferred by default, strict mode -->

<!-- INLINE MODULE -->
<script type="module">
  import { x } from './dep.mjs';
</script>

<!-- IMPORT MAP -->
<script type="importmap">
{
  "imports": {
    "lodash": "/vendor/lodash.mjs"
  }
}
</script>
```

---

## `<script>` Element Deep Dive

* Executes JavaScript in document context
* Placement affects execution timing
* Blocking by default (classic scripts)
* May trigger network fetch

**Non-universal attributes:**

* `src` — external script URL
* `type` — MIME / script type (`text/javascript`, `module`, `importmap`)
* `async` — async fetch + execute (classic only)
* `defer` — deferred execution (classic only)
* `crossorigin` — `anonymous | use-credentials`
* `integrity` — Subresource Integrity hash
* `referrerpolicy` — referrer control
* `nomodule` — ignored by module-capable browsers

> One-liner: *`<script>` is a parser-affecting, execution-triggering element.*

---

## Script Types

### Classic Scripts

* Default if `type` omitted or `text/javascript`
* Share global scope
* Can use `document.write`

### Module Scripts

* `type="module"`
* Deferred automatically
* Strict mode enforced
* Scoped (no globals)
* Support `import` / `export`

### Import Maps

* Declarative module specifier resolution
* Must appear **before** module scripts
* No dynamic updates

> One-liner: *Modules change loading, scoping, and execution semantics entirely.*

---

## Async vs Defer vs Blocking

| Mode     | Parsing   | Execution Time | Order Guaranteed   |
| -------- | --------- | -------------- | ------------------ |
| blocking | stops     | immediately    | yes                |
| defer    | continues | after parse    | yes                |
| async    | continues | ASAP           | no                 |
| module   | continues | after parse    | yes (module graph) |

> One-liner: *Async trades determinism for speed; defer trades speed for order.*

---

## Inline vs External Scripts

* Inline:

  * No network request
  * Affected by CSP (`unsafe-inline`)
* External:

  * Cacheable
  * Supports SRI

> One-liner: *External scripts are safer and cacheable; inline scripts are immediate.*

---

## Execution Order Guarantees

* Blocking scripts: DOM-order
* Deferred scripts: DOM-order after parse
* Async scripts: network-order
* Modules:

  * Dependency graph order
  * Depth-first execution

> One-liner: *Only defer and modules guarantee order without blocking.*

---

## Interaction with DOM Parsing

* Parser pauses for blocking scripts
* DOM may be incomplete during execution
* `document.write` mutates input stream
* Deferred / module scripts see full DOM

> One-liner: *Scripts can halt, mutate, or observe the parser.*

---

## Security Restrictions

* Same-origin policy applies
* CORS enforced for modules
* CSP controls:

  * `script-src`
  * `unsafe-inline`
  * `nonce-*`
* SRI ensures content integrity

> One-liner: *Script execution is one of the most restricted operations in HTML.*

---

## Mental Model (Interview Grade)

> **HTML parsing and JavaScript execution are interleaved processes governed by strict ordering, security, and loading rules — misuse causes race conditions and jank.**
