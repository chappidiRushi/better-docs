---

id: css-math-functions
title: CSS Math & Functions
sidebar_position: 15
--------------------

# CSS Math & Functions

> CSS math functions participate in **computed-value resolution**, allowing expressions, constraints, interpolation, and unit-safe arithmetic before layout.

---

## `calc()` Resolution Rules

> `calc()` allows runtime arithmetic with compatible units and participates in value computation, not layout.

<details>
<summary>Examples</summary>

```css
.box {
  width: calc(100% - 2rem);        /* % + length */
  margin: calc(1rem + 2vh);        /* length + viewport */
  transform: translateX(calc(50% - 10px));
}

/* Rules:

- Operands must be dimensionally compatible
- Percentages resolve late (layout-dependent)
- Nested calc() is flattened
- calc() does NOT create a new value type
*/
```

</details>

---

## `min()` / `max()` / `clamp()`

> Comparison functions that select values based on resolved numeric magnitude.

<details>
<summary>Examples</summary>

```css
.card {
  width: min(90vw, 1200px);        /* smallest */
  padding: max(1rem, 2vw);         /* largest */
  font-size: clamp(1rem, 2vw, 2rem); /* min, preferred, max */
}

/* Resolution order:

1. Resolve arguments
2. Compare numeric results
3. Select winning value

Notes:
- Mixed units allowed
- Result keeps winning unit
*/
```

</details>

---

## Trigonometric Functions

> Trig functions enable angle-based math and animations using radians internally.

<details>
<summary>Examples</summary>

```css
.spinner {
  transform: rotate(calc(sin(45deg) * 1turn));
}

.circle {
  --x: calc(cos(30deg) * 100px);
  --y: calc(sin(30deg) * 100px);
  transform: translate(var(--x), var(--y));
}

/* Functions:

- sin()
- cos()
- tan()
- asin()
- acos()
- atan()
- atan2(y, x)

Notes:
- Angles auto-converted to radians
- Mostly used with custom properties
*/
```

</details>

---

## Mixing Unit Types

> CSS math allows mixing compatible dimensions but forbids invalid dimensional math.

<details>
<summary>Examples</summary>

```css
.box {
  width: calc(10rem + 5vw); /* valid */
  height: calc(100% - 3em); /* valid */
  /* width: calc(10px + 1s); ❌ invalid */
}

/* Compatibility:

- length + length → length
- % + length → length (late-resolved)
- number × length → length
- length × length → ❌ invalid
*/
```

</details>

---

## Function Fallback Behavior

> Invalid functions cause the entire declaration to be discarded unless wrapped safely.

<details>
<summary>Examples</summary>

```css
.box {
  width: min(100%, 500px); /* supported */
}

.box {
  width: clamp(10px, 5vw, 50px); /* ignored in unsupported browsers */
}

/* Safe patterns:

width: 500px;
width: clamp(10px, 5vw, 50px);

Later declaration wins if supported
*/
```

</details>

---

## Math Functions with Custom Properties

> Custom properties defer math evaluation until substitution time.

<details>
<summary>Examples</summary>

```css
:root {
  --gap: 2rem;
}

.grid {
  gap: calc(var(--gap) * 2);
}

/* Notes:

- calc() with var() resolved after cascade
- Enables theme-level math
- Invalid var() invalidates whole expression
*/
```

</details>

---

## Comparison, Rounding & Sign Functions

> CSS exposes numeric helpers for advanced calculations.

<details>
<summary>Examples</summary>

```css
.box {
  width: round(33.333%, 1px); /* round | ceil | floor */
  margin-left: sign(-10px);   /* -1 | 0 | 1 */
  opacity: abs(-0.5);
}

/* Functions:

- round(x, step?)
- ceil(x)
- floor(x)
- abs(x)
- sign(x)

Used mostly with percentages & animations
*/
```

</details>

---

## Notes

* Math functions resolve at computed-value time
* Percentages may remain unresolved until layout
* All math is unit-safe and dimension-checked
* Widely used with container queries & fluid design

## Caveats

* Invalid math invalidates the declaration
* Browser support varies for advanced functions
* Overusing calc() may impact readability (not perf)
