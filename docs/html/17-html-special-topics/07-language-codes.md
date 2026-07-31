---
sidebar_position: 7
---

# 17.7 Language Codes

## Definitions

- **Language Code**: A standardized two-letter (or sometimes three-letter) code used to define a specific human language.
- **ISO 639-1**: The international standard for language codes (e.g., "en" for English, "fr" for French).

## Beginner Level Introduction

You should always include the `lang` attribute inside the `<html>` tag to declare the language of the Web page.

This is meant to assist search engines and screen readers. For example, if a screen reader knows the page is in French, it will use a French pronunciation engine to read the text.

```html
<!DOCTYPE html>
<html lang="en">
<body>
...
</body>
</html>
```

## Deep Dive

### Language and Country Codes

Sometimes, knowing the language isn't enough. You also need to know the regional dialect. You can append a country code to the language code to be more specific.

- `en`: English
- `en-US`: English (United States)
- `en-GB`: English (Great Britain)

- `fr`: French
- `fr-FR`: French (France)
- `fr-CA`: French (Canada)

### Why is this important?

1. **Accessibility**: As mentioned, screen readers rely entirely on this attribute to select the correct voice profile.
2. **Translation Tools**: Browsers like Google Chrome use this attribute to decide whether to offer the user a translation of the page. If an English user lands on an `<html>` page with `lang="es"`, Chrome will pop up the "Translate this page to English?" banner.
3. **Typography**: Some fonts render characters slightly differently depending on the language (e.g., Chinese vs. Japanese Kanji).

## Examples

<details>
<summary><strong>Example: Setting the Document Language</strong></summary>

```html
<!DOCTYPE html>
<!-- Specifies US English -->
<html lang="en-US">
<head>
  <meta charset="UTF-8">
  <title>My US English Page</title>
</head>
<body>
  <p>Color, standard, and center.</p>
</body>
</html>
```

</details>

<details>
<summary><strong>Example: Language Changes within a Document</strong></summary>

If a specific section of your page uses a different language than the main document, you can apply the `lang` attribute to a specific element. Screen readers will switch voices just for that sentence!

```html
<!DOCTYPE html>
<html lang="en">
<body>
  
  <p>The English translation of the famous Spanish phrase is "Good morning".</p>
  
  <!-- The screen reader will read this sentence with a Spanish accent -->
  <p lang="es">Buenos días.</p>

</body>
</html>
```

</details>
