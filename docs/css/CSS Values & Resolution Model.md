---

id: css-values-resolution-model
title: CSS Values & Resolution Model
sidebar_position: 4
-------------------

# CSS Values & Resolution Model

> How CSS values are interpreted, transformed, and resolved during style computation and layout.

---

## Value Lifecycle

> End‑to‑end journey of a CSS value from author input to final pixels on screen. Covers specified, computed, used, and actual stages as defined by the CSS Values & Units spec.

<details>
<summary>Examples</summary>

```css
/* Specified value (author input) */
.box {
  width: 50%;          /* specified as percentage */
  margin-left: 1em;   /* relative to font-size */
}

/* Computed value (after inheritance, relative units resolved) */
.container { width: 400px; font-size: 16px; }
.box {
  width: 50%;          /* still percentage at computed stage */
  margin-left: 16px;  /* 1em resolved against computed font-size */
}

/* Used value (after layout) */
.box {
  width: 200px;        /* 50% of containing block */
}

/* Actual value (post-paint, snapping, rounding) */
/* width may become 199.999px → device pixel aligned */
```

</details>

---

## Percentage Resolution Timing

> Defines when and against what reference percentages are resolved. Timing varies by property and may occur at computed-value or layout time.

<details>
<summary>Examples</summary>

```css
/* Percentages resolved against different references */
.parent {
  width: 400px;
  height: 300px;
}

.child {
  width: 50%;    /* resolved during layout → 200px */
  height: 50%;   /* resolved during layout → 150px */
}

/* Font-relative percentages */
p {
  font-size: 200%; /* resolved at computed-value time */
}

/* Padding percentages always resolve against inline size */
.box {
  width: 300px;
  padding-top: 10%;   /* 30px, NOT height-based */
}
```

</details>

---

## Cyclic Dependencies

> Situations where values depend on themselves directly or indirectly. Cycles are detected during resolution and handled via spec-defined fallback rules.

<details>
<summary>Examples</summary>

```css
/* Direct cycle → invalid */
.box {
  width: calc(100% - width); /* invalid, dropped */
}

/* Indirect cycle via percentages */
.parent {
  width: auto;
}

.child {
  width: 50%; /* depends on parent width → auto → depends on children */
}

/* Result: layout-defined fallback behavior */
```

</details>

---

## Dependency Graphs

> Internal graph used by the engine to resolve interdependent values safely. Ensures deterministic resolution order across inherited, relative, and layout-dependent values.

<details>
<summary>Examples</summary>

```css
/* Dependency ordering */
.container {
  font-size: 20px;
}

.child {
  width: 10em;   /* depends on font-size */
  padding: 5%;   /* depends on containing block width */
}

/* Resolution order (simplified):
   1. font-size
   2. width
   3. padding
*/
```

</details>

---

## Initial vs Computed Values

> Difference between spec-defined defaults and resolved author values. Highlights how inheritance, relative units, and keywords affect computation.

<details>
<summary>Examples</summary>

```css
/* Initial value (from spec) */
div {
  display: block; /* initial for div */
}

/* Computed value */
p {
  font-size: 1em;  /* computed from parent font-size */
}

/* Explicit reset */
.reset {
  all: initial;    /* resets to spec initial values */
}
```

</details>

---

## calc() Participation in Resolution

> `calc()` expressions are preserved until enough information exists to resolve them. Participates lazily in resolution and may span multiple value stages.

<details>
<summary>Examples</summary>

```css
/* Mixed units */
.box {
  width: calc(100% - 2rem);
}

/* Resolution steps */
/* 1. 2rem → resolved at computed-value time */
/* 2. 100% → resolved at layout time */
/* 3. calc evaluated when both sides known */

/* Nested calc */
.box {
  margin-left: calc(10px + calc(2em * 2));
}

/* Invalid calc */
.box {
  height: calc(auto + 10px); /* invalid → declaration dropped */
}
```

</details>

---

## Notes

* Percentages may resolve at **computed** or **used** time depending on property
* Cycles do not crash parsing; engines apply defined fallback rules
* `calc()` does not force early resolution
* Dependency graphs are engine-internal but spec-driven

## Caveats

* `auto` often defers resolution until layout
* Rounding occurs at the **actual value** stage
* Different properties resolve percentages against different references
