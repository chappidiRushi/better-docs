---
sidebar_position: 5
---

# 17.5 HTML Charsets

## Definitions

- **Charset (Character Set)**: A defined list of characters recognized by the computer hardware and software.
- **ASCII**: The first character encoding standard, consisting of 128 english characters and numbers.
- **UTF-8**: The modern Unicode standard that covers almost all characters, punctuations, and symbols in the world.

## Beginner Level Introduction

To display an HTML page correctly, a web browser must know the character set (character encoding) used in the page.

If the browser guesses the wrong character set, text like "café" might render as "caf" or something even stranger.

To prevent this, you should always declare the character set in the `<head>` of your HTML document using the `<meta>` tag:

```html
<meta charset="UTF-8">
```

## Deep Dive

### The History of Character Encodings

1. **ASCII (American Standard Code for Information Interchange)**: The original standard. It only defined 128 characters (English letters A-Z, numbers 0-9, and basic punctuation). It could not display accented letters or non-English alphabets.
2. **ISO-8859-1**: The default character set for HTML 4. It supported 256 different character codes, which allowed it to cover most Western European languages.
3. **ANSI (Windows-1252)**: The original Windows character set, very similar to ISO-8859-1.

### The Rise of UTF-8

Because ISO-8859-1 was limited in size, it was not compatible with multilingual environments (e.g., displaying Russian, Chinese, and English on the same page).

The Unicode Consortium developed the Unicode Standard to cover all characters, punctuations, and symbols globally. 
**UTF-8** is a specific encoding of the Unicode standard.

HTML5 demands UTF-8 as the default character encoding. Today, over 98% of all websites use UTF-8. 

**Rule of Web Development:** Always save your code editor files with UTF-8 encoding, and always declare `<meta charset="UTF-8">` in your HTML.

## Examples

<details>
<summary><strong>Example: Proper Document Structure</strong></summary>

```html
<!DOCTYPE html>
<!-- lang="en" helps screen readers and search engines -->
<html lang="en">
<head>
  <!-- 
    The charset MUST be declared within the first 1024 bytes of the document.
    It should always be the very first element inside the <head>.
  -->
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Understanding Charsets</title>
</head>
<body>
  
  <h1>Greetings</h1>
  <p>Hello (English)</p>
  <p>Hola (Spanish)</p>
  <p>こんにちは (Japanese)</p>
  <p>Привет (Russian)</p>
  
  <p>Because we used UTF-8, all these languages render perfectly on the same page!</p>

</body>
</html>
```

</details>
