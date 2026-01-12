---
title: Union, Intersection & Narrowing
id: Union-Intersection-Narrowing
sidebar_position: 5
-------------------

# Union, Intersection & Narrowing

> TypeScript features for composing types and safely narrowing them at runtime

---

## Type Composition

Type Composition is creating complex types by combining simpler, reusable parts

---

### Union Types (`|`)

Types that allow a value to be **one of several possible types**.

<details>
<summary>Examples</summary>

```ts
let value: string | number;                 // union type

value = 'text';                             // string
value = 10;                                 // number

function print(id: string | number) {
  console.log(id);
}
```

</details>

---

### Intersection Types (`&`)

Types that **combine multiple types into one**, requiring all members.

<details>
<summary>Examples</summary>

```ts
type A = { id: number };
type B = { name: string };

type C = A & B;                             // intersection

const value: C = {
  id: 1,
  name: 'Dev'
};
```

</details>

---

### Literal Types

Types that restrict values to **exact string, number, or boolean literals**.

<details>
<summary>Examples</summary>

```ts
let direction: 'up' | 'down';               // string literals

direction = 'up';
// direction = 'left';                      // ❌ not allowed

function setRole(role: 'admin' | 'user') {}
setRole('admin');
```

</details>

---

## Type Narrowing

Techniques that reduce a Union type to a more specific one at runtime.

---

### Control-flow Narrowing

Automatic narrowing applied through conditional logic and early returns.

<details>
<summary>Examples</summary>

```ts
function process(value: string | null) {
  if (!value) {
    return;                                 // value is null here
  }

  value.toUpperCase();                     // value is string
}

function example(x: string | number) {
  if (typeof x === 'string') {
    return x.length;
  }

  return x.toFixed(2);
}
```

</details>

---

### Type Guards

Runtime checks that narrow a union to a specific type.

<details>
<summary>Examples</summary>

```ts
function log(value: string | number) {
  if (typeof value === 'string') {
    value.toUpperCase();                    // string
  } else {
    value.toFixed(2);                       // number
  }
}
```

</details>

---

### Built-in Guards: `typeof` / `instanceof`

JavaScript operators recognized by TypeScript for narrowing.

<details>
<summary>Examples</summary>

```ts
// typeof
function format(value: string | number) {
  if (typeof value === 'number') {
    return value.toFixed(2);
  }
  return value.toUpperCase();
}

// instanceof
class User {
  name = 'Dev';
}

function handle(value: User | Date) {
  if (value instanceof User) {
    value.name;
  } else {
    value.getTime();
  }
}
```

</details>

---

### User-defined Type Guards

Custom functions that narrow types using **type predicates**.

<details>
<summary>Examples</summary>

```ts
type Admin = { role: 'admin'; permissions: string[] };
type User = { role: 'user' };

type Person = Admin | User;

function isAdmin(value: Person): value is Admin {
  return value.role === 'admin';
}

function handle(person: Person) {
  if (isAdmin(person)) {
    person.permissions;                     // Admin
  }
}
```

</details>

---

### Discriminated Unions

Unions sharing a **common literal property** used for exhaustive narrowing.

<details>
<summary>Examples</summary>

```ts
type Square = { kind: 'square'; size: number };
type Circle = { kind: 'circle'; radius: number };

type Shape = Square | Circle;

function area(shape: Shape) {
  switch (shape.kind) {
    case 'square':
      return shape.size * shape.size;
    case 'circle':
      return Math.PI * shape.radius ** 2;
  }
}
```

</details>

---

## Notes

* Unions describe alternatives; intersections describe combinations
* Narrowing is driven by control flow and runtime checks
* Literal types enable strict domain modeling
* Discriminated unions scale well for complex state and domain logic
