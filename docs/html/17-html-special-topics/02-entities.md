---
sidebar_position: 2
---

# 17.2 HTML Entities

## Definitions

- **Reserved Characters**: Characters that have a special meaning in HTML (like `<` and `>`).
- **Entity**: A string of text that begins with an ampersand (`&`) and ends with a semicolon (`;`), used to display reserved characters or invisible characters.
- **Non-breaking Space (`&nbsp;`)**: A space character that prevents an automatic line break at its position.

## Beginner Level Introduction

Some characters are reserved in HTML.
If you use the less than (`<`) or greater than (`>`) signs in your text, the browser might mix them up with HTML tags.

For example, if you want to write the mathematical equation "5 < 10" on your webpage, you cannot type `5 < 10` in your HTML file. The browser will see the `<` and think you are trying to open a new HTML tag.

To display these reserved characters, you must use **Character Entities**.

An HTML entity looks like this: `&entity_name;` OR `&#entity_number;`

To write "5 less than 10", you write:
```html
<p>5 &lt; 10</p> 
<!-- Output: 5 < 10 -->
```

## Deep Dive

### Common HTML Entities

Here are the most common entities every web developer must know:

| Result | Description | Entity Name | Entity Number |
|---|---|---|---|
| `<` | Less than | `&lt;` | `&#60;` |
| `>` | Greater than | `&gt;` | `&#62;` |
| `&` | Ampersand | `&amp;` | `&#38;` |
| `"` | Double quotation | `&quot;` | `&#34;` |
| `'` | Single quotation | `&apos;` | `&#39;` |
| | Non-breaking space | `&nbsp;` | `&#160;` |

*Note: Entity names are case-sensitive! `&lt;` is not the same as `&LT;`.*

### The Non-Breaking Space (`&nbsp;`)

Browsers truncate spaces in HTML pages. If you write 10 spaces in your text, the browser will remove 9 of them and display just 1.

If you absolutely need extra spacing, or if you want to ensure two words stay together on the same line (like "10 km/h"), you use the non-breaking space entity.

```html
<p>10&nbsp;km/h</p>
```

### Entity Name vs Number

Using an entity name (like `&copy;` for the copyright symbol) is easier to remember.
However, using the entity number (like `&#169;`) is considered slightly more reliable because not all browsers support all the newer entity names, but they all support the numbers.

## Examples

<details>
<summary><strong>Example: Displaying HTML Code on a Webpage</strong></summary>

```html
<p>To create a paragraph, use the following code:</p>

<!-- 
  If we didn't use entities here, the browser would just render a blank paragraph.
  Instead, it renders exactly: <p>Hello</p>
-->
<code>&lt;p&gt;Hello&lt;/p&gt;</code>
```

</details>

<details>
<summary><strong>Example: Using Non-Breaking Spaces</strong></summary>

```html
<!-- The browser will collapse the spaces between these words into one space. -->
<p>This    has    too    many    spaces.</p>

<!-- To force the spaces, we use &nbsp; -->
<p>This&nbsp;&nbsp;&nbsp;&nbsp;forces&nbsp;&nbsp;&nbsp;&nbsp;spacing.</p>
```

</details>
