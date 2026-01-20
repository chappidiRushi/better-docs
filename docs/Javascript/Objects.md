---

id: objects
title: Objects and Object Methods
sidebar_position: 10

---

# `Objects` — Key-Value Pairs

> Objects in JavaScript are collections of key-value pairs where keys are strings (or symbols) and values can be any type.

---

## Object Creation

```js
const obj = { key: 'value' };           // Object literal
const obj2 = new Object();              // Using Object constructor
const obj3 = Object.create(proto);      // Object with specified prototype
```

<details>
<summary>Examples</summary>

```js
const obj = { key: 'value' };             // Simple object literal
const obj2 = new Object();                // Empty object
const proto = { greet: () => 'Hello' };
const obj3 = Object.create(proto);        // Inherits from proto
```

</details>

---

## Properties and Methods

```js
obj.key                // (string) → any — access value by key
obj['key']             // (string) → any — access value by key (alternative)
obj.method()           // (function) → any — call method
```

<details>
<summary>Examples</summary>

```js
const obj = { key: 'value', method: () => 'Hello' };
console.log(obj.key);      // 'value'
console.log(obj['key']);   // 'value'
console.log(obj.method()); // 'Hello'
```

</details>

---

## this Keyword

> The `this` keyword refers to the context object in which a function is called. It can differ depending on the calling environment.

```js
const obj = {
  name: 'John',
  greet() {
    console.log(this.name); // 'this' refers to obj
  }
};
obj.greet();  // 'John'
```

<details>
<summary>Examples</summary>

```js
const obj = {
  name: 'John',
  greet() {
    console.log(this.name); // 'this' refers to obj
  }
};
obj.greet();  // 'John'

// Arrow function version (does not rebind 'this')
const obj2 = {
  name: 'Jane',
  greet: () => {
    console.log(this.name); // 'this' is lexical, may not refer to obj2
  }
};
obj2.greet();  // undefined (or global context)
```

</details>

---

## Object Destructuring

> Destructuring allows unpacking values from arrays or properties from objects into distinct variables.

```js
const { key, method } = obj; // Extract properties into variables
```

<details>
<summary>Examples</summary>

```js
const obj = { key: 'value', method: () => 'Hello' };

// Extract properties
const { key, method } = obj;
console.log(key);    // 'value'
console.log(method()); // 'Hello'

// Renaming variables during destructuring
const { key: newKey } = obj;
console.log(newKey); // 'value'
```

</details>

---

## Object Methods

```js
Object.keys(obj)      // (object) → string[] — get an array of property names
Object.values(obj)    // (object) → any[] — get an array of property values
Object.entries(obj)   // (object) → [key, value][] — get array of key-value pairs
Object.freeze(obj)    // (object) → object — prevent modifications to object
Object.seal(obj)      // (object) → object — prevent adding/removing properties
Object.assign(target, ...sources) // (object, ...objects) → object — merge objects
```

<details>
<summary>Examples</summary>

```js
const obj = { a: 1, b: 2 };

// Object.keys - get property names
console.log(Object.keys(obj)); // ['a', 'b']

// Object.values - get property values
console.log(Object.values(obj)); // [1, 2]

// Object.entries - get key-value pairs
console.log(Object.entries(obj)); // [['a', 1], ['b', 2]]

// Object.freeze - make object immutable
Object.freeze(obj);
obj.a = 10;  // No effect, 'a' remains 1

// Object.seal - prevent adding/removing properties
Object.seal(obj);
obj.c = 3;   // No effect
delete obj.b; // No effect, 'b' remains

// Object.assign - merge objects
const obj2 = { c: 3 };
const obj3 = Object.assign({}, obj, obj2); // Merges obj and obj2
console.log(obj3); // { a: 1, b: 2, c: 3 }
```

</details>

---

## Notes

* `this` refers to the object the method is called on, unless using an arrow function (lexical `this`).
* `Object.freeze()` makes an object immutable; `Object.seal()` prevents additions or deletions, but allows modifications.
* Methods like `Object.keys()`, `Object.values()`, and `Object.entries()` only return enumerable properties.

---

## Caveats

* The `this` keyword behaves differently in arrow functions due to lexical scoping. It is important to remember that arrow functions do not have their own `this`.
* Destructuring skips undefined or null values. If an object property doesn't exist, the corresponding variable will be `undefined`.
* `Object.assign()` performs shallow copy, meaning it doesn't copy nested objects deeply.

