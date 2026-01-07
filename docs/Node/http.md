# `http` / `https` Modules

> Build HTTP/HTTPS servers and clients

---

## Import (ESM)

```js
import * as http from 'node:http';
import * as https from 'node:https';
```

<details>
<summary>Examples</summary>

```js
import { createServer, request } from 'node:http';
```

</details>

---

## Core Classes

```js
// Incoming request (readable stream)
http.IncomingMessage           // class
// Server response (writable stream)
http.ServerResponse            // class
// HTTP server
http.Server                    // class
// HTTP client request
http.ClientRequest             // class
// HTTP agent (connection pooling)
http.Agent                     // class
```

<details>
<summary>Examples</summary>

```js
new http.Agent({ keepAlive: true });
```

</details>

---

## Server Creation

```js
// Create HTTP server
http.createServer(opts?, handler?)     // (object?, Function?) → Server
// Create HTTPS server
https.createServer(opts, handler?)     // (object, Function?) → Server
```

<details>
<summary>Examples</summary>

```js
http.createServer({ keepAliveTimeout: 5000 }, (req, res) => {
  res.end('ok');
});

https.createServer({
  key: 'PRIVATE_KEY',
  cert: 'CERT'
}, (req, res) => {
  res.end('secure');
});
```

</details>

---

## Server Methods

```js
// Start listening
server.listen(port, host?, backlog?, cb?) // (number, string?, number?, Function?) → Server
// Stop server
server.close(cb?)                         // (Function?) → Server
// Track connections
server.on('request', fn)                 // (req, res) → void
```

<details>
<summary>Examples</summary>

```js
server.listen(3000, '0.0.0.0', 511, () => {});
server.close(() => {});
```

</details>

---

## Request (Client)

```js
// Make HTTP request
http.request(url|opts, opts?, cb?)    // (string|object, object?, Function?) → ClientRequest
// Make GET request
http.get(url|opts, opts?, cb?)        // (string|object, object?, Function?) → ClientRequest
```

<details>
<summary>Examples</summary>

```js
http.request({
  hostname: 'example.com',
  port: 80,
  method: 'POST',
  path: '/api',
  headers: { 'Content-Type': 'application/json' }
}, res => {});

http.get('http://example.com', res => {});
```

</details>

---

## ClientRequest Methods

```js
// Write request body
req.write(chunk, enc?, cb?)         // (string|Buffer, string?, Function?) → boolean
// Finish request
req.end(data?, enc?, cb?)            // (string|Buffer?, string?, Function?) → void
// Abort request
req.abort()                          // () → void
// Set timeout
req.setTimeout(ms, cb?)              // (number, Function?) → void
```

<details>
<summary>Examples</summary>

```js
req.write('data', 'utf8', () => {});
req.end(null, null, () => {});
req.setTimeout(5000, () => {});
```

</details>

---

## IncomingMessage (Request / Response)

```js
// HTTP method
msg.method                   // string
// URL path
msg.url                      // string
// Headers map
msg.headers                  // object
// Status code (response only)
msg.statusCode               // number
```

<details>
<summary>Examples</summary>

```js
req.method;
req.url;
req.headers;
res.statusCode;
```

</details>

---

## ServerResponse Methods

```js
// Set status code
res.statusCode = code            // number
// Set response header
res.setHeader(name, value)       // (string, string|string[]) → void
// Write response body
res.write(chunk, enc?)           // (string|Buffer, string?) → boolean
// End response
res.end(data?, enc?)             // (string|Buffer?, string?) → void
```

<details>
<summary>Examples</summary>

```js
res.statusCode = 200;
res.setHeader('Content-Type', 'text/plain');
res.write('hello', 'utf8');
res.end('done', 'utf8');
```

</details>

---

## Agent

```js
// Create connection pool
new http.Agent(opts?)            // (object?) → Agent
// Destroy sockets
agent.destroy()                  // () → void
```

<details>
<summary>Examples</summary>

```js
const agent = new http.Agent({ keepAlive: true, maxSockets: 10 });
agent.destroy();
```

</details>

---

## Events

```js
// Request received
server.on('request', fn)     // (req, res) → void
// Client response
req.on('response', fn)       // (IncomingMessage) → void
// Error handling
req.on('error', fn)          // (Error) → void
```

<details>
<summary>Examples</summary>

```js
server.on('request', () => {});
req.on('response', res => {});
req.on('error', err => {});
```

</details>

---

## Notes

* `https` adds TLS on top of `http`
* Use `Agent` for connection reuse
* Streams power request/response bodies
