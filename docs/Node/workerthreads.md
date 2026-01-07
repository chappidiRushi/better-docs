# `worker_threads` Module

> Run JavaScript in parallel threads with shared memory

---

## Import (ESM)

```js
import { Worker, isMainThread, parentPort, workerData, threadId, MessageChannel, MessagePort, receiveMessageOnPort} from 'node:worker_threads';
```

<details>
<summary>Examples</summary>

```js
import { Worker, isMainThread, parentPort } from 'node:worker_threads';
```

</details>

---

## Thread Info

```js
// True if running in main thread
isMainThread                 // boolean
// Unique thread identifier
threadId                     // number
// Data passed from main thread
workerData                   // any
// Message port to parent
parentPort                   // MessagePort | null
```

<details>
<summary>Examples</summary>

```js
isMainThread;                 // boolean
threadId;                     // id
workerData;                   // initial data
parentPort?.postMessage('hi');
```

</details>

---

## Worker Class

```js
// Create a new worker thread
new Worker(filename, options?) // (string|URL, object?) → Worker
```

<details>
<summary>Examples</summary>

```js
new Worker('./worker.js', {
  workerData: { a: 1 },        // initial data
  env: process.env,            // env vars
  execArgv: ['--inspect'],     // node flags
  argv: ['x', 'y'],            // argv for worker
  stdin: false,                // inherit stdin
  stdout: true,                // pipe stdout
  stderr: true,                // pipe stderr
  resourceLimits: {            // memory limits
    maxOldGenerationSizeMb: 64
  }
});
```

</details>

---

## Worker Properties

```js
worker.threadId              // number — worker id
worker.resourceLimits        // object — applied limits
worker.stdin                 // Writable | null
worker.stdout                // Readable | null
worker.stderr                // Readable | null
```

<details>
<summary>Examples</summary>

```js
const w = new Worker('./w.js');
w.threadId;
w.stdout?.on('data', d => {});
```

</details>

---

## Worker Methods

```js
// Send message to worker
worker.postMessage(value, transferList?) // (any, Array?) → void
// Terminate worker
worker.terminate()            // () → Promise<number>
// Add event listener
worker.on(event, listener)    // (string, Function) → this
```

<details>
<summary>Examples</summary>

```js
worker.postMessage({ cmd: 'run' }, []); // message, transferList
await worker.terminate();               // terminate
worker.on('message', msg => {});        // event
```

</details>

---

## Worker Events

```js
worker.on('message', listener) // (any) → void
worker.on('error', listener)   // (Error) → void
worker.on('exit', listener)    // (number) → void
worker.on('online', listener)  // () → void
```

<details>
<summary>Examples</summary>

```js
worker.on('message', msg => {});
worker.on('error', err => {});
worker.on('exit', code => {});
worker.on('online', () => {});
```

</details>

---

## MessageChannel / MessagePort

```js
// Create a message channel
new MessageChannel()           // () → { port1, port2 }
// Message port for communication
MessagePort                    // class
// Receive message synchronously
receiveMessageOnPort(port)     // (MessagePort) → { message } | undefined
```

<details>
<summary>Examples</summary>

```js
const { port1, port2 } = new MessageChannel();
port1.postMessage('hi');
receiveMessageOnPort(port2);   // sync receive
```

</details>

---

## Shared Memory

```js
// Shared memory buffer
new SharedArrayBuffer(length)  // (number) → SharedArrayBuffer
// Atomic operations
Atomics.add(view, index, value)// (TypedArray, number, number) → number
```

<details>
<summary>Examples</summary>

```js
const sab = new SharedArrayBuffer(1024); // bytes
const view = new Int32Array(sab);
Atomics.add(view, 0, 1);                 // atomic op
```

</details>

---

## Notes

* Threads share memory, not event loop
* Use `SharedArrayBuffer` + `Atomics` for sync
* Better than `child_process` for CPU-bound work
