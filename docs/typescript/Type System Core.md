---
id: type-system-core
title: Type System Core
sidebar_position: 3
---


# Type System Core

> Fundamental concepts that define how TypeScript types are declared, inferred, and enforced

---

## Type Annotations

Explicit syntax used to declare the type of variables, parameters, return values, and object shapes.

<details>
<summary>Examples</summary>

```ts
// variable annotations
let count: number = 1;                      // number
let title: string = 'TS';                   // string
let active: boolean = true;                 // boolean

// function annotations
function add(a: number, b: number): number {
  return a + b;
}

// object shape annotations
let user: { id: number; name: string } = {
  id: 1,
  name: 'Dev'
};

// array annotations
let list: number[] = [1, 2, 3];             // number[] | Array<number>
```

</details>

---

## Type Inference

Automatic deduction of types by the compiler based on assigned values and usage.

<details>
<summary>Examples</summary>

```ts
let count = 1;                              // inferred as number
let title = 'TS';                           // inferred as string
let active = true;                          // inferred as boolean

// function return type inference
function multiply(a: number, b: number) {
  return a * b;                             // inferred return type: number
}

// contextual typing
const handler = (event: MouseEvent) => {
  event.preventDefault();                   // event inferred from context
};
```

</details>

---

## Structural Typing

Type compatibility based on object shape rather than explicit declarations or inheritance.

<details>
<summary>Examples</summary>

```ts
type A = { id: number };
type B = { id: number; name: string };

let a: A;
let b: B = { id: 1, name: 'Dev' };

a = b;                                      // allowed (structure compatible)
// b = a;                                   // ❌ missing property 'name'

// function parameter compatibility
function logId(value: { id: number }) {}
logId({ id: 1, extra: true });              // allowed
```

</details>

---

## any, unknown

Top types that disable strict type checking (`any`) or require explicit narrowing (`unknown`).

<details>
<summary>Examples</summary>

```ts
let a: any = 1;                             // any
let u: unknown = 1;                         // unknown

// any allows unsafe operations
a.toUpperCase();                            // allowed

// unknown requires narrowing
if (typeof u === 'string') {
  u.toUpperCase();                          // safe after check
}

// assignment behavior
let str1: string = a;                       // allowed
// let str2: string = u;                    // ❌ not allowed
```

</details>

---

## void, never

Special return types representing absence of a value or unreachable code paths.

<details>
<summary>Examples</summary>

```ts
// void: no meaningful return value
function log(message: string): void {
  console.log(message);
}

let v: void = undefined;                    // void

// never: function never completes
function fail(error: string): never {
  throw new Error(error);
}

function infinite(): never {
  while (true) {}
}
```

</details>

---

## Type Assertions

Compile-time hints that tell the compiler to treat a value as a specific type.

<details>
<summary>Examples</summary>

```ts
let value: unknown = 'text';

// angle-bracket syntax
let len1: number = (<string>value).length; // assertion

// as-syntax (recommended)
let len2: number = (value as string).length; // assertion

// DOM assertions
const input = document.querySelector('input') as HTMLInputElement;
input.value = 'hello';
```

</details>

---

## Notes

* Type inference reduces the need for explicit annotations
* Structural typing enables flexible object compatibility
* Prefer `unknown` over `any` for safer type handling
* Type assertions do not perform runtime checks
