---
sidebar_position: 3
---

# 16.3 Server-Sent Events (SSE)

## Definitions

- **Server-Sent Events (SSE)**: A standard allowing a web page to get updates from a server automatically.
- **One-way messaging**: Communication flows in a single direction (from server to client).
- **EventSource API**: The JavaScript interface used to connect to the server and receive the events.

## Beginner Level Introduction

Historically, the web worked on a strict "request-response" model. The client (browser) asks the server for data, and the server replies. The server could never initiate contact with the browser. 

If you wanted a live-updating news feed, or a live sports score, you had to write JavaScript that asked the server "Do you have new data?" every 5 seconds. This is called **polling**, and it is very inefficient.

HTML5 introduced **Server-Sent Events (SSE)**. It allows a web page to open a connection to the server, and then the server can push updates to the web page automatically whenever it wants.

Examples of SSE use cases:
- Live stock prices
- Twitter/X timeline updates
- Live sports scores
- Continuous server status monitoring

## Deep Dive

### How SSE Works (The Client Side)

Connecting to a server to receive SSE is incredibly simple using the JavaScript `EventSource` object.

1. Create a new `EventSource`, passing the URL of the server script that generates the events.
2. Listen for the `onmessage` event.

```javascript
const source = new EventSource("demo_sse.php");

source.onmessage = function(event) {
  console.log("New data from server:", event.data);
};
```

### SSE vs WebSockets

SSE is often compared to WebSockets, but they serve different purposes:

- **SSE (Server-Sent Events)**: 
  - One-way communication (Server -&gt; Client).
  - Uses standard HTTP protocols (easier to setup through firewalls and load balancers).
  - Has built-in auto-reconnection if the connection drops.
  - Best for: Live feeds, notifications, dashboards.
- **WebSockets**: 
  - Two-way communication (Client &lt;-&gt; Server).
  - Uses a custom protocol (`ws://`).
  - No built-in auto-reconnect (you have to write it yourself).
  - Best for: Chat applications, multiplayer games.

### The Server Side

For SSE to work, the server must respond with a specific header and data format. This is backend code (e.g., Node.js, Python, PHP), not HTML, but it's important to understand.

The server must:
1. Set the `Content-Type` header to `text/event-stream`.
2. Keep the connection open.
3. Send data in a specific format starting with `data: ` and ending with two newline characters `\n\n`.

## Examples

<details>
<summary><strong>Example: Client-Side SSE Implementation</strong></summary>

```html
<h1>Live Stock Prices</h1>
<div id="result">Waiting for updates...</div>

<script>
  // 1. Check for browser support
  if(typeof(EventSource) !== "undefined") {
    
    // 2. Connect to the server endpoint
    // (This URL would be your actual backend server)
    const source = new EventSource("/api/live-stock-prices");
    
    // 3. Listen for incoming messages
    source.onmessage = function(event) {
      // event.data contains the string sent by the server
      document.getElementById("result").innerHTML += event.data + "<br>";
    };
    
    // Optional: Handle errors or disconnects
    source.onerror = function(event) {
      console.log("Connection lost. Browser will auto-reconnect.");
    }
    
  } else {
    document.getElementById("result").innerHTML = "Sorry, your browser does not support server-sent events...";
  }
</script>
```

</details>

<details>
<summary><strong>Example: What the Server Sends (Backend Perspective)</strong></summary>

While you don't write this in HTML, this is what the browser expects to receive from the server:

```text
HTTP/1.1 200 OK
Content-Type: text/event-stream
Cache-Control: no-cache
Connection: keep-alive

data: {"ticker": "AAPL", "price": 150.25}

data: {"ticker": "GOOGL", "price": 2800.10}

```
Every time the server pushes a block starting with `data: ` and ending with two newlines, the `onmessage` event fires in the browser's JavaScript.

</details>
