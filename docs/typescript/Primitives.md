---

title: TypeScript
sidebar_position: 2
-------------------

# Primitive & Built-in Types

> Core data types provided by the TypeScript type system

---

## Primitive Types

Primitive scalar types representing textual, numeric, logical, and special JavaScript values.

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

// bigint values (arbitrary-precision integers)
let big: bigint = 9007199254740991n;       // bigint

// symbol values (unique identifiers)
let id: symbol = Symbol('id');             // symbol

// null & undefined (absence / uninitialized)
let empty: null = null;                    // null
let notSet: undefined = undefined;          // undefined

// nullable union
let maybe: string | null | undefined;      // union with null / undefined
```

</details>

---

## Top & Bottom Types

Special types that define the upper and lower bounds of the type system.

<details>
<summary>Examples</summary>

```ts
// any (opt-out of type checking)
let anything: any = 1;
anything.foo.bar();                        // allowed

// unknown (safe top type)
let value: unknown;

value = 10;
value = 'text';

if (typeof value === 'string') {
  value.toUpperCase();                     // safe after narrowing
}

// void (no meaningful return)
function log(msg: string): void {
  console.log(msg);
}

// never (no possible value)
function fail(msg: string): never {
  throw new Error(msg);
}
```

</details>

---

## object

Represents any non-primitive value, excluding `null` and `undefined`.

<details>
<summary>Examples</summary>

```ts
let obj: object = { a: 1 };                 // plain object

let record: { id: number; name: string } = {
  id: 1,
  name: 'Dev'
};

let fn: object = function () {};            // function
let arr: object = [1, 2, 3];                // array
```

</details>

---

## Array & ReadonlyArray

Typed indexed collections, optionally enforced as immutable.

<details>
<summary>Examples</summary>

```ts
// mutable arrays
let list: number[] = [1, 2, 3];             // number[]
let names: Array<string> = ['a', 'b'];      // Array<string>

list.push(4);                               // allowed

// readonly arrays
let readonly: ReadonlyArray<number> = [1, 2];

// readonly.push(3);                        // ❌ mutation not allowed
```

</details>

---

## Tuple Types

Fixed-length arrays with known element types at specific positions.

<details>
<summary>Examples</summary>

```ts
let tuple: [number, string] = [1, 'one'];   // fixed order

let optional: [number, string?] = [1];     // optional element

let rest: [string, ...number[]] = ['a', 1, 2];

// named tuple elements
let user: [id: number, name: string] = [1, 'Dev'];
```

</details>

---

## Enum Types

Named constants representing a closed set of values.

<details>
<summary>Examples</summary>

```ts
// numeric enum
enum Status {
  Pending,
  Active,
  Disabled
}

let s: Status = Status.Active;

// string enum
enum Direction {
  Up = 'UP',
  Down = 'DOWN'
}

let d: Direction = Direction.Up;

// const enum (inlined at compile time)
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
* `null` and `undefined` are distinct under `strictNullChecks`
* `unknown` enforces safe type narrowing
* `ReadonlyArray` provides compile-time immutability
* `const enum` values are erased and inlined at compile time
