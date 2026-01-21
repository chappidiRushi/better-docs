# `buffer` Module

> Binary data handling — allocation, encoding, decoding

---

## Import (ESM)

```js
import { Buffer } from 'node:buffer';
```

<details>
<summary>Examples</summary>

```js
import { Buffer } from 'node:buffer';
```

</details>

---

## Buffer Creation

```js
Buffer.alloc(size, fill?, encoding?)        // (number, string|Buffer|number?, string?) → Buffer
Buffer.allocUnsafe(size)                    // (number) → Buffer
Buffer.from(value, encodingOrOffset?, len?)// (string|Array|Buffer|ArrayBuffer, string|number?, number?) → Buffer
```

<details>
<summary>Examples</summary>

```js
Buffer.alloc(10, 'a', 'utf8');                 // size, fill, encoding
Buffer.allocUnsafe(10);                        // size
Buffer.from('hello', 'utf8');                  // string, encoding
Buffer.from([1, 2, 3]);                        // array
Buffer.from(new ArrayBuffer(8), 0, 4);         // arrayBuffer, byteOffset, length
```

</details>

---

## Buffer Properties

```js
buf.length                                   // number of bytes
Buffer.byteLength(str, encoding?)            // (string, string?) → number
Buffer.isBuffer(obj)                         // (any) → boolean
Buffer.isEncoding(enc)                       // (string) → boolean
```

<details>
<summary>Examples</summary>

```js
const buf = Buffer.from('hi', 'utf8');
buf.length;                                   // bytes
Buffer.byteLength('hi', 'utf8');              // string, encoding
Buffer.isBuffer(buf);                         // object
Buffer.isEncoding('utf8');                    // encoding
```

</details>

---

## Reading from Buffer

```js
buf.toString(encoding?, start?, end?)         // (string?, number?, number?) → string
buf.readUInt8(offset)                         // (number) → number
buf.readInt8(offset)                          // (number) → number
buf.readUInt16LE(offset)                      // (number) → number
buf.readUInt16BE(offset)                      // (number) → number
buf.readUInt32LE(offset)                      // (number) → number
buf.readUInt32BE(offset)                      // (number) → number
buf.readFloatLE(offset)                       // (number) → number
buf.readFloatBE(offset)                       // (number) → number
buf.readDoubleLE(offset)                      // (number) → number
buf.readDoubleBE(offset)                      // (number) → number
```

<details>
<summary>Examples</summary>

```js
const buf = Buffer.from([0x01, 0x02, 0x03, 0x04]);
buf.toString('hex', 0, 2);                     // encoding, start, end
buf.readUInt8(0);                              // offset
buf.readInt8(1);                               // offset
buf.readUInt16LE(0);                           // offset
buf.readUInt16BE(0);                           // offset
buf.readUInt32LE(0);                           // offset
buf.readUInt32BE(0);                           // offset
buf.readFloatLE(0);                            // offset
buf.readFloatBE(0);                            // offset
buf.readDoubleLE(0);                           // offset
buf.readDoubleBE(0);                           // offset
```

</details>

---

## Writing to Buffer

```js
buf.write(string, offset?, length?, encoding?)// (string, number?, number?, string?) → number
buf.writeUInt8(value, offset)                 // (number, number) → number
buf.writeInt8(value, offset)                  // (number, number) → number
buf.writeUInt16LE(value, offset)              // (number, number) → number
buf.writeUInt16BE(value, offset)              // (number, number) → number
buf.writeUInt32LE(value, offset)              // (number, number) → number
buf.writeUInt32BE(value, offset)              // (number, number) → number
buf.writeFloatLE(value, offset)               // (number, number) → number
buf.writeFloatBE(value, offset)               // (number, number) → number
buf.writeDoubleLE(value, offset)              // (number, number) → number
buf.writeDoubleBE(value, offset)              // (number, number) → number
```

<details>
<summary>Examples</summary>

```js
const buf = Buffer.alloc(16);
buf.write('hi', 0, 2, 'utf8');                 // string, offset, length, encoding
buf.writeUInt8(255, 0);                        // value, offset
buf.writeInt8(-1, 1);                          // value, offset
buf.writeUInt16LE(500, 2);                     // value, offset
buf.writeUInt16BE(500, 4);                     // value, offset
buf.writeUInt32LE(1000, 6);                    // value, offset
buf.writeUInt32BE(1000, 10);                   // value, offset
buf.writeFloatLE(1.5, 0);                      // value, offset
buf.writeFloatBE(1.5, 4);                      // value, offset
buf.writeDoubleLE(1.234, 0);                   // value, offset
buf.writeDoubleBE(1.234, 8);                   // value, offset
```

</details>

---

## Buffer Utilities

```js
buf.slice(start?, end?)                        // (number?, number?) → Buffer
buf.subarray(start?, end?)                    // (number?, number?) → Buffer
buf.copy(target, targetStart?, sourceStart?, sourceEnd?) // (Buffer, number?, number?, number?) → number
buf.equals(otherBuffer)                       // (Buffer) → boolean
buf.compare(otherBuffer, targetStart?, targetEnd?, sourceStart?, sourceEnd?) // (...) → number
buf.fill(value, offset?, end?, encoding?)     // (string|number|Buffer, number?, number?, string?) → Buffer
```

<details>
<summary>Examples</summary>

```js
const a = Buffer.from('hello');
const b = Buffer.alloc(10);
a.slice(0, 2);                                 // start, end
a.subarray(1, 4);                              // start, end
a.copy(b, 0, 0, 5);                            // target, tStart, sStart, sEnd
a.equals(Buffer.from('hello'));                // buffer
Buffer.compare(a, Buffer.from('world'));       // buffers
b.fill('a', 0, 5, 'utf8');                     // value, start, end, encoding
```

</details>

---

## JSON / Encoding

```js
buf.toJSON()                                  // () → { type: 'Buffer', data: number[] }
```

<details>
<summary>Examples</summary>

```js
const buf = Buffer.from('hi');
buf.toJSON();                                  // no args
```

</details>

---

## Notes

* Buffers are **fixed-size** and mutable
* All offsets are **byte-based**
* Prefer `Buffer.alloc` over `allocUnsafe` unless performance-critical
