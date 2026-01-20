---

id: control-flow
title: Control Flow
sidebar_position: 6
-------------------

# Control Flow

> Constructs that determine execution order, branching, and iteration in JavaScript

---

## `if / else`

*Conditionally executes blocks based on truthy or falsy evaluation.*

```js
if (condition) {}
else if (condition) {}
else {}
```

<details>
<summary>Examples</summary>

```js
if (value) {
  // truthy
} else {
  // falsy
}

// implicit boolean coercion
if ('') {}        // false
if ([]) {}        // true
if ({}) {}        // true

// dangling else binds to nearest if
if (a)
  if (b) foo();
  else bar();
```

</details>

---

## `switch`

*Multi-branch conditional using strict comparison (`===`).*

```js
switch (expression) {
  case value:
    break;
  default:
}
```

<details>
<summary>Examples</summary>

```js
switch (x) {
  case 1:
    foo();
    break;
  case 2:
  case 3:
    bar();
    break;
  default:
    baz();
}

// fallthrough pattern (intentional)
switch (true) {
  case x > 10:
    big(); break;
  case x > 5:
    mid(); break;
}
```

</details>

---

## `for` Loop

*Deterministic iteration with initializer, condition, and update phases.*

```js
for (init; condition; update) {}
```

<details>
<summary>Examples</summary>

```js
for (let i = 0; i < 3; i++) {
  console.log(i);
}

// omitted clauses
for (;;) {
  break; // infinite loop
}

// multiple expressions
for (let i = 0, j = 10; i < j; i++, j--) {}
```

</details>

---

## `while` Loop

*Pre-condition loop that executes while condition remains truthy.*

```js
while (condition) {}
```

<details>
<summary>Examples</summary>

```js
let i = 0;
while (i < 3) {
  i++;
}

// potential infinite loop
while (true) {
  break;
}
```

</details>

---

## `do...while`

*Post-condition loop that executes at least once.*

```js
do {} while (condition);
```

<details>
<summary>Examples</summary>

```js
let i = 0;
do {
  i++;
} while (i < 3);

// executes once even if false
do {
  run();
} while (false);
```

</details>

---

## `break` and `continue`

*Alter loop or switch execution flow.*

```js
break;        // exit loop or switch
continue;     // skip to next iteration
```

<details>
<summary>Examples</summary>

```js
for (let i = 0; i < 5; i++) {
  if (i === 2) continue;
  if (i === 4) break;
  console.log(i);
}

// switch break
switch (x) {
  case 1:
    break;
}
```

</details>

---

## Labeled Statements

*Control flow across nested loops or blocks (rarely used).*

```js
label: statement
```

<details>
<summary>Examples</summary>

```js
outer:
for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (i === j) continue outer;
  }
}
```

</details>

---

## Common Patterns & Edge Cases

*Less common but valid control-flow techniques.*

<details>
<summary>Examples</summary>

```js
// early return instead of else
if (!valid) return;
process();

// switch without break (intentional fallthrough)
switch (x) {
  case 1:
  case 2:
    handle();
}

// loop with side effects in condition
while (node = node.next) {}
```

</details>

---

## Notes

* Conditions use truthy / falsy coercion
* `switch` uses strict equality (`===`)
* `for` is preferred when iteration count is known
* `while` is preferred for condition-driven loops

## Caveats

* Missing `break` in `switch` causes fallthrough
* Infinite loops block the event loop
* Labeled control flow reduces readability and should be avoided
