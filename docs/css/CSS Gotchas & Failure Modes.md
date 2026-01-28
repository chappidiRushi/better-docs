---

id: css-gotchas-failure-modes
title: CSS Gotchas & Failure Modes
sidebar_position: 33
--------------------

# CSS Gotchas & Failure Modes

> This section documents **non-obvious CSS behaviors** that frequently cause production bugs. These are not spec violations — they are **correct behavior with unintuitive mental models**.

Progression: **sizing traps → alignment surprises → layout engine edge cases → stacking & painting failures**.

---

## Percentage Height Traps

> Percentage heights resolve against the **definite size of the containing block**, not against content.

Core rules:

* Percentages require a definite height ancestor
* `height: auto` breaks the chain
* Flex/Grid change definiteness rules

Common failure modes:

* `height: 100%` doing nothing
* Nested percentages collapsing
* Scroll containers not sizing as expected

Blueprint:

```css
height: auto | <length> | <percentage> | min-content | max-content | fit-content;
```

<details>
<summary>Examples</summary>

```css
.parent { height: 100vh; }
.child  { height: 100%; }  /* works */

.parent { height: auto; }
.child  { height: 100%; }  /* ignored */

/* Flex exception */
.flex {
  display: flex;
  height: 300px;
}
.flex > .item { height: 100%; } /* definite */
```

</details>

---

## Auto Margin Behavior

> `margin: auto` participates in **free-space distribution**, not alignment logic.

Key mechanics:

* Absorbs remaining space on the axis
* Requires a definite container size
* Competes with other auto margins

Axis-specific behavior:

* Block axis: rarely useful
* Inline axis: centering
* Flex/Grid: alignment primitive

Blueprint:

```css
margin: auto | <length> | <percentage>;
```

<details>
<summary>Examples</summary>

```css
/* Horizontal centering */
.box {
  width: 200px;
  margin-inline: auto;
}

/* Flex push */
.flex {
  display: flex;
}
.flex .spacer {
  margin-left: auto; /* pushes following items */
}
```

</details>

---

## Flex Overflow Bugs

> Flex items default to `min-width: auto`, which **prevents shrinking** and causes overflow.

Root cause:

* `min-width: auto` resolves to content size
* Overrides `flex-shrink`

Symptoms:

* Flex children overflow container
* Text refuses to wrap
* Scrollbars appear unexpectedly

Canonical fix:

```css
flex-item {
  min-width: 0;
  min-height: 0;
}
```

<details>
<summary>Examples</summary>

```css
.flex {
  display: flex;
}
.flex > .item {
  flex: 1;
  min-width: 0; /* critical */
}

/* Without min-width:0 → overflow */
```

</details>

---

## Grid Min-Content Surprises

> Grid tracks respect **min-content contributions**, often preventing expected shrinking.

Key rules:

* `minmax(auto, 1fr)` != `minmax(0, 1fr)`
* Content can define track minimums
* `fr` units distribute remaining space

Common surprises:

* Grid wider than container
* Text forces track expansion

Blueprint:

```css
grid-template-columns:
  <track-size>+

<track-size> =
  <length> | <percentage> | fr | minmax() | auto | min-content | max-content;
```

<details>
<summary>Examples</summary>

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
}

/* Fix overflow */
.grid {
  grid-template-columns: minmax(0, 1fr) minmax(0, 1fr);
}
```

</details>

---

## Stacking Context Nightmares

> Stacking contexts are **atomic painting layers**. Once created, children cannot escape them.

Context creators (partial):

* `position` + `z-index` (non-auto)
* `opacity < 1`
* `transform`, `filter`, `perspective`
* `isolation: isolate`
* `will-change`

Failure patterns:

* `z-index` has no effect
* Modals appear under content
* Tooltips clipped unexpectedly

Blueprint:

```css
z-index: auto | <integer>;
```

<details>
<summary>Examples</summary>

```css
.parent {
  transform: translateZ(0); /* creates stacking context */
}

.child {
  position: absolute;
  z-index: 9999; /* trapped */
}
```

</details>

---

## Debugging Strategy

> Most CSS bugs are **mental model mismatches**, not syntax errors.

Rules:

* Inspect computed & used values
* Identify formatting context
* Check stacking context boundaries
* Look for implicit minimums

---

## Notes

* Most gotchas are spec-compliant
* Layout engines optimize for correctness, not intuition
* Defaults are conservative

## Caveats

* Browsers differ in edge cases
* DevTools hide some constraints
* Fixes often look "wrong" but are correct
