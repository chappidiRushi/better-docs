---
title: Advanced & Meta Types
id: Advanced-Meta_Types
sidebar_position: 9
---
# Advanced & Meta Types

> TypeScript type-level constructs for transforming, deriving, and validating types

---

## keyof

Produces a union of property names from a given type.

<details>
<summary>Examples</summary>

```ts
type User = {
  id: number;
  name: string;
};

type UserKeys = keyof User;      // 'id' | 'name'

function get<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
```

</details>

---

## typeof

Extracts the type of a value or variable.

<details>
<summary>Examples</summary>

```ts
const config = {
  env: 'prod',
  debug: false
};

type Config = typeof config;     // { env: string; debug: boolean }

function init(options: typeof config) {}
```

</details>

---

## Indexed Access Types

Accesses the type of a specific property using indexing syntax.

<details>
<summary>Examples</summary>

```ts
type User = {
  id: number;
  profile: {
    name: string;
  };
};

type IdType = User['id'];         // number
type NameType = User['profile']['name']; // string

type Value<T, K extends keyof T> = T[K];
```

</details>

---

## Conditional Types

Selects a type based on a compile-time condition.

<details>
<summary>Examples</summary>

```ts
type IsString<T> = T extends string ? true : false;

type A = IsString<string>;        // true
type B = IsString<number>;        // false

// conditional distribution over unions
type ToArray<T> = T extends any ? T[] : never;
type U = ToArray<string | number>; // string[] | number[]
```

</details>

---

## infer Keyword

Introduces a type variable to capture part of a type in a conditional type.

<details>
<summary>Examples</summary>

```ts
type ReturnTypeOf<T> = T extends (...args: any[]) => infer R
  ? R
  : never;

function fn() {
  return 10;
}

type Result = ReturnTypeOf<typeof fn>; // number

// infer with generics
type Unwrap<T> = T extends Promise<infer U> ? U : T;

type A = Unwrap<Promise<string>>; // string
type B = Unwrap<number>;          // number
```

</details>

---

## Mapped Types

Creates new types by transforming properties of existing types.

<details>
<summary>Examples</summary>

```ts
type User = {
  id: number;
  name: string;
};

// built-in mapped types
type ReadonlyUser = Readonly<User>;
type PartialUser = Partial<User>;

// custom mapped type
type Nullable<T> = {
  [K in keyof T]: T[K] | null;
};
```

</details>

---

## Template Literal Types

Constructs string literal types using template string syntax.

<details>
<summary>Examples</summary>

```ts
type Event = 'click' | 'focus';

type HandlerName = `on${Capitalize<Event>}`;
// 'onClick' | 'onFocus'

function handle(event: HandlerName) {}
```

</details>

---

## Recursive Types

Types that reference themselves to model nested or tree-like structures.

<details>
<summary>Examples</summary>

```ts
type TreeNode<T> = {
  value: T;
  children?: TreeNode<T>[];
};

const tree: TreeNode<number> = {
  value: 1,
  children: [{ value: 2 }]
};
```

</details>

---

## satisfies Operator

Validates that a value conforms to a type without changing its inferred type.

<details>
<summary>Examples</summary>

```ts
type Config = {
  mode: 'dev' | 'prod';
  debug: boolean;
};

const config = {
  mode: 'dev',
  debug: true
} satisfies Config;

// retains literal types
config.mode;                      // 'dev'
```

</details>

---

## Notes

* Meta types operate purely at compile time
* Conditional types enable powerful type logic
* `infer` extracts sub-types from complex types
* `satisfies` validates shape without widening
