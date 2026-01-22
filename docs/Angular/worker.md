---
id: web-workers-cheatsheet
title: Web Workers Cheatsheet
sidebar_position: 27
---

# Web Workers Cheatsheet

> Angular Web Workers enable running CPU-intensive tasks off the main UI thread for better performance.

---

## Overview

* **Web Workers** run background threads separate from the main Angular app.
* Useful for heavy computation, data processing, or any blocking tasks.
* Angular CLI supports scaffolding and building web workers.

---

## Adding a Web Worker

<details>
<summary>Example</summary>

```bash
ng generate web-worker my-worker --project=my-app
```

</details>

Options:

* `--project` → specify the project in a workspace
* Generates `my-worker.worker.ts` and updates `angular.json` for build

---

## Worker Code

<details>
<summary>Example</summary>

```ts
/// <reference lib="webworker" />

addEventListener('message', ({ data }) => {
  const result = data.numbers.reduce((a, b) => a + b, 0);
  postMessage(result);
});
```

</details>

* Use `addEventListener('message', callback)` to receive messages
* Use `postMessage(data)` to send back results
* Can import modules and use TypeScript types

---

## Using the Web Worker in Component/Service

<details>
<summary>Example</summary>

```ts
const worker = new Worker(new URL('./my-worker.worker', import.meta.url));
worker.onmessage = ({ data }) => {
  console.log('Sum result from worker:', data);
};
worker.postMessage({ numbers: [1, 2, 3, 4, 5] });
```

</details>

* Use `new URL('./worker.worker', import.meta.url)` for Angular 21+ syntax
* `onmessage` handles results from the worker
* `postMessage` sends data to the worker

---

## Terminating a Worker

<details>
<summary>Example</summary>

```ts
worker.terminate();
```

</details>

* Frees resources after task completion
* Workers are not garbage collected automatically

---

## Passing Data

* Supports serializable objects only (structured clone algorithm)
* No direct DOM access inside workers
* Use `postMessage` for bidirectional communication

---

## Build Considerations

* CLI automatically bundles worker during `ng build`
* Ensure `webWorkerTsConfig` is defined in `angular.json` if custom TS config is needed
* Works with AOT and JIT

---
