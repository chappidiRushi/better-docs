---
title:  Interfaces & Type Aliases
id:  Interfaces_Type_Aliases
sidebar_position: 6
---

# Interfaces & Type Aliases

> TypeScript constructs for defining reusable, composable type contracts

---

## Interface Declarations

Named object shape contracts that describe the structure of values.

<details>

<summary>Examples</summary>

```ts
interface User {
  id: number;
  name: string;
}

const user: User = {
  id: 1,
  name: 'Dev'
};

interface Config {
  url: string;
  method: string;
}

function request(config: Config) {}
```

</details>

---

## Type Aliases

Named aliases for any type, including primitives, unions, intersections, and object shapes.

<details>
<summary>Examples</summary>

```ts
type ID = string | number;                  // union alias

type User = {
  id: ID;
  name: string;
};

const user: User = {
  id: 1,
  name: 'Dev'
};

// aliasing primitives
type Flag = boolean;
let active: Flag = true;
```

</details>

---

## Extends & Intersections

Mechanisms for composing types by inheriting or combining members.

<details>
<summary>Examples</summary>

```ts
// interface extension
interface Base {
  id: number;
}

interface User extends Base {
  name: string;
}

const user: User = { id: 1, name: 'Dev' };

// type intersection
type A = { id: number };
type B = { name: string };

type C = A & B;

const value: C = { id: 1, name: 'Dev' };
```

</details>

---

## Declaration Merging

Automatic merging of multiple interface declarations with the same name.

<details>
<summary>Examples</summary>

```ts
interface Settings {
  theme: string;
}

interface Settings {
  readonly version: number;
}

const settings: Settings = {
  theme: 'dark',
  version: 1
};
```

</details>

---

## Index Signatures

Definitions that allow objects to have dynamic property keys of a specific type.

<details>
<summary>Examples</summary>

```ts
interface Dictionary {
  [key: string]: number;                   // string index signature
}

const scores: Dictionary = {
  math: 90,
  science: 95
};

interface Mixed {
  [key: number]: string;                   // number index signature
}

const list: Mixed = {
  0: 'a',
  1: 'b'
};
```

</details>

---

## Call & Construct Signatures

Type definitions that describe callable or constructable values.

<details>
<summary>Examples</summary>

```ts
// call signature
interface Formatter {
  (value: string, upper?: boolean): string; // string, boolean | undefined
}

const format: Formatter = (v, upper) =>
  upper ? v.toUpperCase() : v.toLowerCase();

// construct signature
interface Service {
  new (url: string): { url: string };
}

class ApiService {
  constructor(public url: string) {}
}

const Svc: Service = ApiService;
const svc = new Svc('/api');
```

</details>

---

## Notes

* Interfaces are open and support declaration merging
* Type aliases are closed and support advanced compositions
* Interfaces are preferred for public object contracts
* Index signatures define constraints for dynamic keys
