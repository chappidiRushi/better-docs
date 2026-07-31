---
sidebar_position: 6
---

# 17.6 URL Encode

## Definitions

- **URL (Uniform Resource Locator)**: The web address of a specific resource.
- **URL Encoding (Percent-encoding)**: A mechanism for encoding information in a Uniform Resource Identifier (URI) under certain circumstances.

## Beginner Level Introduction

URLs can only be sent over the Internet using the **ASCII character-set**. 

Because URLs often contain characters outside the ASCII set (like spaces, accented letters, or special symbols like `?` or `&`), the URL has to be converted into a valid ASCII format. 

URL encoding replaces unsafe ASCII characters with a `%` followed by two hexadecimal digits. 

For example, a URL cannot contain a space. URL encoding normally replaces a space with a plus sign (`+`) or with `%20`.

## Deep Dive

### How URL Encoding Works

If a user submits an HTML form using the `GET` method, the browser automatically URL-encodes the form data before appending it to the URL.

If the user searches for `Hello World`, the browser converts it to:
`?query=Hello%20World`

If the user searches for `$100`, the browser converts it to:
`?query=%24100`

### Common URL Encoded Characters

| Character | URL Encoded |
|---|---|
| ` ` (space) | `%20` or `+` |
| `!` | `%21` |
| `"` | `%22` |
| `#` | `%23` |
| `$` | `%24` |
| `%` | `%25` |
| `&` | `%26` |
| `'` | `%27` |
| `+` | `%2B` |
| `,` | `%2C` |

*(Note: You will frequently see `%20` in the address bar if a website doesn't use dashes to separate words in its page names).*

### JavaScript Functions

If you are building dynamic URLs using JavaScript (e.g., calling an API with a user-provided search term), you must encode the strings manually before making the request.

JavaScript provides two main functions for this:
- `encodeURI()`: Encodes a full URL. It ignores protocol prefixes (like `http://`) and domain separators (`/`, `?`, `=`).
- `encodeURIComponent()`: Encodes a specific component of a URL (like a single query parameter). It encodes *everything*, including `/` and `?`. **This is the one you will use most often.**

## Examples

<details>
<summary><strong>Example: Browser Auto-Encoding in Forms</strong></summary>

```html
<!-- 
  If the user types "Fish & Chips" into the input and clicks submit...
-->
<form action="/search" method="GET">
  <input type="text" name="q">
  <button type="submit">Search</button>
</form>

<!-- 
  The browser will automatically navigate to:
  /search?q=Fish+%26+Chips 
  (The ampersand is encoded to %26, otherwise the server would think it was a new parameter)
-->
```

</details>

<details>
<summary><strong>Example: JavaScript URL Encoding</strong></summary>

```html
<script>
  const baseUrl = "https://api.example.com/search?query=";
  const userInput = "Ben & Jerry's 100%";
  
  // BAD: https://api.example.com/search?query=Ben & Jerry's 100% (Will break the request)
  const badUrl = baseUrl + userInput; 
  
  // GOOD: https://api.example.com/search?query=Ben%20%26%20Jerry's%20100%25
  const safeParam = encodeURIComponent(userInput);
  const goodUrl = baseUrl + safeParam;
  
  console.log("Safe URL:", goodUrl);
</script>
```

</details>
