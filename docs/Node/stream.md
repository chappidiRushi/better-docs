# `stream` Module

> Handle streaming data using readable, writable, duplex, and transform streams

---

## Import (ESM)

```js
import {
  Readable,
  Writable,
  Duplex,
  Transform,
  PassThrough,
  pipeline,
  finished
} from 'node:stream';
```

<details>
<summary>Examples</summary>

```js
import { Readable, Writable, pipeline } from 'node:stream';
```

</details>

---

## Stream Types (Classes)

```js
// Read data from a source
Readable                     // class
// Write data to a destination
Writable                     // class
// Read + write
Duplex                       // class
// Modify data while passing through
Transform                    // class
// Pass data through unchanged
PassThrough                  // class
```

<details>
<summary>Examples</summary>

```js
new Readable();
new Writable();
new Duplex();
new Transform();
new PassThrough();
```

</details>

---

## Readable Stream

```js
// Push data into stream
readable.push(chunk, encoding?)    // (any, string?) → boolean
// Signal no more data
readable.push(null)                // (null) → boolean
// Read data manually
readable.read(size?)               // (number?) → any
// Pause flowing mode
readable.pause()                   // () → this
// Resume flowing mode
readable.resume()                  // () → this
// Pipe to writable
readable.pipe(dest, options?)      // (Writable, object?) → Writable
// Unpipe destination
readable.unpipe(dest?)             // (Writable?) → this
```

<details>
<summary>Examples</summary>

```js
readable.push('data', 'utf8');      // chunk, encoding
readable.push(null);                // end
readable.read(1024);                // size
readable.pause();
readable.resume();
readable.pipe(writable, { end: true }); // dest, options
readable.unpipe(writable);           // dest
```

</details>

---

## Writable Stream

```js
// Write data to stream
writable.write(chunk, encoding?, cb?) // (any, string?, Function?) → boolean
// End writing
writable.end(chunk?, encoding?, cb?)  // (any?, string?, Function?) → void
// Set encoding
writable.setDefaultEncoding(enc)      // (string) → this
```

<details>
<summary>Examples</summary>

```js
writable.write('data', 'utf8', () => {}); // chunk, encoding, cb
writable.end('done', 'utf8', () => {});   // chunk, encoding, cb
writable.setDefaultEncoding('utf8');
```

</details>

---

## Duplex Stream

```js
// Write side
stream.write(chunk, enc?, cb?)     // same as Writable
// Read side
stream.read(size?)                 // same as Readable
```

<details>
<summary>Examples</summary>

```js
duplex.write('in', 'utf8', () => {});
duplex.read(1024);
```

</details>

---

## Transform Stream

```js
// Transform input to output
transform._transform(chunk, enc, cb) // (any, string, Function) → void
// Flush remaining data
transform._flush(cb)                // (Function) → void
```

<details>
<summary>Examples</summary>

```js
new Transform({
  transform(chunk, encoding, callback) {
    callback(null, chunk.toString().toUpperCase());
  },
  flush(callback) {
    callback();
  }
});
```

</details>

---

## Pipeline & Utilities

```js
// Pipe streams safely with error handling
pipeline(...streams, cb?)          // (...Stream[], Function?) → Promise
// Wait for stream to finish
finished(stream, cb?)              // (Stream, Function?) → Promise
```

<details>
<summary>Examples</summary>

```js
pipeline(readable, transform, writable, err => {}); // streams, cb
await pipeline(readable, writable);                  // promise
finished(writable, err => {});                       // stream, cb
await finished(writable);
```

</details>

---

## Stream Events

```js
// Data available
stream.on('data', fn)              // (chunk) → void
// Stream ended
stream.on('end', fn)               // () → void
// Writable finished
stream.on('finish', fn)            // () → void
// Error occurred
stream.on('error', fn)             // (Error) → void
// Stream closed
stream.on('close', fn)             // () → void
```

<details>
<summary>Examples</summary>

```js
stream.on('data', chunk => {});
stream.on('end', () => {});
stream.on('finish', () => {});
stream.on('error', err => {});
stream.on('close', () => {});
```

</details>

---

## Notes

* Streams handle large data efficiently
* Use `pipeline()` instead of `.pipe()` for safety
* Transform streams modify data in transit
