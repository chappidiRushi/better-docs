---

id: css-media-container-queries
title: Media Queries & Container Queries
sidebar_position: 23
--------------------

# Media Queries & Container Queries

> Query systems allow CSS to **conditionally apply styles** based on the environment, viewport, user preferences, or container state. They are evaluated **outside the cascade**, but influence which rules participate in it.

Progression: **media model → syntax → evaluation → container logic → advanced edge cases**.

---

## Media Evaluation Model (Foundations)

> Media queries gate **entire rules**, not individual declarations.

Evaluation characteristics:

* Evaluated before cascade
* Binary result (match / no-match)
* Re-evaluated on environment changes (resize, zoom, preference change)
* Do NOT affect specificity

Media inputs include:

* Viewport size
* Device capabilities
* User preferences
* Interaction modes

```css
/* Blueprint */
@media <media-condition> {
  <qualified-rules>
}
```

<details>
<summary>Examples</summary>

```css
@media (min-width: 768px) {
  .layout {
    grid-template-columns: 1fr 3fr;
  }
}
```

</details>

---

## Media Types & Features

> Media queries operate over **media features**, not devices.

Common feature categories:

* Size: `width`, `height`, `aspect-ratio`
* Resolution: `resolution`, `device-pixel-ratio`
* Color: `color`, `color-gamut`, `dynamic-range`
* Interaction: `hover`, `pointer`, `any-hover`
* Preferences: `prefers-color-scheme`, `prefers-reduced-motion`

```css
@media (hover: hover) and (pointer: fine) {
  .tooltip { display: block; }
}
```

---

## Range Syntax (Modern Queries)

> Range syntax removes redundancy and improves readability.

```css
/* Old */
@media (min-width: 600px) and (max-width: 900px) {}

/* New */
@media (600px <= width <= 900px) {}
```

Rules:

* Inclusive comparisons
* Order-independent
* Evaluated mathematically

<details>
<summary>Examples</summary>

```css
@media (width >= 1024px) {
  .sidebar { display: block; }
}
```

</details>

---

## Interaction & Accessibility Queries

> Queries can reflect **user intent and accessibility preferences**.

Key queries:

* `prefers-reduced-motion`
* `prefers-contrast`
* `forced-colors`
* `inverted-colors`

```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation: none !important;
  }
}
```

Notes:

* Should not be overridden casually
* Often paired with progressive enhancement

---

## Container Queries (Mental Model)

> Container queries respond to **component size**, not viewport size.

Key differences from media queries:

* Scoped to a container
* Enable true component encapsulation
* Evaluated per container instance

Two steps:

1. Declare a container
2. Query its dimensions or style

```css
/* Blueprint */
.container {
  container-type: inline-size;
}

@container (width > 400px) {
  .card { flex-direction: row; }
}
```

---

## Container Size Queries

> Size queries inspect the **content box** of the container.

Container types:

* `size` → inline + block
* `inline-size` → inline axis only

Rules:

* Only direct descendants participate
* Containers do NOT query themselves
* Writing modes affect axes

<details>
<summary>Examples</summary>

```css
.card-wrapper {
  container-type: inline-size;
}

@container (min-width: 30rem) {
  .card {
    display: grid;
    grid-template-columns: auto 1fr;
  }
}
```

</details>

---

## Style Queries

> Style queries allow conditions based on **computed styles**, not size.

```css
@container style(--variant: compact) {
  .item {
    padding: 4px;
  }
}
```

Rules:

* Query computed custom properties
* Enables state-based styling without JS
* Must avoid circular dependencies

---

## Cascade & Query Interaction

> Queries decide **which rules exist**, cascade decides **which wins**.

Key points:

* Queries do not affect specificity
* Query-matched rules fully enter cascade
* `@layer` still applies inside queries

```css
@layer components {
  @media (width > 800px) {
    .card { padding: 2rem; }
  }
}
```

---

## Performance & Re-Evaluation

> Query changes trigger **style recalculation**, not layout by default.

Triggers:

* Resize
* Zoom
* Preference change
* Container resize (container queries)

Performance notes:

* Container queries are more expensive than media queries
* Prefer fewer containers
* Avoid deep container nesting

---

## Common Pitfalls

* Forgetting to set `container-type`
* Expecting container queries to affect ancestors
* Using media queries for component logic
* Over-querying instead of using flexible layouts

---

## Notes

* Media queries are environment-scoped
* Container queries are component-scoped
* Queries gate rules, not declarations
* Cascade still determines final styles

## Caveats

* Container queries require explicit opt-in
* Style queries can create cycles
* Debugging requires browser tooling
* Not all media features are equally supported
