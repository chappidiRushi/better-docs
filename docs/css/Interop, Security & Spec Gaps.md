---

id: css-interop-security
title: Interop, Security & Spec Gaps
sidebar_position: 28
--------------------

# Interop, Security & Spec Gaps

> CSS is constrained not just by specs, but by **browser interoperability, security models, and deliberate underspecification**. Mastery requires knowing where CSS is *intentionally limited*.

Progression: **vendor divergence → undefined behavior → security constraints → spec vs reality**.

---

## Vendor Prefixes

> Prefixes expose **experimental or legacy behavior**, not guarantees.

Common prefixes:

```css
-webkit-  /* Blink / WebKit */
-moz-     /* Gecko */
-ms-      /* Legacy Edge / IE */
```

Rules:

* Prefixed properties may differ semantically
* Unprefixed does NOT imply identical behavior
* Prefixes can outlive specs

<details>
<summary>Examples</summary>

```css
/* Legacy flexbox */
display: -webkit-box;
display: -ms-flexbox;
display: flex;

/* WebKit-only text clipping */
-webkit-background-clip: text;
-webkit-text-fill-color: transparent;
```

</details>

---

## Undefined & Underspecified Behavior Zones

> Some CSS behavior is **deliberately undefined** to allow UA freedom.

Common zones:

* Percentage resolution in cyclic dependencies
* Min/max sizing conflicts
* Scroll snapping edge cases
* Font fallback timing

Implications:

* Different browsers may all be spec-compliant
* Pixel-perfect parity is impossible

<details>
<summary>Examples</summary>

```css
.box {
  width: min(50%, 400px); /* % basis may differ by layout context */
}
```

</details>

---

## Visited Link Privacy

> CSS is restricted to prevent **history sniffing**.

Rules:

* `:visited` styles are heavily limited
* Computed styles are sanitized
* JS cannot read visited state

Allowed properties:

```text
color
background-color
border-color
outline-color
```

<details>
<summary>Examples</summary>

```css
a:visited {
  color: purple; /* allowed */
}

/* Disallowed: layout-affecting */
a:visited {
  font-size: 20px; /* ignored */
}
```

</details>

---

## Font Fingerprinting Limits

> Fonts are a **high-entropy fingerprinting vector**.

Mitigations:

* Restricted font enumeration
* Async font loading
* Permission-gated APIs

CSS effects:

* `@font-face` load failures are opaque
* Fallback timing is UA-controlled

<details>
<summary>Examples</summary>

```css
@font-face {
  font-family: 'PrivateFont';
  src: url(font.woff2);
}

/* Cannot detect whether font exists */
```

</details>

---

## Spec vs Implementation Gaps

> Browsers ship **partial, delayed, or divergent implementations**.

Sources of gaps:

* Performance trade-offs
* Security constraints
* Platform differences
* Backward compatibility

Examples:

* `:has()` performance differences
* Container query edge cases
* Color space interpolation variance

---

## Interop Reality

> CSS correctness ≠ identical rendering.

Practices:

* Test across engines (Blink, Gecko, WebKit)
* Avoid relying on undefined behavior
* Feature-detect, don’t version-detect

---

## Notes

* Prefixes signal instability
* Undefined behavior is intentional
* Security constraints override author intent
* Specs describe ranges, not pixels

## Caveats

* Relying on UA quirks creates fragility
* Security limits cannot be bypassed
* Interop bugs may persist for years
* "Works in Chrome" is meaningless
