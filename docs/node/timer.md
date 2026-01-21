# `timers` Module

> Schedule execution using timeouts, intervals, immediates, and promises

---

## Import (ESM)

```js
import {
  setTimeout,
  clearTimeout,
  setInterval,
  clearInterval,
  setImmediate,
  clearImmediate
} from 'node:timers';

import {
  setTimeout as setTimeoutP,
  setInterval as setIntervalP,
  setImmediate as setImmediateP
} from 'node:timers/promises';
```

<details>
<summary>Examples</summary>

```js
import { setTimeout, setInterval } from 'node:timers';
import { setTimeout as delay } from 'node:timers/promises';
```

</details>

---

## Timeout

```js
// Execute function once after delay
setTimeout(fn, delay, ...args)        // (Function, number, ...any[]) → Timeout
// Cancel timeout
clearTimeout(timeout)                 // (Timeout) → void
```

<details>
<summary>Examples</summary>

```js
const t = setTimeout(
  (a, b) => {},        // fn
  1000,                // delay (ms)
  1, 2                 // args
);
clearTimeout(t);        // timeout
```

</details>

---

## Interval

```js
// Execute function repeatedly
setInterval(fn, delay, ...args)       // (Function, number, ...any[]) → Interval
// Cancel interval
clearInterval(interval)               // (Interval) → void
```

<details>
<summary>Examples</summary>

```js
const i = setInterval(
  (msg) => {},         // fn
  500,                 // delay (ms)
  'tick'               // args
);
clearInterval(i);      // interval
```

</details>

---

## Immediate

```js
// Execute after I/O events
setImmediate(fn, ...args)             // (Function, ...any[]) → Immediate
// Cancel immediate
clearImmediate(immediate)             // (Immediate) → void
```

<details>
<summary>Examples</summary>

```js
const im = setImmediate(
  (x) => {},           // fn
  42                   // args
);
clearImmediate(im);    // immediate
```

</details>

---

## Timer Object Methods

```js
// Prevent event loop from exiting
timer.ref()                           // () → this
// Allow process to exit if only timer remains
timer.unref()                         // () → this
// Check if timer is referenced
timer.hasRef()                        // () → boolean
```

<details>
<summary>Examples</summary>

```js
t.ref();
t.unref();
t.hasRef();
```

</details>

---

## Promises API (`timers/promises`)

```js
// Promise-based timeout
setTimeoutP(delay, value?, opts?)     // (number, any?, object?) → Promise<any>
// Promise-based interval (async iterator)
setIntervalP(delay, value?, opts?)    // (number, any?, object?) → AsyncIterator
// Promise-based immediate
setImmediateP(value?, opts?)          // (any?, object?) → Promise<any>
```

<details>
<summary>Examples</summary>

```js
await setTimeoutP(1000, 'done', { signal });

for await (const v of setIntervalP(500, 'tick', { signal })) {
  break;
}

await setImmediateP('now', {});
```

</details>

---

## Notes

* `setTimeout(0)` ≠ `setImmediate`
* Use `unref()` for background timers
* Prefer `timers/promises` with async/await
