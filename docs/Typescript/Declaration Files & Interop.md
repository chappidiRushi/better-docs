---

id: declaration-interop
title: Declaration Files & Interop
sidebar_position: 13
-------------------

# Declaration Files & Interop

> Describing external APIs and integrating JavaScript with TypeScript

---

## .d.ts Files

Provide type information for code without TypeScript sources.

<details>
<summary>Examples</summary>

```ts
// math.d.ts
export function sum(a: number, b: number): number;
export const version: string;
```

```ts
// usage
import { sum } from './math';
```

</details>

---

## Ambient Declarations

Declare types for values that exist at runtime but are not imported.

<details>
<summary>Examples</summary>

```ts
// ambient variable
declare const APP_VERSION: string;

// ambient function
declare function track(event: string): void;

track('init');
```

</details>

---

## Global vs Module Scope

Controls whether declarations are available globally or only via imports.

<details>
<summary>Examples</summary>

```ts
// global scope
declare global {
  interface Window {
    appName: string;
  }
}

// module scope
export interface User {
  id: number;
}
```

</details>

---

## Third-Party Typings

Adds type information for external JavaScript libraries.

<details>
<summary>Examples</summary>

```ts
// library provides its own types
import express from 'express';

const app = express();
```

```ts
// custom typing fallback
declare module 'legacy-lib' {
  export function run(): void;
}
```

</details>

---

## DefinitelyTyped (@types)

Community-maintained type definitions for JavaScript libraries.

<details>
<summary>Examples</summary>

```bash
npm install --save-dev @types/lodash
```

```ts
import _ from 'lodash';

_.chunk([1, 2, 3, 4], 2);
```

</details>

---

## JavaScript Interop (JSDoc)

Uses JSDoc comments to type-check JavaScript files.

<details>
<summary>Examples</summary>

```js
// @ts-check

/**
 * @param {number} a
 * @param {number} b
 * @returns {number}
 */
function add(a, b) {
  return a + b;
}
```

```js
/** @type {{ id: number; name: string }} */
const user = { id: 1, name: 'Dev' };
```

</details>

---

## Notes

* `.d.ts` files contain only types, no runtime code
* Ambient declarations describe existing globals
* Global scope affects all files in the project
* `@types/*` packages are used when libraries lack types
* JSDoc enables gradual typing of JavaScript
