---
# title: Typescript
sidebar_position: 2
---
# Primitive & Built-in Types

> Core data types provided by the TypeScript type system

---

## string, number, boolean

Primitive scalar types representing textual, numeric, and logical values.

<details>
<summary>Examples</summary>

```ts
// string values
let title: string = 'TypeScript';          // string
let template: string = `v${1}`;            // string (template literal)

// number values
let count: number = 10;                    // number (int | float)
let price: number = 99.99;                 // number
let hex: number = 0xff;                    // number (hex | binary | octal)

// boolean values
let enabled: boolean = true;               // boolean
let visible: boolean = false;              // boolean
```

</details>

---

## null & undefined

Special primitive types representing absence of a value or an uninitialized value.

<details>
<summary>Examples</summary>

```ts
let n: null = null;                         // null only
let u: undefined = undefined;               // undefined only

let maybe: string | null;                   // union with null
let optional: string | undefined;           // union with undefined

function log(value?: string) {              // value: string | undefined
  console.log(value);
}
```

</details>

---

## unknown

Top type representing an unknown value that must be type-checked before use.

<details>
<summary>Examples</summary>

```ts
let value: unknown;                         // unknown

value = 10;                                // number
value = 'text';                            // string
value = { a: 1 };                          // object

// type narrowing required
if (typeof value === 'string') {
  value.toUpperCase();                     // safe after check
}

// comparison with any
let anyValue: any = value;                 // allowed
// let str: string = value;                // ❌ not allowed without narrowing
```

</details>

---

## symbol & bigint

Primitive types for unique identifiers and arbitrary-precision integers.

<details>
<summary>Examples</summary>

```ts
// symbol
const id: symbol = Symbol('id');            // unique symbol
const key = Symbol.for('shared');            // global symbol

// bigint
let big: bigint = 9007199254740991n;        // bigint literal
let calculated: bigint = BigInt(123);       // BigInt constructor

// bigint operations
let sum: bigint = big + calculated;         // bigint arithmetic
```

</details>

---

## object

Represents non-primitive values such as plain objects, functions, and arrays.

<details>
<summary>Examples</summary>

```ts
let obj: object = { a: 1 };                  // any non-primitive

let record: { id: number; name: string } = {
  id: 1,
  name: 'Dev'
};

let fn: object = function () {};             // function as object
let arr: object = [1, 2, 3];                 // array as object
```

</details>

---

## Array & ReadonlyArray

Typed collections of values, optionally marked as immutable.

<details>
<summary>Examples</summary>

```ts
// mutable arrays
let list: number[] = [1, 2, 3];              // number[]
let names: Array<string> = ['a', 'b'];       // Array<string>

list.push(4);                                // allowed

// readonly arrays
let readonly: ReadonlyArray<number> = [1, 2]; // ReadonlyArray<number>

// readonly.push(3);                          // ❌ mutation not allowed
```

</details>

---

## Tuple Types

Fixed-length arrays with known element types at each position.

<details>
<summary>Examples</summary>

```ts
let tuple: [number, string] = [1, 'one'];    // fixed order and length

let optional: [number, string?] = [1];      // optional element

let rest: [string, ...number[]] = ['a', 1]; // rest elements

// named tuple elements
let user: [id: number, name: string] = [1, 'Dev'];
```

</details>

---

## Enum Types

Named constants representing a fixed set of values.

<details>
<summary>Examples</summary>

```ts
// numeric enum
enum Status {
  Pending,        // 0
  Active,         // 1
  Disabled        // 2
}

let s: Status = Status.Active;

// string enum
enum Direction {
  Up = 'UP',
  Down = 'DOWN'
}

let d: Direction = Direction.Up;

// const enum
const enum Role {
  Admin,
  User
}

let r: Role = Role.Admin;
```

</details>

---

## Notes

* Primitive types are immutable
* `null` and `undefined` are distinct types under strict mode
* `ReadonlyArray` enforces immutability at compile time
* `const enum` values are inlined at compile time
