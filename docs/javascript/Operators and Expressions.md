---

id: operators-expressions
title: Operators and Expressions
sidebar_position: 5
-------------------

# Operators and Expressions

> All JavaScript operators used to compute values, compare operands, manipulate objects, and control execution semantics

---

## Arithmetic Operators

*Perform mathematical operations on numeric operands.*

```js
+   // addition
-   // subtraction
*   // multiplication
/   // division
%   // remainder
**  // exponentiation
++  // increment (prefix | postfix)
--  // decrement (prefix | postfix)
```

<details>
<summary>Examples</summary>

```js
let a = 5;
let b = 2;

a + b;   // 7

a - b;   // 3

a * b;   // 10

a / b;   // 2.5

a % b;   // 1

a ** b;  // 25

++a;     // 6 (prefix)
b--;     // 2 → 1 (postfix returns old value)
```

</details>

---

## Comparison Operators

*Compare two values and return a boolean.*

```js
==    // loose equality
!=    // loose inequality
===   // strict equality
!==   // strict inequality
>     // greater than
<     // less than
>=    // greater than or equal
<=    // less than or equal
```

<details>
<summary>Examples</summary>

```js
1 == '1';          // true (coercion)
1 === '1';         // false

null == undefined; // true
null === undefined; // false

NaN === NaN;       // false
Object.is(NaN, NaN); // true
```

</details>

---

## Logical Operators

*Combine expressions using short-circuit logic.*

```js
&&   // AND
||   // OR
!    // NOT
??   // nullish coalescing
```

<details>
<summary>Examples</summary>

```js
true && false;       // false
true || false;       // true
!false;              // true

0 || 'fallback';     // 'fallback'
0 ?? 'fallback';     // 0

null ?? 'default';   // 'default'
```

</details>

---

## Assignment Operators

*Assign or update variable values.*

```js
=     // assignment
+=    // add and assign
-=    // subtract and assign
*=    // multiply and assign
/=    // divide and assign
%=    // remainder and assign
**=   // exponent and assign
&&=   // logical AND assign
||=   // logical OR assign
??=   // nullish assign
```

<details>
<summary>Examples</summary>

```js
let x = 10;

x += 5;    // 15
x *= 2;    // 30
x **= 2;   // 900

let a = null;
a ??= 5;   // 5

let b = 0;
b ||= 10;  // 10

let c = true;
c &&= false; // false
```

</details>

---

## Unary Operators

*Operate on a single operand.*

```js
+value     // unary plus (number conversion)
-value     // unary minus
typeof     // type inspection
void       // discard return value
delete     // remove property
await      // resolve promise (async only)
```

<details>
<summary>Examples</summary>

```js
+'42';           // 42
-'5';            // -5

typeof null;     // 'object'

void 0;          // undefined

const obj = { a: 1 };
delete obj.a;    // true

await Promise.resolve(1); // 1
```

</details>

---

## Relational Operators

*Test relationships between operands.*

```js
in            // property existence
instanceof   // prototype chain check
```

<details>
<summary>Examples</summary>

```js
'a' in { a: 1 };          // true
[] instanceof Array;     // true
{} instanceof Object;    // true
```

</details>

---

## Bitwise Operators

*Operate on 32-bit integers at the binary level.*

```js
&   // AND
|   // OR
^   // XOR
~   // NOT
<<  // left shift
>>  // sign-propagating right shift
>>> // zero-fill right shift
```

<details>
<summary>Examples</summary>

```js
5 & 1;      // 1
5 | 1;      // 5
5 ^ 1;      // 4
~5;         // -6
5 << 1;     // 10
5 >> 1;     // 2
5 >>> 1;    // 2
```

</details>

---

## Conditional (Ternary) Operator

*Inline conditional expression.*

```js
condition ? expr1 : expr2
```

<details>
<summary>Examples</summary>

```js
const role = isAdmin ? 'admin' : 'user';
```

</details>

---

## Optional Chaining Operator

*Safely access nested properties without throwing.*

```js
?.   // optional property access
```

<details>
<summary>Examples</summary>

```js
user?.profile?.email;
fn?.(1, 2);
arr?.[0];
```

</details>

---

## Spread & Rest Operators

*Expand or collect values.*

```js
...value   // spread
...param   // rest
```

<details>
<summary>Examples</summary>

```js
const arr = [1, 2];
const copy = [...arr, 3];

function fn(...args) {
  return args.length;
}
```

</details>

---

## Comma Operator

*Evaluate multiple expressions and return the last.*

```js
expr1, expr2
```

<details>
<summary>Examples</summary>

```js
let x = (1 + 2, 3 + 4);
// x === 7
```

</details>

---

## Operator Precedence & Associativity

*Rules that define evaluation order.*

```js
// Highest → unary
// Then arithmetic
// Then comparison
// Then logical
// Assignment is right-to-left
```

<details>
<summary>Examples</summary>

```js
2 + 3 * 4;      // 14
(2 + 3) * 4;    // 20

let y;
y = y = 5;      // right-to-left
```

</details>

---

## Notes

* Operators return values, not statements
* Logical operators return operands, not booleans
* Bitwise ops convert values to 32-bit signed ints
* `instanceof` depends on prototype chain

## Caveats

* `delete` affects hidden classes and performance
* `==` introduces coercion pitfalls
* `??` cannot be mixed with `||` or `&&` without parentheses
