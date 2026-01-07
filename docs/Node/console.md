# `console` Module

> Stdout / stderr logging, formatting, timers, counters

---

## Import (ESM)

```js
import console from 'node:console';
```

<details>
<summary>Examples</summary>

```js
import console from 'node:console';
```

</details>

---

## Console Methods — Logging

```js
console.log(...data)              // (...any[]) → void — stdout
console.info(...data)             // (...any[]) → void — alias of log
console.debug(...data)            // (...any[]) → void — alias of log
console.warn(...data)             // (...any[]) → void — stderr
console.error(...data)            // (...any[]) → void — stderr
```

<details>
<summary>Examples</summary>

```js
console.log('a', 1, {}, []);       // any args
console.info('info', true);        // any args
console.debug('debug', { a: 1 });  // any args
console.warn('warn', new Error()); // any args
console.error('err', 500);         // any args
```

</details>

---

## Formatting / Inspection

```js
console.dir(obj, options?)         // (any, object?) → void
console.table(data, properties?)  // (object|Array, string[]?) → void
```

<details>
<summary>Examples</summary>

```js
console.dir({ a: { b: 1 } }, { depth: null, colors: true }); // object, options
console.table([{ a: 1 }, { a: 2 }], ['a']);                  // data, properties
```

</details>

---

## Assertions

```js
console.assert(value, ...message)  // (any, ...any[]) → void
```

<details>
<summary>Examples</summary>

```js
console.assert(1 === 2, 'fail', { expected: 2 }); // condition, message
```

</details>

---

## Counters

```js
console.count(label?)              // (string?) → void
console.countReset(label?)         // (string?) → void
```

<details>
<summary>Examples</summary>

```js
console.count('req');              // label
console.count('req');              // increments
console.countReset('req');         // reset label
```

</details>

---

## Timers

```js
console.time(label)                // (string) → void
console.timeLog(label, ...data)    // (string, ...any[]) → void
console.timeEnd(label)             // (string) → void
```

<details>
<summary>Examples</summary>

```js
console.time('db');                // label
console.timeLog('db', 'step1');    // label, data
console.timeEnd('db');             // label
```

</details>

---

## Stack / Groups

```js
console.trace(...message)          // (...any[]) → void
console.group(...label)            // (...any[]) → void
console.groupCollapsed(...label)  // (...any[]) → void
console.groupEnd()                 // () → void
```

<details>
<summary>Examples</summary>

```js
console.trace('trace here');        // message
console.group('grp', 1);            // label args
console.log('inside');
console.groupCollapsed('grp2');     // collapsed group
console.groupEnd();                 // end
console.groupEnd();                 // end
```

</details>

---

## Clearing / Streams

```js
console.clear()                    // () → void
```

<details>
<summary>Examples</summary>

```js
console.clear();                   // clears TTY if supported
```

</details>

---

## Console Class

```js
new console.Console(stdout, stderr?, ignoreErrors?) // (Writable, Writable?, boolean?) → Console
```

<details>
<summary>Examples</summary>

```js
import fs from 'node:fs';
const out = fs.createWriteStream('out.log');
const err = fs.createWriteStream('err.log');
const c = new console.Console(out, err, true); // stdout, stderr, ignoreErrors
c.log('file');
c.error('err');
```

</details>

---

## Notes

* Formatting uses `util.format`
* `warn` / `error` write to **stderr**
* Timers & counters are label‑scoped
