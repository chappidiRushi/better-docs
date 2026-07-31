---
sidebar_position: 1
---

# 12.1 Forms Introduction

## Definitions

- **Form**: An HTML element (`<form>`) used to collect user input, which is most often sent to a server for processing.
- **Form Data**: The specific information (text, numbers, files) entered by the user into the form's input fields.
- **Action**: The URL of the server-side script or endpoint that will process the form data.
- **Method**: The HTTP method used to send the data (usually `GET` or `POST`).

## Beginner Level Introduction

### The Form Element

Whenever you log into a website, search for a product, or buy something online, you are using an HTML form. 

The `<form>` element is a container for different types of input elements, such as text fields, checkboxes, radio buttons, and submit buttons.

```html
<form>
  <!-- Form input elements go here -->
</form>
```

By itself, the `<form>` tag doesn't display anything on the screen. It acts as an invisible wrapper that groups the input fields together so they can be submitted as a single package.

## Deep Dive

### Form Submission

To make a form work, you need two essential attributes on the `<form>` tag: `action` and `method`.

- `action`: Where should the browser send the data? This is usually a URL pointing to a backend server (like a PHP script, a Node.js endpoint, or a Python Django view).
- `method`: How should the browser send the data? The two most common methods are `GET` and `POST`.

### The GET Method

When you use `method="GET"`, the browser takes all the form data and appends it directly to the URL in the address bar as "query parameters".

```html
<form action="/search" method="GET">
```
If a user searches for "shoes", the resulting URL looks like this: `https://example.com/search?query=shoes`.

**When to use GET:**
- For non-sensitive data (like search queries).
- When you want the user to be able to bookmark the result page.
- **Never** use GET for passwords, credit card numbers, or sensitive information, because it will be visible in the URL history and server logs.
- GET requests have a length limit (around 2048 characters).

### The POST Method

When you use `method="POST"`, the browser sends the form data hidden inside the body of the HTTP request. It does not appear in the URL.

```html
<form action="/login" method="POST">
```

**When to use POST:**
- For sensitive data (passwords, personal info).
- When the form submission will change something on the server (like updating a database, creating a new user, or deleting an item).
- When sending large amounts of data (like uploading a file), as POST has no size limitations.

### Form Security

Forms are the primary way hackers attack websites. Two massive vulnerabilities are:

1. **XSS (Cross-Site Scripting)**: A hacker enters malicious JavaScript into a text input. If the backend doesn't sanitize (clean) the input before displaying it back on the site, the script runs in the browsers of other users.
2. **CSRF (Cross-Site Request Forgery)**: A hacker creates a fake form on a different website that submits data to *your* website, tricking the browser into thinking the user performed the action. (Usually solved with hidden CSRF tokens in the form).

*Note: As an HTML developer, you can use client-side validation to help users, but **security is always the responsibility of the backend server**.*

## Examples

<details>
<summary><strong>Example: A Basic GET Form (Search)</strong></summary>

```html
<!-- 
  Special Attributes:
  - action: The endpoint that handles the search.
  - method: GET, so the search query is put into the URL.
-->
<form action="https://www.google.com/search" method="GET">
  
  <label for="q">Search Google:</label>
  
  <!-- 
    The 'name' attribute is critical! 
    Google's servers look for a parameter named 'q'.
  -->
  <input type="text" id="q" name="q">
  
  <button type="submit">Search</button>
  
</form>
```

</details>

<details>
<summary><strong>Example: A Basic POST Form (Login)</strong></summary>

```html
<!-- 
  Special Attributes:
  - action: The server endpoint that handles authentication.
  - method: POST, so the password is hidden in the HTTP body, not the URL.
-->
<form action="/api/login" method="POST">
  
  <label for="username">Username:</label>
  <input type="text" id="username" name="username" required>
  <br><br>
  
  <label for="password">Password:</label>
  <input type="password" id="password" name="password" required>
  <br><br>
  
  <button type="submit">Login</button>
  
</form>
```

</details>
