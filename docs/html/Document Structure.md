---

id: document-structure

title: Document Structure

sidebar_position: 2
--------------

# Document Structure

> Canonical HTML document skeleton, metadata, and conformance rules

---

## Basic Document Structure

```txt
Minimal required elements for a standards-compliant HTML document
```

```html
<!DOCTYPE html>   <!-- standards mode -->
<html lang="en"> <!-- root element -->
  <head>          <!-- metadata container -->
  </head>
  <body>          <!-- render tree root -->
  </body>
</html>
```

<details>
<summary>Examples</summary>

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Doc</title>
  </head>
  <body>
    <main>Hello</main>
  </body>
</html>
```

```txt
Notes:
- <html>, <head>, <body> may be omitted (parser inserts them)
- <!DOCTYPE> controls quirks vs standards mode
```

</details>

---

## Meta Tags

```txt
Metadata that influences parsing, layout, SEO, and UA behavior
```

```html
<meta charset="utf-8">                  <!-- encoding -->
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="description" content="SEO summary">
```

<details>
<summary>Examples</summary>

```html
<head>
  <meta charset="utf-8">               <!-- must be first -->
  <meta name="viewport"
        content="width=device-width, initial-scale=1, viewport-fit=cover">
  <!-- width: device-width | number -->
  <!-- initial-scale: number -->

  <meta name="robots" content="index,follow"> <!-- index | noindex | follow | nofollow -->
  <meta name="author" content="Name">
</head>
```

```txt
SEO basics:
- <title> + description matter
- meta keywords ignored
```

</details>

---

## HTML Validation & Best Practices

```txt
Ensuring conformance with the HTML Living Standard
```

<details>
<summary>Examples</summary>

```txt
Validation checks:
- correct element nesting
- required attributes present
- unique IDs
- valid attribute values
```

```html
<form>
  <input type="email" required> <!-- type: email | text | url | number -->
</form>
```

```txt
Best practices:
- one <main> per document
- semantic elements over divs
- avoid deprecated elements (<font>, <center>)
```

</details>

---

## Notes

* Browsers auto-correct invalid structure
* Metadata affects rendering before DOM is ready
* Validation improves portability, not correctness

## Caveats

* Passing validation ≠ accessible
* SEO meta tags are hints, not guarantees
* Some invalid markup is intentionally supported
