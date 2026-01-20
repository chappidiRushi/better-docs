---

id: links-media

title: Links & Media

sidebar_position: 4
--------------


# Links & Media

> Hyperlinking, embedded media, and external resource integration in HTML

---

## Links (`<a>`)

```txt
Creates hyperlinks to resources, fragments, or actions
```

```html
<a href="URL">text</a>
```

<details>
<summary>Examples</summary>

```html
<a href="/page">Internal</a>
<a href="https://example.com" target="_blank" rel="noopener noreferrer">
  External
</a>
<a href="#section">Fragment</a>
<a href="mailto:a@b.com">Email</a>
<a href="tel:+123">Phone</a>
```

```txt
Attributes:
- href: URL | fragment | scheme
- target: _self | _blank | _parent | _top
- rel: space-separated tokens
```

</details>

---

## Images (`<img>`)

```txt
Embeds raster or vector images as replaced elements
```

```html
<img src="url" alt="text">
```

<details>
<summary>Examples</summary>

```html
<img src="a.png" alt="Diagram">
<img src="a.webp" alt="" loading="lazy" decoding="async">
```

```html
<img
  src="small.jpg"
  srcset="small.jpg 1x, large.jpg 2x"
  sizes="(max-width: 600px) 100vw, 600px">
```

```txt
Attributes:
- alt: string (required)
- loading: eager | lazy
- decoding: sync | async | auto
```

</details>

---

## Audio (`<audio>`)

```txt
Embeds sound content with optional controls and scripting
```

```html
<audio src="a.mp3"></audio>
```

<details>
<summary>Examples</summary>

```html
<audio controls autoplay loop muted>
  <source src="a.mp3" type="audio/mpeg">
  <source src="a.ogg" type="audio/ogg">
</audio>
```

```txt
Attributes:
- controls | autoplay | loop | muted
- preload: none | metadata | auto
```

</details>

---

## Video (`<video>`)

```txt
Embeds video content with playback and rendering controls
```

```html
<video src="v.mp4"></video>
```

<details>
<summary>Examples</summary>

```html
<video controls poster="p.jpg" width="640" height="360">
  <source src="v.mp4" type="video/mp4">
  <source src="v.webm" type="video/webm">
</video>
```

```txt
Attributes:
- controls | autoplay | loop | muted | playsinline
- preload: none | metadata | auto
```

</details>

---

## Iframes (`<iframe>`)

```txt
Embeds another browsing context within the document
```

```html
<iframe src="url"></iframe>
```

<details>
<summary>Examples</summary>

```html
<iframe
  src="https://example.com"
  loading="lazy"
  sandbox="allow-scripts allow-same-origin"
  referrerpolicy="no-referrer">
</iframe>
```

```txt
Attributes:
- sandbox: space-separated flags
- loading: eager | lazy
- referrerpolicy: no-referrer | origin | strict-origin
```

</details>

---

## Embedding External Content

```txt
Including third-party or remote content via HTML embedding mechanisms
```

<details>
<summary>Examples</summary>

```html
<!-- Video platform embed -->
<iframe src="https://player.example.com/embed/123"></iframe>
```

```html
<!-- Object/embed (legacy) -->
<object data="file.pdf" type="application/pdf"></object>
<embed src="file.swf">
```

```txt
Notes:
- Prefer <iframe> over <object>/<embed>
- Apply sandbox for untrusted content
```

</details>

---

## Notes

* Media elements are replaced elements
* Network loading is UA-controlled
* Accessibility relies on alt, tracks, controls

## Caveats

* Autoplay is often blocked without muted
* Iframes introduce security boundaries
* Missing alt breaks accessibility
