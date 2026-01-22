---

id: html-i18n
title: Internationalization (i18n)
sidebar_position: 21
--------------------

# Internationalization (i18n)

> How HTML encodes language, direction, text processing, and Unicode semantics for global content.

---

## Unified Example (Language, Direction, Encoding)

```html
<!DOCTYPE html>
<html lang="ar" dir="rtl"> <!-- lang: BCP 47 tag | dir: ltr | rtl | auto -->
<head>
  <meta charset="utf-8"> <!-- character encoding: utf-8 REQUIRED -->
  <title>مثال</title>
</head>
<body>

  <p lang="en">English text</p> <!-- language override -->

  <p dir="ltr">123 ABC العربية</p> <!-- explicit direction override -->

  <bdi>user123</bdi> <!-- bidi-isolated user content -->
  <bdo dir="ltr">FORCED</bdo> <!-- bidi override (dangerous) -->

  <time datetime="2026-01-01">01/01/2026</time> <!-- localization-sensitive -->

</body>
</html>
```

---

## Language Declaration (`lang`)

* Declares natural language of content
* Used by:

  * Screen readers
  * Spellcheck
  * Hyphenation
  * Translation tools

**Values:**

* BCP 47 language tags (`en`, `en-US`, `fr-CA`, `ar`, `zh-Hans`)

* Inherited by descendants

* Must be set on `<html>`

> One-liner: *`lang` defines how text is interpreted, not how it looks.*

---

## Directionality (`dir`)

* Controls inline text direction

**Values:**

* `ltr` — left-to-right

* `rtl` — right-to-left

* `auto` — UA heuristic

* Inherited

* Affects cursor movement, punctuation, alignment

> One-liner: *Direction is semantic, not stylistic.*

---

## Bidirectional (Bidi) Algorithm Basics

* Unicode algorithm resolves mixed-direction text
* Triggered by strong characters (Arabic, Hebrew)

**HTML helpers:**

* `<bdi>` — isolates embedded text
* `<bdo>` — overrides direction (forceful)

> One-liner: *Bidi is automatic unless you break it.*

---

## Character Encodings

* Browsers must support UTF-8
* Encoding detection order:

  1. HTTP headers
  2. `<meta charset>`
  3. BOM

**Rules:**

* Always use UTF-8
* Declare early

> One-liner: *Wrong encoding corrupts everything upstream.*

---

## Unicode Normalization

* Same glyph ≠ same code points
* Normalization forms:

  * NFC (preferred for HTML)
  * NFD
  * NFKC / NFKD

**Implications:**

* String comparison
* IDs, URLs, form values

> One-liner: *Text equality is not visual equality.*

---

## Localization Pitfalls

* Hardcoded text in markup
* Concatenated strings
* Date / time formats
* Number separators
* Text expansion (layout breakage)

**HTML-specific traps:**

* Missing `lang`
* Forcing `dir` with CSS
* Using `<br>` for layout

> One-liner: *Internationalization failures are structural, not cosmetic.*

---

## Often-Missed Details (Hardcore)

* `:lang()` CSS selector depends on `lang`
* Search engines use `lang`
* Assistive tech switches pronunciation per `lang`
* Emoji are bidi-neutral
* Form submission preserves Unicode

---

## Mental Model (Interview Grade)

> **HTML internationalization is about correctly declaring language, direction, and encoding so that text can be processed, spoken, searched, and rendered correctly worldwide.**
