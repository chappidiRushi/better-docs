---
# title: Intro
sidebar_position: 1
---
# Language & Compiler Basics

> TypeScript language fundamentals and compilation pipeline

---

## TypeScript Overview

TypeScript is a statically typed superset of JavaScript that adds type annotations, interfaces, enums, generics, and compile‑time checks while emitting plain JavaScript.

<details>
<summary>Examples</summary>

```ts
// primitive types
let id: number = 1;
let name: string = 'TypeScript';
let active: boolean = true;

// complex types
let tags: string[] = ['ui', 'web'];        // Array<string>
let tuple: [number, string] = [1, 'one'];  // fixed length tuple

// object typing
interface User {
  id: number;
  name: string;
  role?: string; // optional
}

const user: User = { id: 1, name: 'Dev' };

// function typing
function sum(a: number, b: number): number {
  return a + b;
}
```

</details>

---

## TypeScript vs JavaScript

TypeScript adds static typing and compile‑time validation on top of JavaScript while preserving runtime behavior and browser compatibility.

<details>
<summary>Examples</summary>

```ts
// JavaScript (runtime error possible)
function multiply(a, b) {
  return a * b;
}
multiply(2, '3'); // allowed

// TypeScript (compile-time error)
function multiply(a: number, b: number): number {
  return a * b;
}
multiply(2, '3'); // ❌ type error

// transpiled output (JS)
function multiply(a, b) {
  return a * b;
}
```

</details>

---

## TypeScript Compiler (tsc)

The TypeScript compiler converts `.ts` files into JavaScript, performs type checking, and applies configuration rules defined in `tsconfig.json`.

<details>
<summary>Examples</summary>

```bash
# compile a single file
tsc app.ts

# compile using project config
tsc

# watch mode
tsc --watch
```

```ts
// input: app.ts
let count: number = 0;

// output: app.js
var count = 0;
```

</details>

---

## tsconfig.json

`tsconfig.json` defines compiler options, file inclusion rules, and language features for a TypeScript project.

<details>
<summary>Examples</summary>

```json
{
  "compilerOptions": {
    // Specifies the JavaScript language version for emitted output
    "target": "ES2020",                // ES3 | ES5 | ES2015 | ES2016 | ES2017 | ES2018 | ES2019 | ES2020 | ES2021 | ESNext

    // Defines the module system used in the generated JavaScript
    "module": "ES2020",                // None | CommonJS | AMD | UMD | System | ES2015 | ES2020 | ESNext

    // Specifies a list of built-in library declaration files to include
    "lib": ["ES2020", "DOM"],          // ES5 | ES2015 | ES2020 | DOM | DOM.Iterable | WebWorker

    // Enables all strict type-checking options
    "strict": true,                     // boolean

    // Raises error on expressions and declarations with an implied `any` type
    "noImplicitAny": true,              // boolean

    // Enforces strict null and undefined checks
    "strictNullChecks": true,           // boolean

    // Ensures function parameters are checked contravariantly
    "strictFunctionTypes": true,        // boolean

    // Ensures class properties are initialized in the constructor
    "strictPropertyInitialization": true,// boolean

    // Disallows `this` expressions with an implied `any` type
    "noImplicitThis": true,             // boolean

    // Emits "use strict" for each source file
    "alwaysStrict": true,               // boolean

    // Determines how module specifiers are resolved
    "moduleResolution": "node",        // node | classic

    // Base directory to resolve non-relative module names
    "baseUrl": "./",                   // string path

    // Maps module import paths to physical paths
    "paths": {                          // object<string, string[]>
      "@app/*": ["src/app/*"]          // alias pattern → path pattern(s)
    },

    // Specifies the root directory of input files
    "rootDir": "./src",                // string path

    // Redirects output structure to the specified directory
    "outDir": "./dist",                // string path

    // Generates corresponding .map files for debugging
    "sourceMap": true,                  // boolean

    // Includes the original TypeScript source in source maps
    "inlineSources": true,              // boolean

    // Removes comments from emitted JavaScript
    "removeComments": false,            // boolean

    // Generates .d.ts declaration files
    "declaration": false,               // boolean

    // Creates source maps for declaration files
    "declarationMap": false,            // boolean

    // Enables interoperability between CommonJS and ES modules
    "esModuleInterop": true,             // boolean

    // Allows default imports from modules without default export
    "allowSyntheticDefaultImports": true,// boolean

    // Allows importing JSON files as modules
    "resolveJsonModule": true,           // boolean

    // Prevents emitting files if any type-checking errors occur
    "noEmitOnError": true,               // boolean

    // Enables incremental compilation for faster rebuilds
    "incremental": true,                // boolean

    // Specifies the file used to store incremental build information
    "tsBuildInfoFile": ".tsbuildinfo"   // string path
  },

  // Specifies files or glob patterns to include in the program
  "include": ["src/**/*.ts"],          // string[] (glob patterns)

  // Specifies files or folders to exclude from compilation
  "exclude": ["node_modules", "dist"]// string[]
```

</details>

---

## Compilation Targets

Compilation targets control which JavaScript version TypeScript outputs, affecting syntax, polyfills, and browser compatibility.

<details>
<summary>Examples</summary>

```json
{
  "compilerOptions": {
    "target": "ES5" // ES5 | ES2015 | ES2016 | ES2020 | ESNext
  }
}
```

```ts
// input
class User {
  constructor(public name: string) {}
}

// output (ES5)
var User = /** @class */ (function () {
  function User(name) {
    this.name = name;
  }
  return User;
}());
```

</details>

---

## Source Maps

Source maps map compiled JavaScript back to original TypeScript files, enabling debugging in browsers using original source code.

<details>
<summary>Examples</summary>

```json
{
  "compilerOptions": {
    "sourceMap": true // boolean
  }
}
```

```text
app.ts      → original TypeScript
app.js      → compiled JavaScript
app.js.map  → mapping metadata
```

```js
//# sourceMappingURL=app.js.map
```

</details>

---

## Notes

* `tsc` can be run directly or via build tools
* Strict mode improves type safety in TypeScript projects
* Source maps are essential for debugging compiled JavaScript
