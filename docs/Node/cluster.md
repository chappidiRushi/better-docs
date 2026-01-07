# `cluster` Module

> Multi‑process load balancing using worker processes

---

## Import (ESM)

```js
import cluster from 'node:cluster';
import os from 'node:os';
```

<details>
<summary>Examples</summary>

```js
import cluster from 'node:cluster';
import os from 'node:os';
```

</details>

---

## Cluster Properties

```js
cluster.isPrimary              // boolean — true in primary process
cluster.isWorker               // boolean — true in worker process
cluster.worker                 // Worker | undefined — current worker instance
cluster.workers                // { [id: string]: Worker } — active workers
cluster.settings               // object — fork settings
cluster.schedulingPolicy       // number — scheduling mode
cluster.SCHED_NONE             // number — OS scheduling
cluster.SCHED_RR               // number — round‑robin scheduling
```

<details>
<summary>Examples</summary>

```js
cluster.isPrimary;              // boolean
cluster.isWorker;               // boolean
cluster.worker;                 // Worker or undefined
cluster.workers;                // map of workers
cluster.settings;               // current settings
cluster.schedulingPolicy;       // policy value
```

</details>

---

## Cluster Methods

```js
cluster.fork(env?)              // (object?) → Worker
cluster.disconnect(cb?)         // (Function?) → void
cluster.setupPrimary(settings?) // (object?) → void
```

<details>
<summary>Examples</summary>

```js
cluster.fork({ NODE_ENV: 'prod' });             // env vars
cluster.disconnect(() => console.log('done'));  // optional callback
cluster.setupPrimary({ exec: 'worker.js', args: [], silent: false }); // settings
```

</details>

---

## Worker Class

```js
worker.id                       // number — worker id
worker.process                  // ChildProcess — underlying process
worker.isConnected()            // () → boolean
worker.isDead()                 // () → boolean
worker.send(message, handle?, options?, cb?) // (...) → boolean
worker.kill(signal?)            // (string?) → void
worker.disconnect()             // () → void
```

<details>
<summary>Examples</summary>

```js
const w = cluster.fork();
w.id;                                            // worker id
w.process;                                       // child process
w.isConnected();                                 // no args
w.isDead();                                      // no args
w.send({ cmd: 'ping' }, null, {}, () => {});     // message, handle, options, callback
w.kill('SIGTERM');                               // signal
w.disconnect();                                  // no args
```

</details>

---

## Cluster Events

```js
cluster.on('fork', listener)     // (Worker) — worker forked
cluster.on('online', listener)  // (Worker) — worker online
cluster.on('listening', listener) // (Worker, address) — server listening
cluster.on('disconnect', listener) // (Worker) — IPC disconnected
cluster.on('exit', listener)    // (Worker, code, signal) — worker exited
cluster.on('setup', listener)   // (settings) — primary setup
```

<details>
<summary>Examples</summary>

```js
cluster.on('fork', w => {});                       // worker
cluster.on('online', w => {});                    // worker
cluster.on('listening', (w, addr) => {});         // worker, address
cluster.on('disconnect', w => {});                // worker
cluster.on('exit', (w, code, sig) => {});         // worker, code, signal
cluster.on('setup', settings => {});              // settings
```

</details>

---

## Worker Events

```js
worker.on('message', listener)   // (message, handle) — IPC message
worker.on('online', listener)   // () — worker online
worker.on('listening', listener)// (address) — server listening
worker.on('disconnect', listener)// () — IPC disconnected
worker.on('exit', listener)     // (code, signal) — worker exited
worker.on('error', listener)    // (error) — process error
```

<details>
<summary>Examples</summary>

```js
const w = cluster.fork();
w.on('message', (msg, handle) => {});   // message, handle
w.on('online', () => {});               // no args
w.on('listening', addr => {});          // address
w.on('disconnect', () => {});           // no args
w.on('exit', (code, sig) => {});         // code, signal
w.on('error', err => {});                // error
```

</details>

---

## Typical Pattern

```js
// primary → forks workers
// worker  → runs server logic
```

<details>
<summary>Examples</summary>

```js
if (cluster.isPrimary) {
  for (let i = 0; i < os.cpus().length; i++) {
    cluster.fork();                     // no args
  }
} else {
  // worker code
}
```

</details>

---

## Notes

* Workers share **server ports**, not memory
* IPC uses `process.send`
* Prefer `worker_threads` for shared memory
