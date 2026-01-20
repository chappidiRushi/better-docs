---

id: Variables-and-data-types
title: Variables And Data Types
sidebar_position: 4
-------------------

# Variables and Data Types

> Execution context bindings, scoping rules, mutability, and value vs reference semantics

---

## Variable Declarations — `var`, `let`, `const`

*Declares variables and controls their scope, lifetime, and reassignment behavior.*

```js
var a = 1;        // function-scoped, hoisted (initialized as undefined)
let b = 2;        // block-scoped, temporal dead zone (TDZ)
const c = 3;      // block-scoped, immutable binding (not value)
```

<details>
<summary>Examples</summary>

```js
// var — function scope, re-declarable
function fn() {
  console.log(x);    // undefined (hoisted)
  var x = 10;
  var x = 20;        // allowed
}

// let — block scope, TDZ
{
  // console.log(y); // ReferenceError (TDZ)
  let y = 10;
  y = 20;            // allowed
}

// const — block scope, binding cannot be reassigned
const z = 10;
// z = 20;           // TypeError

const obj = { a: 1 };
obj.a = 2;           // allowed (mutation)
// obj = {};         // TypeError (rebinding)
```

</details>

---

## Hoisting & Temporal Dead Zone (TDZ)

*Explains when variables are created and when they can be accessed safely.*

```js
// var → hoisted + initialized
// let/const → hoisted, uninitialized (TDZ)
```

<details>
<summary>Examples</summary>

```js
console.log(v); // undefined
var v = 1;

// console.log(l); // ReferenceError
let l = 2;

// console.log(c); // ReferenceError
const c = 3;
```

</details>

---

## Primitive Types

*Basic data types that store single values and are immutable.*

```js
string      // '', ""
number      // 1, 1.5, NaN, Infinity
bigint      // 1n
boolean     // true | false
undefined   // uninitialized
null        // intentional absence
symbol      // unique identifiers
```

<details>
<summary>Examples</summary>

```js
let s = 'text';           // string
let n = 42;               // number
let nan = NaN;            // number
let inf = Infinity;       // number
let b = true;             // boolean
let u;                    // undefined
let nul = null;           // null
let id = Symbol('id');    // symbol
let big = 9007199254740991n; // bigint
```

</details>

---

## Primitive Characteristics

*Defines how primitive values behave when copied, compared, and modified.*

```js
// Immutable
// Passed by value
// Compared by value (except NaN)
```

<details>
<summary>Examples</summary>

```js
let a = 'hi';
let b = a;
b = 'bye';
console.log(a); // 'hi'

console.log(NaN === NaN);        // false
console.log(Object.is(NaN, NaN)); // true
```

</details>

---

## Reference Types

*Complex data types stored by reference, including objects, arrays, and functions.*

```js
Object
Array
Function
Date
RegExp
Map / Set
WeakMap / WeakSet
```

<details>
<summary>Examples</summary>

```js
const obj = { a: 1 };
const arr = [1, 2, 3];
const fn = function () {};
const map = new Map([["a", 1]]);
const set = new Set([1, 2]);
```

</details>

---

## Reference Semantics

*Explains how objects are passed, shared, and compared in memory.*

```js
// Stored as references
// Passed by sharing
// Compared by reference
```

<details>
<summary>Examples</summary>

```js
const o1 = { x: 1 };
const o2 = o1;
o2.x = 2;
console.log(o1.x); // 2

console.log({} === {}); // false
```

</details>

---

## Mutability vs Immutability

*Difference between changing a variable reference and changing the data itself.*

```js
// const prevents rebinding, not mutation
// Object.freeze is shallow
```

<details>
<summary>Examples</summary>

```js
const data = Object.freeze({ a: 1 });
data.a = 2;          // fails silently or throws (strict)

const deep = { a: { b: 1 } };
Object.freeze(deep);
deep.a.b = 2;        // allowed (not deep frozen)
```

</details>

---

## Type Checking

*Ways to check and identify data types at runtime.*

```js
typeof value          // primitives + function
value instanceof C   // prototype chain
Object.prototype.toString.call(value)
```

<details>
<summary>Examples</summary>

```js
typeof 'a';            // 'string'
typeof null;           // 'object' (legacy bug)

a instanceof Array;   // false
[] instanceof Array;  // true

Object.prototype.toString.call(null); // '[object Null]'
Object.prototype.toString.call([]);   // '[object Array]'
```

</details>

---

## Notes

* `var` should be avoided outside legacy code
* `const` is default; use `let` only when reassignment is required
* Primitives live on stack; references point to heap allocations (engine-level detail)
* BigInt and Number are **not interoperable** without explicit conversion

## Caveats

* `typeof null === 'object'` is a spec bug
* `Object.freeze` does **not** make objects deeply immutable
* Passing objects to functions allows mutation of caller state
