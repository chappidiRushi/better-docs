---

id: arrays-sets-maps
title: Arrays, Sets, and Maps
sidebar_position: 9

---

# `Arrays` — Ordered collections

> Arrays store indexed collections of values. Can hold any type of data and are dynamically sized.

---

## Array Creation

```js
const arr = [1, 2, 3];              // Array of numbers
const arr2 = new Array(5);          // Array of 5 empty slots
const arr3 = Array.from([1, 2, 3]); // Array from iterable
```

<details>
<summary>Examples</summary>

```js
const arr = [1, 2, 3];             // simple array
const arr2 = new Array(5);         // empty slots array (length 5)
const arr3 = Array.from([1, 2, 3]); // iterable to array
```

</details>

---

## Array Methods

```js
arr.push(item)         // (any) → number — adds item to the end
arr.pop()              // () → any — removes last item
arr.shift()            // () → any — removes first item
arr.unshift(item)      // (any) → number — adds item to the front
arr.concat(...arrays)  // (any[]) → Array — merge arrays
arr.slice(start, end)  // (number?, number?) → Array — shallow copy a section
arr.splice(start, deleteCount, ...items) // (number, number, any?) → Array — change array contents
```

<details>
<summary>Examples</summary>

```js
const arr = [1, 2, 3];
arr.push(4);        // arr: [1, 2, 3, 4]
arr.pop();          // arr: [1, 2, 3], returns 4
arr.shift();        // arr: [2, 3], returns 1
arr.unshift(0);     // arr: [0, 2, 3]
arr.concat([4, 5]); // arr: [0, 2, 3, 4, 5]
arr.slice(1, 3);    // returns [2, 3]
arr.splice(1, 1, 9); // arr: [0, 9, 3]
```

</details>

---

## Array Iteration

```js
arr.forEach(callback)     // (function) → void — executes a function on each item
arr.map(callback)         // (function) → Array — create a new array from results of a function
arr.filter(callback)      // (function) → Array — filters elements based on condition
arr.reduce(callback, initValue) // (function, any?) → any — reduces array to a single value
```

<details>
<summary>Examples</summary>

```js
const arr = [1, 2, 3, 4];
arr.forEach(item => console.log(item));    // prints each element
arr.map(item => item * 2);                  // returns [2, 4, 6, 8]
arr.filter(item => item % 2 === 0);         // returns [2, 4]
arr.reduce((acc, curr) => acc + curr, 0);   // returns 10 (sum of elements)
```

</details>

---

## Notes

* Arrays are 0-indexed.
* The `push()` and `pop()` methods are O(1) operations, while `shift()` and `unshift()` are O(n).

---

## `Sets` — Unique collections

> A `Set` is a collection of values where each value must be unique. Ideal for checking membership and eliminating duplicates.

---

## Set Creation

```js
const set = new Set([1, 2, 3]);    // Set from array
const set2 = new Set();            // Empty Set
```

<details>
<summary>Examples</summary>

```js
const set = new Set([1, 2, 3]);    // { 1, 2, 3 }
const set2 = new Set();            // Set {}
```

</details>

---

## Set Methods

```js
set.add(value)        // (any) → Set — adds value to set
set.delete(value)     // (any) → boolean — removes value
set.has(value)        // (any) → boolean — checks if value exists
set.size              // (number) — number of elements in set
set.clear()           // () → void — removes all values
```

<details>
<summary>Examples</summary>

```js
const set = new Set([1, 2, 3]);
set.add(4);              // Set {1, 2, 3, 4}
set.delete(3);           // Set {1, 2, 4}
set.has(2);              // returns true
set.size;                // returns 3
set.clear();             // Set {}
```

</details>

---

## Set Iteration

```js
set.forEach(callback)  // (function) → void — executes function for each element
for (let item of set)  // (any) → void — iterate over elements
```

<details>
<summary>Examples</summary>

```js
const set = new Set([1, 2, 3]);
set.forEach(item => console.log(item)); // prints 1, 2, 3
for (let item of set) {
  console.log(item);  // prints 1, 2, 3
}
```

</details>

---

## Notes

* Sets store values in insertion order.
* `Set` does not allow duplicate entries.

---

## `Maps` — Key-Value pairs

> A `Map` is a collection of key-value pairs where keys can be any type (primitive or object).

---

## Map Creation

```js
const map = new Map([['key', 'value']]);    // Map from array of pairs
const map2 = new Map();                     // Empty Map
```

<details>
<summary>Examples</summary>

```js
const map = new Map([['key', 'value']]);    // Map { 'key' => 'value' }
const map2 = new Map();                     // Map {}
```

</details>

---

## Map Methods

```js
map.set(key, value)    // (any, any) → Map — adds a key-value pair
map.get(key)           // (any) → any — retrieves value by key
map.has(key)           // (any) → boolean — checks if key exists
map.delete(key)        // (any) → boolean — removes key-value pair
map.size               // (number) — number of key-value pairs
map.clear()            // () → void — removes all pairs
```

<details>
<summary>Examples</summary>

```js
const map = new Map();
map.set('a', 1);          // Map { 'a' => 1 }
map.get('a');             // returns 1
map.has('a');             // returns true
map.delete('a');          // returns true, Map { }
map.size;                 // returns 0
map.clear();              // Map {}
```

</details>

---

## Map Iteration

```js
map.forEach(callback)      // (function) → void — executes function for each key-value pair
for (let [key, value] of map) // (any, any) → void — iterate over entries
```

<details>
<summary>Examples</summary>

```js
const map = new Map([['a', 1], ['b', 2]]);
map.forEach((value, key) => console.log(key, value));  // prints 'a 1', 'b 2'
for (let [key, value] of map) {
  console.log(key, value);  // prints 'a 1', 'b 2'
}
```

</details>

---

## Notes

* `Map` preserves the order of key insertion.
* The keys in a `Map` can be any data type, unlike `Objects` where keys are strings or symbols.
