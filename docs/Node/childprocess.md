# `child_process` Module

> Spawn and manage subprocesses, exec commands, IPC

---

## Import (ESM)

```js
import {
  spawn,
  spawnSync,
  exec,
  execSync,
  execFile,
  execFileSync,
  fork
} from 'node:child_process';
```

<details>
<summary>Examples</summary>

```js
import { spawn, exec, fork } from 'node:child_process';
```

</details>

---

## spawn / spawnSync

```js
// Spawn a process with streaming stdio
spawn(command, args?, options?)          // (string, string[]?, object?) → ChildProcess
// Spawn synchronously (blocking)
spawnSync(command, args?, options?)      // (string, string[]?, object?) → { pid, output, stdout, stderr, status }
```

<details>
<summary>Examples</summary>

```js
spawn('ls', ['-la'], {
  cwd: '/tmp',            // working directory
  env: process.env,       // env vars
  stdio: 'pipe',          // stdio config
  shell: false,           // run in shell
  detached: false         // detach process
});

spawnSync('node', ['-v'], {
  encoding: 'utf8',
  timeout: 1000,
  maxBuffer: 1024 * 1024
});
```

</details>

---

## exec / execSync

```js
// Execute command in a shell (buffered output)
exec(command, options?, callback?)        // (string, object?, Function?) → ChildProcess
// Execute command synchronously
execSync(command, options?)               // (string, object?) → Buffer|string
```

<details>
<summary>Examples</summary>

```js
exec('ls -la', {
  cwd: '/tmp',
  env: process.env,
  encoding: 'utf8',
  maxBuffer: 1024 * 1024,
  timeout: 1000
}, (err, stdout, stderr) => {});

execSync('node -v', {
  encoding: 'utf8',
  stdio: 'pipe'
});
```

</details>

---

## execFile / execFileSync

```js
// Execute a file directly (no shell)
execFile(file, args?, options?, callback?) // (string, string[]?, object?, Function?) → ChildProcess
// Execute file synchronously
execFileSync(file, args?, options?)        // (string, string[]?, object?) → Buffer|string
```

<details>
<summary>Examples</summary>

```js
execFile('node', ['-v'], {
  cwd: process.cwd(),
  env: process.env,
  encoding: 'utf8'
}, (err, stdout, stderr) => {});

execFileSync('node', ['-v'], {
  encoding: 'utf8'
});
```

</details>

---

## fork (Node-only)

```js
// Spawn a Node.js child with IPC
fork(modulePath, args?, options?)          // (string, string[]?, object?) → ChildProcess
```

<details>
<summary>Examples</summary>

```js
fork('./worker.js', ['a', 'b'], {
  cwd: process.cwd(),
  env: process.env,
  execArgv: ['--inspect'],
  silent: false
});
```

</details>

---

## ChildProcess Properties

```js
child.pid                       // number — process id
child.stdin                     // Writable | null
child.stdout                    // Readable | null
child.stderr                    // Readable | null
child.connected                 // boolean — IPC connected
```

<details>
<summary>Examples</summary>

```js
const child = spawn('node', ['-v']);
child.pid;
child.stdout.on('data', d => {});
child.stderr.on('data', d => {});
child.connected;
```

</details>

---

## ChildProcess Methods

```js
child.kill(signal?)             // (string?) → boolean
child.send(message, handle?, options?, callback?) // (...) → boolean
child.disconnect()              // () → void
```

<details>
<summary>Examples</summary>

```js
child.kill('SIGTERM');
child.send({ cmd: 'ping' }, null, {}, () => {});
child.disconnect();
```

</details>

---

## ChildProcess Events

```js
child.on('spawn', listener)     // () → void
child.on('exit', listener)      // (code, signal) → void
child.on('close', listener)     // (code, signal) → void
child.on('error', listener)     // (Error) → void
child.on('message', listener)  // (message, handle) → void
```

<details>
<summary>Examples</summary>

```js
child.on('spawn', () => {});
child.on('exit', (code, sig) => {});
child.on('close', (code, sig) => {});
child.on('error', err => {});
child.on('message', (msg, handle) => {});
```

</details>

---

## Notes

* `spawn` = streaming (preferred)
* `exec` = buffered + shell
* `execFile` = safer (no shell)
* `fork` enables Node IPC
