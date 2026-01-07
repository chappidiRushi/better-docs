# `events` Module

> Event‑driven architecture using EventEmitter

---

## Import (ESM)

```js
import { EventEmitter, once, on } from 'node:events';
```

<details>
<summary>Examples</summary>

```js
import { EventEmitter, once, on } from 'node:events';
```

</details>

---

## EventEmitter Class

```js
new EventEmitter(options?)                 // ({ captureRejections?: boolean }?) → EventEmitter
```

<details>
<summary>Examples</summary>

```js
const ee = new EventEmitter({ captureRejections: true }); // options
```

</details>

---

## Registering Listeners

```js
ee.on(event, listener)                    // (string|symbol, Function) → this
ee.addListener(event, listener)           // alias of on
ee.once(event, listener)                  // (string|symbol, Function) → this
ee.prependListener(event, listener)       // (string|symbol, Function) → this
ee.prependOnceListener(event, listener)   // (string|symbol, Function) → this
```

<details>
<summary>Examples</summary>

```js
ee.on('data', (a, b) => {});                 // event, listener
ee.addListener('data', () => {});             // event, listener
ee.once('end', () => {});                     // event, listener
ee.prependListener('log', msg => {});         // event, listener
ee.prependOnceListener('init', () => {});     // event, listener
```

</details>

---

## Emitting Events

```js
ee.emit(event, ...args)                   // (string|symbol, ...any[]) → boolean
```

<details>
<summary>Examples</summary>

```js
ee.emit('data', 1, 2, 3);                   // event, args
```

</details>

---

## Removing Listeners

```js
ee.off(event, listener)                   // (string|symbol, Function) → this
ee.removeListener(event, listener)         // alias of off
ee.removeAllListeners(event?)              // (string|symbol?) → this
```

<details>
<summary>Examples</summary>

```js
const fn = () => {};
ee.off('data', fn);                          // event, listener
ee.removeListener('data', fn);               // event, listener
ee.removeAllListeners('data');               // event
ee.removeAllListeners();                     // all events
```

</details>

---

## Listener Introspection

```js
ee.listeners(event)                       // (string|symbol) → Function[]
ee.rawListeners(event)                    // (string|symbol) → Function[]
ee.listenerCount(event)                   // (string|symbol) → number
ee.eventNames()                            // () → (string|symbol)[]
```

<details>
<summary>Examples</summary>

```js
ee.listeners('data');                      // event
ee.rawListeners('data');                   // event
ee.listenerCount('data');                  // event
ee.eventNames();                            // no args
```

</details>

---

## Listener Limits

```js
ee.setMaxListeners(n)                     // (number) → this
ee.getMaxListeners()                      // () → number
EventEmitter.defaultMaxListeners           // number
EventEmitter.setMaxListeners(n, ...ees)    // (number, ...EventEmitter[]) → void
```

<details>
<summary>Examples</summary>

```js
ee.setMaxListeners(20);                    // number
ee.getMaxListeners();                      // no args
EventEmitter.defaultMaxListeners = 15;      // global default
EventEmitter.setMaxListeners(5, ee);        // limit for instances
```

</details>

---

## Error Handling

```js
ee.emit('error', err)                    // (Error) → boolean
```

<details>
<summary>Examples</summary>

```js
ee.on('error', err => {});                // error listener
ee.emit('error', new Error('fail'));       // error
```

</details>

---

## Async Utilities

```js
once(emitter, event, options?)             // (EventEmitter, string|symbol, object?) → Promise<any[]>
on(emitter, event, options?)               // (EventEmitter, string|symbol, object?) → AsyncIterator
```

<details>
<summary>Examples</summary>

```js
await once(ee, 'ready', { signal });        // emitter, event, options

for await (const [msg] of on(ee, 'data', { signal })) {
  // async iteration
}
```

</details>

---

## Static Helpers

```js
EventEmitter.listenerCount(emitter, event) // (EventEmitter, string|symbol) → number
```

<details>
<summary>Examples</summary>

```js
EventEmitter.listenerCount(ee, 'data');     // emitter, event
```

</details>

---

## Symbols

```js
EventEmitter.errorMonitor                  // Symbol — observe errors without consuming them
```

<details>
<summary>Examples</summary>

```js
ee.on(EventEmitter.errorMonitor, err => {}); // error observer
```

</details>

---

## Notes

* `'error'` without listeners will **throw**
* Listeners are called **sync** by default
* Use `once()` / `on()` for async workflows
