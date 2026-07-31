---
sidebar_position: 1
---

# 3.1 Text Formatting

## Definitions

- **Text Formatting**: The process of applying visual or semantic styles to text, such as making it bold, italicized, or highlighted.
- **Semantic Element**: An HTML element that conveys meaning about the content enclosed within it, rather than just describing how it should look visually.
- **Subscript**: Text that appears slightly below the normal line of type, often used in chemical formulas.
- **Superscript**: Text that appears slightly above the normal line of type, often used in mathematical exponents or footnotes.

## Beginner Level Introduction

HTML contains several elements used for defining text with a special meaning. While you can achieve similar visual results using CSS, using the correct HTML tags provides semantic meaning, which is crucial for SEO and accessibility (screen readers).

### Basic Formatting Elements

- `<b>`: **Bold Text**. Used to draw attention to text without conveying extra importance.
- `<strong>`: **Important Text**. Renders as bold, but semantically indicates that the text has strong importance, seriousness, or urgency.
- `<i>`: *Italic Text*. Used for text in an alternate voice or mood, or to offset it from the normal prose (e.g., technical terms, thoughts, ship names).
- `<em>`: *Emphasized Text*. Renders as italic, but semantically indicates stress emphasis (how you would change your tone of voice when speaking).

```html
<p>This is normal text, but <b>this is bold</b>.</p>
<p>This is normal text, but <i>this is italic</i>.</p>
```

## Deep Dive

### Advanced Formatting Elements

HTML provides more specialized tags for specific types of text content.

- `<mark>`: **Marked/Highlighted Text**. Represents text that has been highlighted for reference purposes (like a yellow highlighter marker).
- `<small>`: **Small Text**. Represents side comments and small print (e.g., copyright and legal text).
- `<del>`: **Deleted Text**. Represents text that has been deleted from a document. Browsers typically render this as ~~struck through~~ text.
- `<ins>`: **Inserted Text**. Represents text that has been added to a document. Browsers typically render this as <u>underlined</u> text.
- `<sub>`: **Subscript Text**. Used for typographical conventions like chemical formulas (e.g., H₂O).
- `<sup>`: **Superscript Text**. Used for mathematical exponents (e.g., E = mc²) or ordinal numbers (e.g., 1<sup>st</sup>).

### Semantic vs. Presentational

In the early days of HTML, tags like `<b>` and `<i>` were purely presentational (they just changed how things looked). In HTML5, they have been redefined to carry some semantic meaning, but `<strong>` and `<em>` are generally preferred when you want to convey actual importance or emphasis.

If you *only* want to make text bold or italic for decorative purposes, without altering its meaning, it is best practice to use CSS (`font-weight: bold` or `font-style: italic`) instead of HTML tags.

## Examples

<details>
<summary><strong>Example: Semantic Emphasis and Importance</strong></summary>

```html
<p>I <em>love</em> eating pizza.</p>
<!-- 
  The <em> tag tells screen readers to pronounce "love" with emphasis.
-->

<p><strong>Warning:</strong> Do not feed the bears.</p>
<!-- 
  The <strong> tag indicates this is highly important information.
-->
```

</details>

<details>
<summary><strong>Example: Edits and Highlights</strong></summary>

```html
<p>The concert tickets cost <del>$50</del> <ins>$40</ins> for early birds.</p>
<!-- 
  <del> shows old/deleted info (strikethrough).
  <ins> shows new/added info (underlined).
-->

<p>When studying HTML, remember the <mark>semantic elements</mark>.</p>
<!-- 
  <mark> highlights the text, usually with a yellow background.
-->
```

</details>

<details>
<summary><strong>Example: Subscript and Superscript</strong></summary>

```html
<p>The chemical formula for water is H<sub>2</sub>O.</p>
<!-- 
  <sub> lowers the "2" below the baseline.
-->

<p>The Pythagorean theorem is a<sup>2</sup> + b<sup>2</sup> = c<sup>2</sup>.</p>
<!-- 
  <sup> raises the "2" above the baseline.
-->
```

</details>
