# `os` Module

> Core OS utilities — **sync**, **fast**, **no classes**

---

## Import (ESM)

```js
import os from 'node:os'; // no args → import module
```

<details>
<summary>Examples</summary>

```js
console.log(os.platform());
```

</details>

---

## Properties

```js
os.EOL                       // () → string | OS end‑of‑line
os.constants                 // () → object | signals, errno, priority
```

<details>
<summary>Examples</summary>

```js
process.stdout.write(`a${os.EOL}b`);
console.log(os.constants.signals.SIGTERM);
```

</details>

---

## System Info

```js
os.arch()                    // () → string | CPU architecture
os.platform()                // () → string | OS platform
os.type()                    // () → string | kernel OS name
os.release()                 // () → string | OS release
os.version()                 // () → string | human‑readable OS version
os.machine()                 // () → string | hardware identifier
```

<details>
<summary>Examples</summary>

```js
console.log(os.arch(), os.platform());
console.log(os.type(), os.release());
```

</details>

---

## CPU & Parallelism

```js
os.cpus()                    // () → Array | logical CPU core info
os.availableParallelism()    // () → number | usable parallel threads
```

<details>
<summary>Examples</summary>

```js
console.log(os.cpus().length);
console.log(os.availableParallelism());
```

</details>

---

## Memory

```js
os.totalmem()                // () → number | total RAM (bytes)
os.freemem()                 // () → number | free RAM (bytes)
```

<details>
<summary>Examples</summary>

```js
console.log(os.freemem() / os.totalmem());
```

</details>

---

## Uptime & Load

```js
os.uptime()                  // () → number | system uptime (seconds)
os.loadavg()                 // () → [1m,5m,15m] | CPU load (Windows: zeros)
```

<details>
<summary>Examples</summary>

```js
console.log(os.uptime());
console.log(os.loadavg());
```

</details>

---

## User & Host

```js
os.hostname()                // () → string | system hostname
os.homedir()                 // () → string | user home directory
os.userInfo(options?)        // ({ encoding?: 'utf8'|'buffer' }) → object
```

<details>
<summary>Examples</summary>

```js
console.log(os.userInfo().username);
console.log(os.homedir());
```

</details>

---

## Networking

```js
os.networkInterfaces()       // () → object | interfaces by name
```

<details>
<summary>Examples</summary>

```js
console.log(os.networkInterfaces().lo);
```

</details>

---

## Temp

```js
os.tmpdir()                  // () → string | default temp directory
```

<details>
<summary>Examples</summary>

```js
console.log(os.tmpdir());
```

</details>

---

## Process Priority

```js
os.getPriority(pid?)         // (number?) → number | process priority
os.setPriority(pid?, prio)   // (number?, number) → void | set priority
```

<details>
<summary>Examples</summary>

```js
console.log(os.getPriority());
os.setPriority(0, 10);
```

</details>

---

## Endianness

```js
os.endianness()              // () → 'LE' | 'BE' | CPU byte order
```

<details>
<summary>Examples</summary>

```js
console.log(os.endianness());
```

</details>

---

## Notes

* All APIs are synchronous
* Zero allocations unless returned data
* Safe at startup & runtime
