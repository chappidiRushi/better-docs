---
sidebar_position: 10
---

# 17.10 Security & CORS

## Definitions

- **CORS (Cross-Origin Resource Sharing)**: A mechanism that allows restricted resources on a web page to be requested from another domain outside the domain from which the first resource was served.
- **CSP (Content Security Policy)**: An added layer of security that helps to detect and mitigate certain types of attacks, including Cross-Site Scripting (XSS) and data injection attacks.
- **XSS (Cross-Site Scripting)**: A vulnerability where an attacker injects malicious executable scripts into the code of a trusted application or website.

## Beginner Level Introduction

When you build a website, you are often loading assets (images, fonts, scripts) from other websites. 
For example, you might load a font from Google Fonts, an image from an Amazon S3 bucket, and a script from a CDN.

By default, the browser's **Same-Origin Policy** strictly controls how documents or scripts loaded from one origin (your website) can interact with a resource from another origin (a third-party server). 

To manage these interactions securely, HTML provides several attributes and meta tags to define exactly what is allowed and what is forbidden.

## Deep Dive

### Content Security Policy (CSP)

The most effective way to protect your users from XSS attacks is to implement a Content Security Policy. While this is usually done via HTTP response headers sent by your backend server, you can also define it directly in the `<head>` of your HTML document using the `<meta>` tag.

A CSP tells the browser exactly which domains are allowed to load scripts, styles, or images. If an attacker manages to inject a malicious script into your HTML, the browser will refuse to run it because the attacker's domain is not on your CSP "whitelist."

```html
<!-- 
  This policy says: 
  "Only allow resources from my own domain ('self'), 
  and only allow scripts specifically from https://apis.google.com" 
-->
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self' https://apis.google.com;">
```

### The `crossorigin` Attribute

When you request a resource from a different domain (like drawing a third-party image onto a `<canvas>` or fetching a third-party font), the browser needs to know if it should send user credentials (like cookies) with the request.

The `crossorigin` attribute dictates this behavior on `<audio>`, `<img>`, `<link>`, `<script>`, and `<video>` tags.

- `crossorigin="anonymous"`: The browser fetches the resource without sending user cookies or HTTP authentication. (This is the default if you just write `crossorigin`).
- `crossorigin="use-credentials"`: The browser sends user cookies/authentication with the request.

### The `referrerpolicy` Attribute

When a user clicks a link on your site to go to another site (or when an image loads from another site), the browser usually sends the URL of *your* page to the *other* site in the `Referer` HTTP header. 
This can leak sensitive data (e.g., if your URL is `https://yoursite.com/password-reset?token=123`).

The `referrerpolicy` attribute allows you to control what information is sent.
- `no-referrer`: The browser will not send the `Referer` header at all.
- `origin`: The browser will only send the domain name (e.g., `https://yoursite.com/`), not the full path or query parameters.

### Iframe Security (`sandbox`)

Embedding an `<iframe>` from a third party is inherently dangerous. The third party could run malicious scripts, open popups, or try to submit forms on behalf of the user.

The `sandbox` attribute restricts what the iframe can do. By default, adding `sandbox` disables absolutely everything (no scripts, no popups, no form submissions). You can then selectively re-enable specific permissions.

```html
<!-- The iframe can run scripts and submit forms, but it CANNOT open popups -->
<iframe src="https://example.com" sandbox="allow-scripts allow-forms"></iframe>
```

## Examples

<details>
<summary><strong>Example: Secure Third-Party Links</strong></summary>

When you link to another website and open it in a new tab (`target="_blank"`), the new tab historically gained partial access to your original tab's `window` object via `window.opener`. This was a major security risk (tabnabbing).

Modern browsers now implicitly apply `rel="noopener"` to `target="_blank"` links, but it is best practice to always include it explicitly for older browsers.

```html
<!-- 
  Special Attributes: 
  - target="_blank": Opens in a new tab.
  - rel="noopener noreferrer": Protects against tabnabbing and hides your URL from the destination site.
-->
<a href="https://untrustedsite.com" target="_blank" rel="noopener noreferrer">Visit Site</a>
```

</details>

<details>
<summary><strong>Example: Handling CORS with Canvas</strong></summary>

If you draw an image onto a `<canvas>`, and that image came from a different domain without the proper CORS headers, the canvas becomes "tainted." You will not be allowed to read the pixel data out of it (to prevent you from stealing data from other sites).

```html
<!-- 
  Special Attribute: crossorigin="anonymous"
  - We ask the remote server to grant us permission to use this image in our canvas.
  - If the server replies with the correct "Access-Control-Allow-Origin" header, the canvas will not be tainted.
-->
<img src="https://remote-server.com/photo.jpg" crossorigin="anonymous" id="myImage">
```

</details>
