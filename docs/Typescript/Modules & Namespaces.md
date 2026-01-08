---
id: modules-namespaces
title: Modules & Namespaces
sidebar_position: 11
---
# Modules & Namespaces

> Organizing, importing, and resolving code in TypeScript

---

## ES Module Syntax

Defines how files import and export values using standard JavaScript modules.

<details>
<summary>Examples</summary>

```ts
// export values
export const version = '1.0';
export function init() {}

// import values
import { version, init } from './module';

// import all
import * as mod from './module';
```

</details>

---

## Default vs Named Exports

Controls whether a module exposes a single default value or multiple named values.

<details>
<summary>Examples</summary>

```ts
// default export
export default function start() {}

// named export
export const stop = () => {};

// importing
import start from './service';              // default
import { stop } from './service';            // named
```

</details>

---

## Re-exports

Re-exposes exports from another module.

<details>
<summary>Examples</summary>

```ts
// re-export named
export { readFile } from 'fs';

// re-export all
export * from './utils';

// re-export default as named
export { default as Logger } from './logger';
```

</details>

---

## Dynamic Imports

Loads modules asynchronously at runtime.

<details>
<summary>Examples</summary>

```ts
// returns Promise<typeof module>
const mod = await import('./feature');

mod.run();

// with condition
if (env === 'dev') {
  const devTools = await import('./dev-tools');
}
```

</details>

---

## Namespaces

Groups related types and values under a single global scope.

<details>
<summary>Examples</summary>

```ts
namespace Utils {
  // exported function is accessible outside the namespace
  export function sum(a: number, b: number) {
    return a + b;
  }

  // exported type becomes part of the namespace API
  export type ID = string | number; // string | number

  // non-exported members are private to the namespace
  const internal = 'hidden';
}

// namespace members are accessed via dot notation
Utils.sum(1, 2);

// using namespace type
const id: Utils.ID = 1; // number | string
```

</details>

---

## Path Mapping (baseUrl, paths)

Defines custom module resolution aliases via compiler configuration.

<details>
<summary>Examples</summary>

```jsonc
{
  // Base directory for non-relative imports
  "compilerOptions": {
    "baseUrl": "./src",                // string

    // Path aliases
    "paths": {
      "@utils/*": ["utils/*"],         // string[]
      "@models": ["models/index.ts"]  // string[]
    }
  }
}
```

```ts
// usage
import { sum } from '@utils/math';
import { User } from '@models';
```

</details>

---

## Notes

* ES modules are the standard for modern TypeScript
* Default exports allow one primary value per module
* Re-exports simplify public APIs
* Dynamic imports enable lazy loading
* Namespaces are mainly for legacy or global scripts
* Path mapping affects compile-time resolution only
