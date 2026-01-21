# `util` Module

> Utility functions for formatting, inspection, promisification, and debugging

---

## Import (ESM)

```js
import {
  format,
  formatWithOptions,
  inspect,
  types,
  promisify,
  callbackify,
  inherits,
  debuglog,
  deprecate,
  isDeepStrictEqual,
  getSystemErrorName,
  getSystemErrorMap,
  toUSVString,
  stripVTControlCharacters,
  TextEncoder,
  TextDecoder
} from 'node:util';
```

<details>
<summary>Examples</summary>

```js
import { format, inspect, promisify } from 'node:util';
```

</details>

---

## Formatting

```js
// Format string like printf
format(fmt, ...args)                    // (string, ...any[]) → string
// Format with inspect options
formatWithOptions(opts, fmt, ...args)   // (object, string, ...any[]) → string
```

<details>
<summary>Examples</summary>

```js
format('%s:%d', 'port', 3000);                 // fmt, args
formatWithOptions({ colors: true }, '%o', {}); // options, fmt, args
```

</details>

---

## Object Inspection

```js
// Convert object to readable string
inspect(obj, options?)                  // (any, object?) → string
// Type-checking utilities
types                                     // object
```

<details>
<summary>Examples</summary>

```js
inspect({ a: 1 }, { depth: null, colors: true }); // obj, options
types.isDate(new Date());                         // type check
```

</details>

---

## Promisification

```js
// Convert callback API to promise
promisify(fn)                          // (Function) → Function
// Convert promise API to callback
callbackify(fn)                        // (Function) → Function
```

<details>
<summary>Examples</summary>

```js
const readFileAsync = promisify(fs.readFile);   // fn
const cbFn = callbackify(async () => 'ok');     // fn
```

</details>

---

## Inheritance & Debugging

```js
// Set up prototype inheritance
inherits(ctor, superCtor)             // (Function, Function) → void
// Create conditional debug logger
debuglog(section)                     // (string) → Function
```

<details>
<summary>Examples</summary>

```js
inherits(Sub, Base);                   // ctor, superCtor
const debug = debuglog('app');         // section
debug('message');
```

</details>

---

## Deprecation

```js
// Mark function as deprecated
deprecate(fn, msg, code?)              // (Function, string, string?) → Function
```

<details>
<summary>Examples</summary>

```js
const oldFn = deprecate(() => {}, 'deprecated', 'DEP001');
oldFn();
```

</details>

---

## Equality & Errors

```js
// Deep strict equality check
isDeepStrictEqual(a, b)               // (any, any) → boolean
// Get system error name from errno
getSystemErrorName(err)               // (number) → string
// Map of all system errors
getSystemErrorMap()                   // () → Map
```

<details>
<summary>Examples</summary>

```js
isDeepStrictEqual({ a: 1 }, { a: 1 }); // values
getSystemErrorName(-2);               // errno
getSystemErrorMap();                  // no args
```

</details>

---

## String Utilities

```js
// Convert to valid UTF‑16 string
toUSVString(str)                      // (string) → string
// Remove terminal escape codes
stripVTControlCharacters(str)         // (string) → string
```

<details>
<summary>Examples</summary>

```js
toUSVString('\uD800');
stripVTControlCharacters('[31mred');
```

</details>

---

## Text Encoding

```js
// Encode string to Uint8Array
new TextEncoder()                     // () → TextEncoder
// Decode buffer to string
new TextDecoder(enc?, opts?)          // (string?, object?) → TextDecoder
```

<details>
<summary>Examples</summary>

```js
const enc = new TextEncoder();
enc.encode('text');                   // string
const dec = new TextDecoder('utf-8', { fatal: false });
dec.decode(Buffer.from('text'));
```

</details>

---

## Notes

* `util.promisify` underpins many Node promise APIs
* `inspect` powers console output
* Prefer `types` over manual `typeof` checks
