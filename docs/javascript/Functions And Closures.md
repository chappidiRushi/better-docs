---

id: functions-closures
title: Functions and Closures
sidebar_position: 7
-------------------

# Functions and Closures

> Callable objects that define execution contexts, parameter binding, and lexical scoping behavior

---

## Function Declarations

*Hoisted named functions bound to the enclosing scope.*

```js
function fn(a, b) {}
```

<details>
<summary>Examples</summary>

```js
hoisted(); // works

function hoisted(a = 1, b = 2) {
  return a + b;
}

// redeclaration allowed in same scope (non-strict)
function dup() {}
function dup() {}
```

</details>

---

## Function Expressions

*Functions created as part of an expression and assigned to variables.*

```js
const fn = function () {};
const named = function inner() {};
```

<details>
<summary>Examples</summary>

```js
const add = function (a, b) {
  return a + b;
};

// named function expression (name scoped to body only)
const factorial = function fact(n) {
  return n <= 1 ? 1 : n * fact(n - 1);
};

// not hoisted
// expr(); // ReferenceError
```

</details>

---

## Arrow Functions

*Lexically bind `this`, `arguments`, `super`, and `new.target`.*

```js
(args) => expression
(args) => { return value; }
```

<details>
<summary>Examples</summary>

```js
const sum = (a, b) => a + b;

// single param, implicit return
const inc = x => x + 1;

// no own this
const obj = {
  val: 1,
  fn: () => this.val, // undefined (lexical this)
};

// not constructible
// new (() => {})(); // TypeError
```

</details>

---

## Parameters and Arguments

*Define how data is passed into functions.*

```js
function fn(a, b = 1, ...rest) {}
```

<details>
<summary>Examples</summary>

```js
function demo(a, b = 10, ...rest) {
  return { a, b, rest };
}

demo(1);            // { a:1, b:10, rest:[] }
demo(1, undefined); // default applied
demo(1, null);      // null overrides default

// arguments object (non-arrow only)
function legacy() {
  return arguments.length;
}

// parameter destructuring
function cfg({ url, method = 'GET' }) {}
```

</details>

---

## `this` Binding Rules

*Determines the execution context object for a function call.*

```js
// default | implicit | explicit | new | lexical (arrow)
```

<details>
<summary>Examples</summary>

```js
function f() { return this; }

f();                  // global / undefined (strict)
obj.f();              // obj (implicit)
f.call(obj);          // obj (explicit)
new f();              // instance

(() => this)();       // lexical
```

</details>

---

## Closures

*Functions that retain access to variables from their lexical scope.*

```js
function outer() {
  let x;
  return function inner() {};
}
```

<details>
<summary>Examples</summary>

```js
function counter() {
  let count = 0;
  return () => ++count;
}

const c = counter();
c(); // 1
c(); // 2

// loop + closure pitfall
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i)); // 3,3,3
}

for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i)); // 0,1,2
}
```

</details>

---

## IIFE (Immediately Invoked Function Expression)

*Execute a function immediately to create an isolated scope.*

```js
(function () {})();
```

<details>
<summary>Examples</summary>

```js
(function () {
  const secret = 42;
})();

(() => {
  // arrow IIFE
})();
```

</details>

---

## Advanced / Rare Patterns

*Less common but interview-relevant function patterns.*

<details>
<summary>Examples</summary>

```js
// higher-order function
const once = fn => {
  let called = false;
  return (...args) => called ? undefined : (called = true, fn(...args));
};

// currying
const curry = a => b => c => a + b + c;

// function length property
(function (a, b, c = 1) {}).length; // 2

// new.target
function C() {
  if (!new.target) throw Error('use new');
}
```

</details>

---

## Notes

* Functions are first-class objects
* Closures are created per function invocation
* Arrow functions should not be used as methods
* Default parameters are evaluated at call time

## Caveats

* Closures can cause memory retention if misused
* `arguments` is not a real array
* Arrow functions cannot be generators or constructors
