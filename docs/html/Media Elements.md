---

id: media-elements
title: Media Elements
sidebar_position: 12
--------------------

# Media Elements

> Media elements embed external visual and audio resources, with complex loading, decoding, error handling, and security behavior.

---

## Unified Example (Images, Audio, Video, Tracks)

```html
<!-- Image element: replaced element, no children rendered -->
<img
  src="image.jpg" <!-- src: image URL -->
  alt="Architecture diagram" <!-- alt: REQUIRED for accessibility ("" for decorative) -->
  width="800" height="400" <!-- width/height: intrinsic size, prevents layout shift -->
  loading="lazy" <!-- loading: eager | lazy -->
  decoding="async" <!-- decoding: sync | async | auto -->
  referrerpolicy="no-referrer" <!-- referrerpolicy: controls Referer header -->
  crossorigin="anonymous" <!-- crossorigin: anonymous | use-credentials -->
  fetchpriority="high" <!-- fetchpriority: high | low | auto -->
>

<!-- Responsive images -->
<img
  src="small.jpg"
  srcset="small.jpg 480w, medium.jpg 800w, large.jpg 1200w" <!-- srcset: image candidates with width or density descriptors -->
  sizes="(max-width: 600px) 90vw, 800px" <!-- sizes: slot size hints -->
  alt="Responsive example"
>

<!-- Picture: art direction & format switching -->
<picture>
  <source srcset="image.avif" type="image/avif"> <!-- preferred modern format -->
  <source srcset="image.webp" type="image/webp"> <!-- fallback modern format -->
  <img src="image.jpg" alt="Picture fallback"> <!-- required fallback -->
</picture>

<!-- Audio element -->
<audio
  controls <!-- controls: built-in UI -->
  autoplay <!-- autoplay: may be blocked without muted -->
  muted <!-- muted: allows autoplay -->
  loop <!-- loop: restart automatically -->
  preload="metadata" <!-- preload: none | metadata | auto -->
  crossorigin="anonymous"
>
  <source src="audio.mp3" type="audio/mpeg">
  <source src="audio.ogg" type="audio/ogg">
</audio>

<!-- Video element -->
<video
  controls
  autoplay
  muted
  loop
  playsinline <!-- playsinline: prevents fullscreen on mobile -->
  poster="poster.jpg" <!-- poster: placeholder image -->
  preload="metadata"
  crossorigin="anonymous"
>
  <source src="video.mp4" type="video/mp4">
  <source src="video.webm" type="video/webm">

  <!-- Text tracks -->
  <track
    kind="subtitles" <!-- kind: subtitles | captions | descriptions | chapters | metadata -->
    src="subs.vtt" <!-- WebVTT file -->
    srclang="en" <!-- language -->
    label="English"
    default <!-- default: boolean -->
  >
</video>
```

---

## Notes for Hardcore Developers

* `<img>` is a replaced element; DOM children are ignored
* Missing `alt` is an accessibility failure; empty `alt` marks decorative images
* `width`/`height` define aspect ratio and prevent CLS
* `srcset` selection depends on DPR, viewport, and `sizes`
* `<picture>` enables **art direction**, not just resolution switching
* Media elements create **network requests early** via the preload scanner
* `autoplay` requires `muted` in most browsers
* `preload` is a hint, not a guarantee
* Text tracks (`<track>`) are required for captions compliance

---

## CORS, Security, and Errors

* `crossorigin` controls credential mode for media fetches
* Canvas extraction from media requires proper CORS headers
* Media error states exposed via `HTMLMediaElement.error`
* Common error causes: codec unsupported, CORS failure, network abort

---

## Mental Model (Interview One-Liner)

> **Media elements are replaced elements with complex fetch, decode, and playback lifecycles; performance, accessibility, and CORS are integral—not optional—concerns.**
