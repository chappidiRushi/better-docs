---

id: html-under-the-hood
title: How HTML Works Under the Hood
sidebar_position: 3
-------------------

# HTML Rendering Pipeline (Under the Hood)

> From URL navigation → network → parsing → rendering → pixels on screen

---

## 1. Navigation & URL Resolution

**Definition:** Browser resolves the input URL into a concrete network request target.

Key operations:

* URL parsing (scheme, host, port, path, query, fragment)
* Default port inference (80, 443)
* HSTS upgrade (HTTP → HTTPS)
* Service Worker navigation interception (if registered)

<details>
<summary>Example</summary>

```text
https://example.com:443/app/index.html?x=1#section
└─ scheme   → https
└─ origin   → example.com:443
└─ path     → /app/index.html
└─ query    → x=1
└─ fragment → #section (client-only, not sent to server)
```

</details>

---

## 2. DNS Resolution

**Definition:** Maps hostname → IP address.

Resolution order:

* Browser DNS cache
* OS DNS cache
* Router cache
* ISP / Recursive resolver
* Authoritative DNS server

Protocols:

* UDP (default)
* TCP fallback
* DoH / DoT (modern browsers)

<details>
<summary>Example</summary>

```text
example.com
→ 93.184.216.34 (A record)
→ 2606:2800:220:1:248:1893:25c8:1946 (AAAA record)
```

</details>

---

## 3. TCP Connection / TLS Handshake

**Definition:** Establishes a reliable, secure transport channel.

Steps:

* TCP 3‑way handshake (SYN → SYN/ACK → ACK)
* TLS handshake

  * Certificate exchange
  * Key agreement
  * Cipher negotiation
* ALPN protocol selection (http/1.1 | h2 | h3)

<details>
<summary>Example</summary>

```text
Client → SYN
Server → SYN-ACK
Client → ACK
TLS → ServerCert + KeyExchange
TLS → Secure channel established
```

</details>

---

## 4. HTTP Request Dispatch

**Definition:** Browser sends an HTTP request for the HTML document.

Request details:

* Method: GET
* Headers:

  * Accept: text/html
  * User-Agent
  * Cookies
  * Cache-Control
* Conditional headers:

  * If-None-Match (ETag)
  * If-Modified-Since

<details>
<summary>Example</summary>

```http
GET /index.html HTTP/2
Host: example.com
Accept: text/html
User-Agent: Chrome/121
```

</details>

---

## 5. Server Processing & Response

**Definition:** Server generates and returns the HTML payload.

Possible server paths:

* Static file (disk / CDN)
* SSR framework (Next.js, Nuxt, etc.)
* Backend templating (PHP, Rails, Django)

Response metadata:

* Status code (200, 304, 404)
* Content-Type: text/html; charset=UTF-8
* Content-Encoding: gzip | br

<details>
<summary>Example</summary>

```http
HTTP/2 200 OK
Content-Type: text/html; charset=UTF-8
Content-Encoding: br
```

</details>

---

## 6. HTML Byte Stream Decoding

**Definition:** Converts raw bytes → Unicode code points.

Steps:

* Charset sniffing (HTTP headers → meta charset → fallback UTF‑8)
* Decompression (gzip / brotli)
* Byte → character decoding

<details>
<summary>Example</summary>

```html
<meta charset="utf-8">
```

</details>

---

## 7. Tokenization (HTML Lexer)

**Definition:** Converts character stream into HTML tokens.

Token types:

* Doctype
* Start tag
* End tag
* Comment
* Character

Special parsing states:

* RawText (script, style)
* RCDATA (textarea, title)

<details>
<summary>Example</summary>

```html
<div id="a">Hello</div>

Tokens:
- StartTag(div, id="a")
- Character(Hello)
- EndTag(div)
```

</details>

---

## 8. DOM Tree Construction

**Definition:** Tokens are converted into DOM nodes.

Rules:

* Tree correction (malformed HTML auto-fix)
* Implicit tag insertion
* Foster parenting (tables)

DOM node types:

* Document
* Element
* Text
* Comment

<details>
<summary>Example</summary>

```html
<p><div>A</p>

DOM (auto-corrected):
<p></p>
<div>A</div>
```

</details>

---

## 9. CSS Parsing → CSSOM

**Definition:** CSS files are parsed into the CSS Object Model.

Blocking behavior:

* External CSS blocks rendering
* Inline CSS parsed immediately

Steps:

* Fetch CSS
* Tokenize
* Parse rules
* Cascade resolution

<details>
<summary>Example</summary>

```css
#app { color: red; }
```

</details>

---

## 10. Render Tree Construction

**Definition:** Combines DOM + CSSOM into a renderable tree.

Rules:

* Excludes non-visual nodes (display:none)
* Includes pseudo-elements
* Each node has computed styles

<details>
<summary>Example</summary>

```text
DOM + CSSOM → Render Tree
```

</details>

---

## 11. Layout (Reflow)

**Definition:** Calculates geometry of each render node.

Outputs:

* Box model dimensions
* Positioning (static, relative, absolute, fixed)
* Line boxes

Triggers:

* DOM mutations
* Font load
* Viewport resize

<details>
<summary>Example</summary>

```text
width: 100px
height: 20px
x: 10px
y: 40px
```

</details>

---

## 12. Paint

**Definition:** Converts layout tree into drawing commands.

Operations:

* Backgrounds
* Text
* Borders
* Shadows

Paint records are rasterized later.

<details>
<summary>Example</summary>

```text
Paint → drawRect → drawText → drawBorder
```

</details>

---

## 13. Compositing & GPU Rasterization

**Definition:** Layers are composited and pushed to GPU.

Layer triggers:

* transform
* opacity
* position: fixed
* will-change

Pipeline:

* Rasterize layers
* Upload to GPU
* Composite into final frame

<details>
<summary>Example</summary>

```css
.box {
  transform: translateZ(0); /* forces compositing layer */
}
```

</details>

---

## 14. JavaScript Execution (Main Thread)

**Definition:** JS executes during parsing and after load.

Rules:

* `<script>` blocks HTML parsing
* `defer` executes after DOM built
* `async` executes ASAP

JS can trigger:

* DOM mutations
* Style recalculation
* Layout thrashing

<details>
<summary>Example</summary>

```html
<script defer src="app.js"></script>
```

</details>

---

## 15. Event Loop & Rendering Lifecycle

**Definition:** Coordinates JS, rendering, and user input.

Order:

* Macro task
* Microtasks (Promise)
* requestAnimationFrame
* Render

<details>
<summary>Example</summary>

```js
requestAnimationFrame(() => {
  element.style.left = '10px';
});
```

</details>

---

## 16. Frame Output → Screen

**Definition:** Final frame is sent to display hardware.

Steps:

* VSync
* Double buffering
* Screen refresh (60Hz / 120Hz)

<details>
<summary>Example</summary>

```text
GPU Framebuffer → Display Controller → Monitor
```

</details>

---

## 17. Continuous Updates

**Definition:** Rendering pipeline repeats on changes.

Triggers:

* User input
* Network updates
* JS timers
* Animations

Optimizations:

* Partial repaint
* Layer reuse
* Dirty rectangles

<details>
<summary>Example</summary>

```js
setInterval(() => el.textContent++, 1000);
```

</details>

---

## Mental Model (Interview One‑Liner)

> **HTML is streamed, tokenized, parsed into DOM, styled via CSSOM, merged into a render tree, laid out, painted, composited on GPU, and refreshed per frame — with JS interleaved via the event loop.**
