---
sidebar_position: 5
---

# 7.5 Performance & Resource Hints

## Definitions

- **Resource Hint**: An HTML `<link>` relation that instructs the browser to preemptively connect to a server or download a resource to improve page load speed.
- **Latency**: The time it takes for data to pass from one point on a network to another.
- **DNS Resolution**: The process of translating a domain name (like `google.com`) into an IP address.

## Beginner Level Introduction

When a user visits your website, the browser downloads the HTML file, reads it from top to bottom, and downloads resources (CSS, JavaScript, images, fonts) as it finds them.

Sometimes, the browser doesn't know it needs a resource until very late in the process. For example, the browser downloads a CSS file, reads it, and only *then* realizes it needs to download a custom web font mentioned inside the CSS. This delay causes a flash of unstyled text.

**Resource Hints** are placed in the `<head>` of your HTML document. They act as "VIP passes," telling the browser: *"Hey, I know you haven't seen the code that requires this file yet, but trust me, you are going to need it soon. Start downloading it right now!"*

## Deep Dive

Modern HTML provides several distinct resource hints via the `<link>` tag's `rel` attribute.

### 1. `preload`
`preload` is a mandatory instruction for the browser to download a specific resource immediately. Use this for critical resources required for the current page to render correctly (like a Hero image or a core CSS/Font file).

You must specify the `as` attribute so the browser knows how to prioritize and parse it.
```html
<link rel="preload" href="hero-image.webp" as="image">
<link rel="preload" href="custom-font.woff2" as="font" type="font/woff2" crossorigin>
```
*Note: If you preload a resource and don't use it within 3 seconds, the browser will log a performance warning in the console.*

### 2. `prefetch`
`prefetch` is a low-priority suggestion to the browser to download a resource in the background during idle time. Use this for resources the user *might* need on the *next* page they visit.
```html
<!-- The user is on the homepage, but they will probably click the login button next -->
<link rel="prefetch" href="/login.js">
```

### 3. `preconnect`
Establishing a secure connection to a new server takes time (DNS lookup + TCP handshake + TLS negotiation). If you know your page will load resources from a third-party domain (like Google Fonts or a CDN), you can tell the browser to set up the connection early, saving hundreds of milliseconds.
```html
<!-- Prepare the connection to Google Fonts before we actually request the font file -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
```

### 4. `dns-prefetch`
Similar to `preconnect`, but it only resolves the domain name (DNS lookup) without doing the full TCP/TLS handshake. It is lighter on resources than `preconnect` and has wider support on older browsers.
```html
<link rel="dns-prefetch" href="https://api.mybackend.com">
```

## Examples

<details>
<summary><strong>Example: An Optimized Document Head</strong></summary>

Here is an example of a highly optimized `<head>` using resource hints to improve Core Web Vitals (like Largest Contentful Paint).

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>High Performance Page</title>

  <!-- 1. Preconnect to critical third-party domains -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>

  <!-- 2. Preload the Hero image so it renders immediately -->
  <link rel="preload" href="/images/hero-banner.webp" as="image">

  <!-- 3. Preload a critical font file -->
  <!-- 
    Special Attribute: crossorigin 
    - Fonts fetched using @font-face are always fetched using anonymous-mode CORS. 
    Therefore, you MUST add the crossorigin attribute when preloading fonts, even if they are on the same server!
  -->
  <link rel="preload" href="/fonts/Roboto-Bold.woff2" as="font" type="font/woff2" crossorigin>

  <!-- 4. Normal CSS link -->
  <link rel="stylesheet" href="style.css">
  
  <!-- 5. Prefetch assets for the next logical page -->
  <link rel="prefetch" href="/checkout.css">
</head>
<body>
  ...
</body>
</html>
```

</details>
