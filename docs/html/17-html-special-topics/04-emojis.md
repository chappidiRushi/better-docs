---
sidebar_position: 4
---

# 17.4 Emojis

## Definitions

- **Emoji**: Ideograms and smileys used in electronic messages and web pages.
- **UTF-8**: The dominant character encoding for the World Wide Web, which includes the Unicode numbers for all emojis.

## Beginner Level Introduction

Emojis look like images, or icons, but they are not. 

They are letters (characters) from the UTF-8 character set. Because they are characters, you can copy and paste them directly into your HTML code, and you can change their size using the CSS `font-size` property!

To display emojis correctly, your HTML document MUST include the UTF-8 meta tag in the `<head>`:

```html
<meta charset="UTF-8">
```

## Deep Dive

### How Emojis Work

Just like the letter "A" is represented by the number 65, emojis are represented by numbers in the Unicode standard.

For example, the Grinning Face emoji (😀) is character number `128512`.

You can display an emoji in HTML in two ways:
1. **Directly copy-pasting**: Type or paste the emoji character directly into your code: `😀`.
2. **Using the Entity Number**: Use the `&#` format followed by the decimal number of the emoji: `&#128512;`.

### Why Emojis Look Different on Different Devices

Because emojis are essentially text characters, it is up to the operating system's font engine (Apple, Google, Microsoft) to decide *how* that character is drawn. 

This is why the hamburger emoji looks different on an iPhone compared to an Android phone or a Windows computer. The HTML code is exactly the same, but the device's native font renders it in its own unique style.

## Examples

<details>
<summary><strong>Example: Emojis and CSS</strong></summary>

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <style>
    .emoji-large {
      font-size: 48px; /* Scales the emoji up perfectly */
    }
    
    .emoji-rotated {
      font-size: 48px;
      display: inline-block;
      transform: rotate(45deg); /* You can rotate them like text/images */
    }
  </style>
</head>
<body>

  <!-- Using direct characters -->
  <p>I love HTML! ❤️</p>
  
  <!-- Using Entity Numbers -->
  <p>I love coding! &#128187;</p>

  <!-- Styling Emojis -->
  <p class="emoji-large">🚀</p>
  <p class="emoji-rotated">🚀</p>

</body>
</html>
```

</details>
