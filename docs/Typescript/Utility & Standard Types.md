---
title: Utility & Standard Types
id: utility-standard_types
sidebar_position: 10
---

# Utility & Standard Types

> Built-in TypeScript utility types for transforming and deriving types

---

## Partial

Makes all properties of a type optional.

<details>
<summary>Examples</summary>

```ts
type User = { id: number; name: string };

type PartialUser = Partial<User>;

const user: PartialUser = {
  name: 'Dev'
};
```

</details>

---

## Required

Makes all properties of a type required.

<details>
<summary>Examples</summary>

```ts
type User = { id?: number; name?: string };

type RequiredUser = Required<User>;

const user: RequiredUser = {
  id: 1,
  name: 'Dev'
};
```

</details>

---

## Readonly

Marks all properties of a type as immutable.

<details>
<summary>Examples</summary>

```ts
type Config = { env: string };

type ReadonlyConfig = Readonly<Config>;

const cfg: ReadonlyConfig = { env: 'prod' };
// cfg.env = 'dev';               // ❌ cannot reassign
```

</details>

---

## Pick

Creates a type by selecting a subset of properties.

<details>
<summary>Examples</summary>

```ts
type User = { id: number; name: string; age: number };

type UserPreview = Pick<User, 'id' | 'name'>;

const user: UserPreview = { id: 1, name: 'Dev' };
```

</details>

---

## Omit

Creates a type by excluding specific properties.

<details>
<summary>Examples</summary>

```ts
type User = { id: number; name: string; password: string };

type SafeUser = Omit<User, 'password'>;

const user: SafeUser = { id: 1, name: 'Dev' };
```

</details>

---

## Record

Constructs an object type with specified keys and value type.

<details>
<summary>Examples</summary>

```ts
type Role = 'admin' | 'user';

type Permissions = Record<Role, boolean>;

const perms: Permissions = {
  admin: true,
  user: false
};
```

</details>

---

## Exclude / Extract

Removes or selects union members.

<details>
<summary>Examples</summary>

```ts
type Status = 'success' | 'error' | 'loading';

type WithoutLoading = Exclude<Status, 'loading'>; // 'success' | 'error'

type OnlyError = Extract<Status, 'error' | 'warn'>; // 'error'
```

</details>

---

## NonNullable

Removes `null` and `undefined` from a type.

<details>
<summary>Examples</summary>

```ts
type Value = string | null | undefined;

type Clean = NonNullable<Value>; // string
```

</details>

---

## ReturnType / Parameters

Extracts return types or parameter tuples from functions.

<details>
<summary>Examples</summary>

```ts
function add(a: number, b: number) {
  return a + b;
}

type AddReturn = ReturnType<typeof add>;     // number
type AddParams = Parameters<typeof add>;     // [number, number]
```

</details>

---

## NoInfer

Blocks inferences to the contained type. Other than blocking inferences, NoInfer Type is identical to Type.

<details>
<summary>Example</summary>
```ts
function createStreetLight<C extends string>(
  colors: C[],
  defaultColor?: NoInfer<C>,
) {
  // ...
}
createStreetLight(["red", "yellow", "green"], "red");  // OK
createStreetLight(["red", "yellow", "green"], "blue");  // Error
```
</details>

## InstanceType

Extracts the instance type from a constructor function.

<details>
<summary>Examples</summary>

```ts
class Service {
  url = '/api';
}

type ServiceInstance = InstanceType<typeof Service>;

const svc: ServiceInstance = new Service();
```

</details>

---

## Additional Utility Types

Other commonly used built-in utilities.

<details>
<summary>Examples</summary>

```ts
// Awaited: unwraps Promise types
type A = Awaited<Promise<string>>;           // string

// ConstructorParameters
type CtorArgs = ConstructorParameters<typeof Service>;

// ThisType (contextual `this` typing)
type ObjectWithThis = {
  value: number;
  increment(this: ObjectWithThis): void;
} & ThisType<ObjectWithThis>;

// String manipulation utilities
type Upper = Uppercase<'hello'>;             // 'HELLO'
type Lower = Lowercase<'HELLO'>;             // 'hello'
type Cap = Capitalize<'dev'>;                // 'Dev'
type Uncap = Uncapitalize<'Dev'>;             // 'dev'
```

</details>

---

## Notes

* Utility types are implemented using mapped and conditional types
* They improve reuse and reduce boilerplate
* Prefer utilities over manual type duplication
* Most utilities compose cleanly with generics

