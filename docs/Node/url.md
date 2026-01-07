# `url` Module

> URL parsing, formatting, resolution, and WHATWG URL utilities

---

## Import (ESM)

```js
import {
  URL,
  URLSearchParams,
  fileURLToPath,
  pathToFileURL,
  urlToHttpOptions,
  domainToASCII,
  domainToUnicode
} from 'node:url';
```

<details>
<summary>Examples</summary>

```js
import { URL, URLSearchParams } from 'node:url';
```

</details>

---

## WHATWG URL Class

```js
// Create and parse a URL
new URL(input, base?)            // (string, string|URL?) → URL
```

<details>
<summary>Examples</summary>

```js
new URL('https://example.com/path?x=1', 'https://base.com'); // input, base
```

</details>

---

## URL Properties

```js
// Full serialized URL
url.href                         // string
// Protocol (scheme)
url.protocol                     // string
// Hostname + port
url.host                         // string
// Hostname only
url.hostname                     // string
// Port number
url.port                         // string
// Pathname
url.pathname                     // string
// Query string
url.search                       // string
// Hash fragment
url.hash                         // string
// URL origin
url.origin                       // string
// URL username
url.username                     // string
// URL password
url.password                     // string
// URLSearchParams instance
url.searchParams                 // URLSearchParams
```

<details>
<summary>Examples</summary>

```js
const url = new URL('https://user:pass@ex.com:8080/p?q=1#h');
url.href;
url.protocol;
url.host;
url.hostname;
url.port;
url.pathname;
url.search;
url.hash;
url.origin;
url.username;
url.password;
url.searchParams;
```

</details>

---

## URL Methods

```js
// Convert URL to JSON
url.toJSON()                     // () → string
// Convert URL to string
url.toString()                   // () → string
```

<details>
<summary>Examples</summary>

```js
url.toJSON();                     // no args
url.toString();                   // no args
```

</details>

---

## URLSearchParams Class

```js
// Create query string params
new URLSearchParams(init?)        // (string|object|Iterable?) → URLSearchParams
```

<details>
<summary>Examples</summary>

```js
new URLSearchParams('a=1&b=2');                 // string
new URLSearchParams({ a: 1, b: 2 });             // object
new URLSearchParams([['a', '1'], ['b', '2']]);   // iterable
```

</details>

---

## URLSearchParams Methods

```js
// Append a key/value pair
params.append(name, value)       // (string, string) → void
// Delete a key
params.delete(name)              // (string) → void
// Get first value
params.get(name)                 // (string) → string|null
// Get all values
params.getAll(name)              // (string) → string[]
// Check existence
params.has(name)                 // (string) → boolean
// Set value (overwrite)
params.set(name, value)          // (string, string) → void
// Sort parameters
params.sort()                    // () → void
// Iterate entries
params.entries()                 // () → Iterator<[string,string]>
// Convert to string
params.toString()                // () → string
```

<details>
<summary>Examples</summary>

```js
const params = new URLSearchParams('a=1&a=2');
params.append('b', '3');          // name, value
params.delete('a');               // name
params.get('b');                  // name
params.getAll('a');               // name
params.has('b');                  // name
params.set('a', '9');             // name, value
params.sort();                    // no args
[...params.entries()];            // iterator
params.toString();                // string
```

</details>

---

## File URL Conversion

```js
// Convert file URL to path
fileURLToPath(url)               // (URL|string) → string
// Convert path to file URL
pathToFileURL(path)              // (string) → URL
```

<details>
<summary>Examples</summary>

```js
fileURLToPath(new URL('file:///a/b.txt')); // URL
pathToFileURL('/a/b.txt');                 // path
```

</details>

---

## Legacy / Utilities

```js
// Convert URL to http.request options
urlToHttpOptions(url)            // (URL) → object
// Convert domain to ASCII (punycode)
domainToASCII(domain)            // (string) → string
// Convert domain to Unicode
domainToUnicode(domain)          // (string) → string
```

<details>
<summary>Examples</summary>

```js
urlToHttpOptions(new URL('https://ex.com:8080')); // URL
domainToASCII('mañana.com');                     // domain
domainToUnicode('xn--maana-pta.com');             // domain
```

</details>

---

## Notes

* Prefer WHATWG `URL` over legacy `url.parse`
* `URLSearchParams` preserves insertion order
* File URLs always use `file://`
