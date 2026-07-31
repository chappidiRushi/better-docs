---
sidebar_position: 1
---

# 14.1 Audio

## Definitions

- **Audio Element (`<audio>`)**: The HTML5 element used to embed sound content in documents.
- **Source Element (`<source>`)**: Used within `<audio>` and `<video>` tags to specify multiple alternative media resources.
- **Controls**: The play, pause, and volume buttons provided natively by the browser.
- **Autoplay**: A feature that starts playing the audio automatically as soon as it is ready.

## Beginner Level Introduction

### The Audio Element

Before HTML5, playing audio on a webpage required third-party plugins like Flash. Now, HTML has native support for audio using the `<audio>` tag.

To play an audio file, you provide a source URL and include the `controls` attribute so the user can interact with it.

```html
<audio src="music.mp3" controls></audio>
```

The text between the `<audio>` and `</audio>` tags will only be displayed in browsers that do not support the `<audio>` element (which is extremely rare today).

```html
<audio src="music.mp3" controls>
  Your browser does not support the audio element.
</audio>
```

## Deep Dive

### Audio Sources and Formats

Different browsers support different audio formats. The three main supported formats are:
1. **MP3**: Supported by all modern browsers.
2. **WAV**: Uncompressed audio, large file sizes. Supported by all.
3. **OGG**: Open-source format, good compression. Supported by Chrome/Firefox, but traditionally not Safari.

To ensure your audio plays in *every* browser, you can provide multiple `<source>` elements instead of a single `src` attribute. The browser will try them top-to-bottom and play the first one it understands.

```html
<audio controls>
  <source src="horse.ogg" type="audio/ogg">
  <source src="horse.mp3" type="audio/mpeg">
</audio>
```

### Autoplay and Policies

The `autoplay` attribute makes the audio start playing automatically.

```html
<audio controls autoplay>
```

**Crucial Warning:** Modern browsers (Chrome, Safari, Firefox) have strict autoplay policies. They will **block** audio from autoplaying unless the user has already interacted with the webpage (like clicking a button). This prevents annoying users with sudden loud noises when they open a tab.

If you want background audio to autoplay, you must also include the `muted` attribute. Muted autoplay is generally allowed, but then the user has to manually unmute it to hear anything.

### Other Useful Attributes

- `loop`: The audio will start over again, every time it is finished.
- `muted`: Specifies that the audio output should be muted by default.
- `preload`: Specifies if/how the author thinks the audio should be loaded when the page loads (`auto`, `metadata`, or `none`).

### Accessibility

Audio that plays automatically (and lasts more than 3 seconds) can be a massive problem for screen reader users, as the audio drowns out the screen reader's voice.
Always provide `controls` so the user can pause the audio, and avoid `autoplay` unless absolutely necessary.

## Examples

<details>
<summary><strong>Example: Robust Audio Player with Multiple Sources</strong></summary>

```html
<!-- 
  Special Attributes on <audio>:
  - controls: Shows the browser's native UI.
  - loop: Plays the track infinitely.
-->
<audio controls loop>
  
  <!-- 
    Special Attributes on <source>:
    - src: The path to the file.
    - type: The MIME type of the file. This helps the browser quickly 
      decide if it can play it without downloading it first.
  -->
  <source src="background_music.ogg" type="audio/ogg">
  <source src="background_music.mp3" type="audio/mpeg">
  
  <!-- Fallback text -->
  <p>Your browser does not support HTML5 audio. Here is a <a href="background_music.mp3">link to the audio</a> instead.</p>
  
</audio>
```

</details>

<details>
<summary><strong>Example: JavaScript Controlled Audio</strong></summary>

```html
<!-- We remove the 'controls' attribute because we are building our own UI -->
<audio id="myAudio">
  <source src="ding.mp3" type="audio/mpeg">
</audio>

<button onclick="playAudio()" type="button">Play Sound Effect</button>
<button onclick="pauseAudio()" type="button">Pause Sound Effect</button>

<script>
  const audioObj = document.getElementById("myAudio"); 

  function playAudio() { 
    // Uses the native HTMLMediaElement API
    audioObj.play(); 
  } 

  function pauseAudio() { 
    audioObj.pause(); 
  } 
</script>
```

</details>
