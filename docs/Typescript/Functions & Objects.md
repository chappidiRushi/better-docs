---
title: Objects & Functions
id: objects-functions
sidebar_position: 4
---
# Objects & Functions

> TypeScript typing features for object shapes and function definitions

---

## Object Type Literals

Inline object type definitions that describe the shape of an object.

<details>
<summary>Examples</summary>

```ts
let user: { id: number; name: string } = {
  id: 1,
  name: 'Dev'
};

function print(user: { id: number; active: boolean }) {
  console.log(user.id, user.active);
}
```

</details>

---

## Optional & readonly Properties

Modifiers that mark object properties as optional or immutable.

<details>
<summary>Examples</summary>

```ts
type User = {
  readonly id: number;          // cannot be reassigned
  name: string;
  email?: string;               // optional
};

const user: User = { id: 1, name: 'Dev' };

// user.id = 2;                 // ❌ readonly
user.email = 'a@b.com';
```

</details>

---

## Excess Property Checks

Compile-time validation that prevents assigning extra properties to object literals.

<details>
<summary>Examples</summary>

```ts
type Config = { url: string; method: string };

const cfg1: Config = {
  url: '/api',
  method: 'GET'
};

const cfg2: Config = {
  url: '/api',
  method: 'GET',
  // timeout: 1000           // ❌ excess property
};

// bypass via variable
const temp = { url: '/api', method: 'GET', timeout: 1000 };
const cfg3: Config = temp;     // allowed
```

</details>

---

## Function Types

Type definitions that describe function parameters and return values.

<details>
<summary>Examples</summary>

```ts
// inline function type
let sum: (a: number, b: number) => number;

sum = (x, y) => x + y;

// type alias
type Comparator = (a: number, b: number) => boolean;

const compare: Comparator = (a, b) => a > b;
```

</details>

---

## Optional & Default Parameters

Parameters that may be omitted or have default values.

<details>
<summary>Examples</summary>

```ts
function log(message: string, level?: string) {
  console.log(level, message);
}

function createUser(name: string, role: string = 'user') {
  return { name, role };
}

log('hello');                 // optional omitted
createUser('Dev');            // default applied
```

</details>

---

## Rest Parameters

Parameters that collect multiple arguments into an array.

<details>
<summary>Examples</summary>

```ts
function sumAll(...values: number[]) {
  return values.reduce((a, b) => a + b, 0);
}

sumAll(1, 2, 3);
sumAll(5, 10, 15, 20);
```

</details>

---

## Function Overloads

Multiple function signatures describing different valid call shapes.

<details>
<summary>Examples</summary>

```ts
function parse(value: string): string;
function parse(value: number): number;
function parse(value: string | number) {
  return typeof value === 'string'
    ? value.toUpperCase()
    : value * 2;
}

parse('ts');
parse(10);
```

</details>

---

## this Parameter Typing

Explicit typing of `this` inside functions for contextual safety.

<details>
<summary>Examples</summary>

```ts
type Logger = {
  prefix: string;
  log(this: Logger, message: string): void;
};

const logger: Logger = {
  prefix: '[LOG]',
  log(this: Logger, message: string) {
    console.log(this.prefix, message);
  }
};

logger.log('hello');
```

</details>

---

## Notes

* Object type literals are structural, not nominal
* Excess property checks apply only to object literals
* Overloads affect the call signature, not the implementation
* `this` parameters are erased at runtime
