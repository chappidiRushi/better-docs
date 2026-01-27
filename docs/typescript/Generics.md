---
sidebar_position : 8
---
# Generics

> TypeScript mechanisms for creating reusable, type-safe components

---

## Generic Functions

Functions parameterized with types to operate on multiple types while preserving type safety.

<details>
<summary>Examples</summary>

```ts
function identity<T>(value: T): T {
  return value;
}

identity<string>('text');        // T = string
identity<number>(10);            // T = number

// type inference
identity(true);                  // T inferred as boolean
```

</details>

---

## Generic Interfaces

Interfaces that define contracts using type parameters.

<details>
<summary>Examples</summary>

```ts
interface ApiResponse<T> {
  data: T;
  error: string | null;
}

const response: ApiResponse<number> = {
  data: 200,
  error: null
};

const userResponse: ApiResponse<{ id: number }> = {
  data: { id: 1 },
  error: null
};
```

</details>

---

## Generic Classes

Classes that operate on a type parameter for properties and methods.

<details>
<summary>Examples</summary>

```ts
class Box<T> {
  constructor(public value: T) {}

  get(): T {
    return this.value;
  }
}

const box1 = new Box<string>('text');
const box2 = new Box<number>(10);
```

</details>

---

## Generic Constraints

Restrictions placed on generic types using `extends`.

<details>
<summary>Examples</summary>

```ts
// constraint with object shape
function getLength<T extends { length: number }>(value: T): number {
  return value.length;
}

getLength('text');               // string has length
getLength([1, 2, 3]);            // array has length
// getLength(10);                // ❌ number has no length

// constraint with union
function toId<T extends string | number>(value: T): string {
  return String(value);
}

// constraint using another generic
function merge<T extends U, U>(a: T, b: U) {
  return { ...a, ...b };
}
```

</details>

---

## Default Generic Parameters

Fallback types used when no explicit generic argument is provided.

<details>
<summary>Examples</summary>

```ts
interface Result<T = string> {
  value: T;
}

const r1: Result = { value: 'ok' };     // T defaults to string
const r2: Result<number> = { value: 1 };// T explicitly set
```

</details>

---

## Variance

Rules that determine how generic types relate to each other based on assignability.

<details>
<summary>Examples</summary>

```ts
// covariant (output positions)
type Producer<T> = () => T;

let pString: Producer<string>;
let pUnion: Producer<string | number>;

pUnion = pString;                 // allowed

// contravariant (input positions)
type Consumer<T> = (value: T) => void;

let cString: Consumer<string>;
let cUnion: Consumer<string | number>;

cString = cUnion;                 // allowed

// invariant (both input and output)
type Box<T> = { value: T };

let bString: Box<string>;
let bUnion: Box<string | number>;

// bUnion = bString;              // ❌ invariant
```

</details>

---

## keyof with Generics

Uses property keys of a type as a constrained generic parameter.

<details>
<summary>Examples</summary>

```ts
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user = { id: 1, name: 'Dev' };

getProperty(user, 'id');          // number
getProperty(user, 'name');        // string
// getProperty(user, 'age');      // ❌ invalid key
```

</details>

---

## Advanced Patterns & Tricks

Reusable patterns that leverage generics for stronger type safety.

<details>
<summary>Examples</summary>

```ts
// generic factory
function createFactory<T>(value: T) {
  return () => value;
}

const makeNumber = createFactory(10); // T inferred as number

// conditional generic inference
function unwrap<T>(value: T[]): T {
  return value[0];
}

// mapped generics
function freeze<T>(obj: T): Readonly<T> {
  return Object.freeze(obj);
}

// higher-order generics
function withId<T>(value: T): T & { id: string } {
  return { ...value, id: 'generated' };
}
```

</details>

---

## Notes

* Generics preserve type relationships across inputs and outputs
* Constraints narrow allowed generic arguments
* Variance depends on how a type parameter is used
* Advanced patterns improve reuse without losing type safety
