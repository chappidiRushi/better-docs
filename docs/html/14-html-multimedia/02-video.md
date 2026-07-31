---
sidebar_position: 2
---

# 14.2 Video

## Definitions

- **Video Element (`<video>`)**: The HTML5 element used to embed video content in a document.
- **Poster**: An image that is shown while the video is downloading, or until the user hits the play button.
- **Captions/Subtitles**: Text versions of the spoken audio in a video, displayed over the video.

## Beginner Level Introduction

### The Video Element

Similar to the `<audio>` tag, the `<video>` tag allows you to embed video directly into your HTML without needing Flash or other plugins.

```html
<video src="movie.mp4" controls width="400"></video>
```
The `controls` attribute adds video controls, like play, pause, and volume. It is highly recommended to also include `width` and `height` attributes to prevent the page from shifting while the video loads.

## Deep Dive

### Video Sources and Formats

There are three supported video formats in HTML:
1. **MP4**: Supported by all modern browsers. It is the gold standard for web video.
2. **WebM**: Excellent compression, supported by most modern browsers.
3. **Ogg**: Supported by Firefox and Chrome, but rarely used compared to MP4 and WebM.

Just like with audio, you should use the `<source>` tag to provide multiple formats, ensuring maximum compatibility.

```html
<video width="320" height="240" controls>
  <source src="movie.mp4" type="video/mp4">
  <source src="movie.ogg" type="video/ogg">
  Your browser does not support the video tag.
</video>
```

### The Poster Attribute

The `poster` attribute specifies an image to be shown while the video is downloading, or until the user hits the play button. If this is not included, the browser will use the first frame of the video, which might be a blank black screen.

```html
<video src="movie.mp4" poster="movie_thumbnail.jpg" controls></video>
```

### Autoplay and Muted Videos

Just like audio, browsers hate auto-playing videos with sound because they annoy users.

If you want a video to play automatically (for example, a silent video background on a landing page), **you must mute it**. Browsers almost always allow muted videos to autoplay.

```html
<video src="background.mp4" autoplay loop muted playsinline></video>
```
*(Note: `playsinline` is necessary for iOS Safari to allow the video to play directly on the page instead of forcing it into Apple's native fullscreen video player).*

### Captions and Subtitles (`<track>`)

For accessibility (deaf or hard of hearing users) and usability (users watching videos on a train without headphones), providing captions is critical.

You can add subtitles using the `<track>` element. It points to a VTT (Web Video Text Tracks) file that contains the timestamps and the text.

```html
<video src="movie.mp4" controls>
  <track src="subtitles_en.vtt" kind="subtitles" srclang="en" label="English">
  <track src="subtitles_es.vtt" kind="subtitles" srclang="es" label="Spanish">
</video>
```

## Examples

<details>
<summary><strong>Example: The Ultimate Video Background</strong></summary>

```html
<!-- 
  Special Attributes on <video>:
  - autoplay: Starts immediately.
  - muted: CRITICAL for autoplay to work on modern browsers.
  - loop: Loops infinitely.
  - playsinline: Prevents iOS from popping the video into fullscreen.
  - poster: A fallback image if the video takes a moment to load.
-->
<video autoplay muted loop playsinline poster="fallback-image.jpg" id="bg-video" style="width: 100vw; height: 100vh; object-fit: cover;">
  <source src="hero_background.webm" type="video/webm">
  <source src="hero_background.mp4" type="video/mp4">
</video>
```

</details>

<details>
<summary><strong>Example: Accessible Video with Subtitles</strong></summary>

```html
<video width="640" height="360" controls poster="thumbnail.jpg">
  <source src="interview.mp4" type="video/mp4">
  
  <!-- 
    Special Attributes on <track>:
    - kind: Defines what the track is. Options are subtitles, captions, descriptions, chapters, or metadata.
    - srclang: The language code.
    - label: What the user sees in the CC menu (e.g., "English").
    - default: Specifies this track should be enabled by default.
  -->
  <track src="interview_en.vtt" kind="subtitles" srclang="en" label="English" default>
  
  <p>Your browser doesn't support HTML5 video.</p>
</video>
```

</details>
