# `process` Module

> Process information, lifecycle, environment, signals, and IPC

---

## Import (ESM)

```js
import process from 'node:process';
```

<details>
<summary>Examples</summary>

```js
import process from 'node:process';
```

</details>

---

## Process Info

```js
// Current process id
process.pid                      // number
// Parent process id
process.ppid                     // number
// Node.js version
process.version                  // string
// Node.js versions (deps)
process.versions                 // object
// Platform (darwin, win32, linux)
process.platform                 // string
// CPU architecture
process.arch                     // string
// Process title shown in ps/top
process.title                    // string
```

<details>
<summary>Examples</summary>

```js
process.pid;                      // pid
process.ppid;                     // parent pid
process.version;                  // node version
process.versions;                 // dependency versions
process.platform;                 // platform
process.arch;                     // architecture
process.title = 'my-app';         // set title
```

</details>

---

## Arguments & Exec

```js
// Command-line arguments
process.argv                     // string[]
// Node executable path
process.execPath                 // string
// Exec arguments passed to Node
process.execArgv                 // string[]
```

<details>
<summary>Examples</summary>

```js
process.argv;                     // ['node', 'app.js', ...]
process.execPath;                 // /usr/bin/node
process.execArgv;                 // ['--inspect']
```

</details>

---

## Environment

```js
// Environment variables
process.env                      // object
```

<details>
<summary>Examples</summary>

```js
process.env.NODE_ENV = 'prod';    // set env
process.env.PATH;                 // read env
```

</details>

---

## Working Directory

```js
// Get current working directory
process.cwd()                    // () → string
// Change working directory
process.chdir(directory)         // (string) → void
```

<details>
<summary>Examples</summary>

```js
process.cwd();                    // no args
process.chdir('/tmp');            // directory
```

</details>

---

## Memory & Resource Usage

```js
// Memory usage statistics
process.memoryUsage()            // () → object
// Resource usage (CPU, fs)
process.resourceUsage()          // () → object
// High-resolution time
process.hrtime(time?)            // ([number, number]?) → [number, number]
process.hrtime.bigint()          // () → bigint
```

<details>
<summary>Examples</summary>

```js
process.memoryUsage();            // no args
process.resourceUsage();          // no args
const start = process.hrtime();   // tuple
process.hrtime(start);            // diff
process.hrtime.bigint();          // bigint
```

</details>

---

## Exit & Lifecycle

```js
// Exit process with code
process.exit(code?)              // (number?) → never
// Exit code to be used on exit
process.exitCode                 // number
// Whether process is exiting
process._exiting                 // boolean (internal)
```

<details>
<summary>Examples</summary>

```js
process.exitCode = 1;             // set exit code
process.exit(0);                  // exit immediately
```

</details>

---

## Signals

```js
// Send signal to process
process.kill(pid, signal?)       // (number, string?) → boolean
```

<details>
<summary>Examples</summary>

```js
process.kill(process.pid, 'SIGTERM'); // pid, signal
```

</details>

---

## Standard IO Streams

```js
// Standard input (Readable)
process.stdin                    // stream.Readable
// Standard output (Writable)
process.stdout                   // stream.Writable
// Standard error (Writable)
process.stderr                   // stream.Writable
```

<details>
<summary>Examples</summary>

```js
process.stdout.write('hello');    // write
process.stderr.write('error');    // write
process.stdin.on('data', d => {});// read
```

</details>

---

## IPC / Messaging

```js
// Send message to parent process
process.send(message, sendHandle?, options?, callback?) // (...) → boolean
// Disconnect IPC channel
process.disconnect()             // () → void
// Whether IPC is connected
process.connected                // boolean
```

<details>
<summary>Examples</summary>

```js
process.send({ cmd: 'ping' }, null, {}, () => {}); // message, handle, options, cb
process.disconnect();             // no args
process.connected;                // boolean
```

</details>

---

## Events

```js
// Emitted before exit
process.on('beforeExit', listener) // (number) => void
// Emitted on exit
process.on('exit', listener)      // (number) => void
// Uncaught exception
process.on('uncaughtException', listener) // (Error) => void
// Unhandled promise rejection
process.on('unhandledRejection', listener) // (reason, promise) => void
// Signal events
process.on('SIGINT', listener)    // () => void
```

<details>
<summary>Examples</summary>

```js
process.on('beforeExit', code => {});        // code
process.on('exit', code => {});              // code
process.on('uncaughtException', err => {});  // error
process.on('unhandledRejection', (r, p) => {}); // reason, promise
process.on('SIGINT', () => {});               // signal
```

</details>

---

## Performance

```js
// Next tick queue
process.nextTick(callback, ...args) // (Function, ...any[]) → void
// Emit warning
process.emitWarning(warning, options?) // (string|Error, object?) → void
```

<details>
<summary>Examples</summary>

```js
process.nextTick(() => {}, 1, 2);   // callback, args
process.emitWarning('warn', { code: 'W1', type: 'Custom' }); // warning, options
```

</details>

---

## Notes

* `process.exit()` skips pending async work
* Prefer `exitCode` for graceful shutdowns
* Signals are platform-dependent
