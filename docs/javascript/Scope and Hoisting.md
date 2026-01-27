---

id: scope-hoisting
title: Scope and Hoisting
sidebar_position: 8
-------------------

# Scope and Hoisting

> Rules that govern variable visibility, lifetime, and binding resolution during code execution

---

## Global Scope

*Variables accessible everywhere in the program.*

```js
var a;
let b;
const c;
```

<details>
<summary>Examples</summary>

```js
var x = 1;        // global object property (non-module)
let y = 2;        // global lexical binding

console.log(window.x); // 1
console.log(window.y); // undefined

// implicit globals (non-strict)
function leak() {
  z = 3;          // creates global
}
```

</details>

---

## Function Scope

*Variables scoped to the body of a function.*

```js
function fn() {
  var x;
}
```

<details>
<summary>Examples</summary>

```js
function test() {
  var a = 1;
  if (true) {
    var a = 2;   // same scope
  }
  return a;      // 2
}

// parameters are function-scoped
function args(a, b) {
  return a + b;
}
```

</details>

---

## Block Scope

*Variables limited to the nearest block `{}`.*

```js
{
  let x;
  const y = 1;
}
```

<details>
<summary>Examples</summary>

```js
if (true) {
  let a = 1;
  const b = 2;
}
// a, b not accessible

for (let i = 0; i < 3; i++) {}
// i not accessible

// block scope shadowing
let x = 1;
{
  let x = 2;
}
```

</details>

---

## Lexical Scope

*Scope determined by source code structure, not runtime execution.*

```js
function outer() {
  function inner() {}
}
```

<details>
<summary>Examples</summary>

```js
const x = 1;

function outer() {
  const x = 2;
  function inner() {
    return x; // 2
  }
  return inner();
}

// dynamic scope does not exist in JS
```

</details>

---

## Hoisting Behavior

*Declaration processing phase before code execution.*

```js
// var → hoisted + initialized
// let / const → hoisted, uninitialized (TDZ)
// function → fully hoisted
```

<details>
<summary>Examples</summary>

```js
console.log(a); // undefined
var a = 1;

// console.log(b); // ReferenceError (TDZ)
let b = 2;

hoisted();
function hoisted() {}

// function expression not hoisted
// expr(); // TypeError
var expr = function () {};
```

</details>

---

## Temporal Dead Zone (TDZ)

*Time between entering scope and variable initialization.*

<details>
<summary>Examples</summary>

```js
{
  // console.log(x); // ReferenceError
  let x = 1;
}

// default parameters create their own scope
function fn(a = b, b = 2) {} // ReferenceError
```

</details>

---

## Scope Chain Resolution

*Identifier lookup proceeds outward through nested scopes.*

<details>
<summary>Examples</summary>

```js
let a = 1;

function outer() {
  let b = 2;
  function inner() {
    let c = 3;
    return a + b + c; // resolves outward
  }
  return inner();
}
```

</details>

---

## Rare / Edge Patterns

*Uncommon but interview-relevant scoping cases.*

<details>
<summary>Examples</summary>

```js
// catch block scope
try {
  throw 1;
} catch (e) {
  let e2 = 2;
}
// e2 not accessible

// function declaration in blocks (non-strict)
if (true) {
  function f() {}
}

// eval modifies scope (avoid)
function evil() {
  eval('var x = 1');
  return x;
}
```

</details>

---

## Notes

* JavaScript uses lexical scoping
* `let` / `const` introduce block scope
* Hoisting happens per execution context
* Modules always run in strict mode

## Caveats

* Implicit globals cause leaks
* TDZ errors occur at runtime, not parse time
* `eval` and `with` break scope guarantees
