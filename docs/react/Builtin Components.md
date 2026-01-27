---

id: built-in-react-components
title: Built-in React Components
sidebar_position: 6
-------------------

# Built-in React Components

> React provides a small set of built-in components that solve common structural, performance, and tooling-related problems.

---

## Overview

Built-in React components are special components shipped with React itself. They are not user-defined and have specific behavior understood by the React renderer.

They help with:

* Grouping elements without extra DOM nodes
* Measuring rendering performance
* Handling async loading states
* Catching bugs early in development
* Preserving and restoring UI state

---

## `<Fragment>`

`<Fragment>` lets you group multiple JSX elements **without adding an extra DOM node**.

You can also use the shorthand syntax `<>...</>`.

<details>
<summary>Example</summary>

```js
function List() {
  return (
    <React.Fragment>
      <li>Item A</li>
      <li>Item B</li>
    </React.Fragment>
  );
}

// Shorthand syntax
function App() {
  return (
    <>
      <h1>Title</h1>
      <p>Description</p>
    </>
  );
}
```

</details>

**When to use:**

* Avoid unnecessary wrapper elements
* Maintain valid HTML structure (e.g. table rows)

---

## `<Suspense>`

`<Suspense>` lets you display a fallback UI while its children are loading.

It works with:

* `React.lazy`
* Suspense-enabled data fetching

<details>
<summary>Example</summary>

```js
const Profile = React.lazy(() => import('./Profile'));

function App() {
  return (
    <Suspense fallback={<p>Loading profile...</p>}>
      <Profile />
    </Suspense>
  );
}
```

</details>

**Key points:**

* Fallback is shown until children are ready
* Suspense boundaries can be nested

---

## `<StrictMode>`

`<StrictMode>` enables additional **development-only checks** and warnings.

It does **not** affect production builds.

<details>
<summary>Example</summary>

```js
import { StrictMode } from 'react';

const root = createRoot(document.getElementById('root'));

root.render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

</details>

**What it helps detect:**

* Unsafe lifecycle usage
* Unexpected side effects
* Deprecated APIs
* Double-invoked render/effects (development only)

---

## `<Profiler>`

`<Profiler>` lets you **measure rendering performance** of a React subtree programmatically.

It reports timing information whenever a component renders.

<details>
<summary>Example</summary>

```js
function onRender(
  id,
  phase,
  actualDuration,
  baseDuration
) {
  console.log({ id, phase, actualDuration, baseDuration });
}

function App() {
  return (
    <Profiler id="App" onRender={onRender}>
      <Dashboard />
    </Profiler>
  );
}
```

</details>

**Common use cases:**

* Identifying slow renders
* Measuring the impact of memoization

---

## `<Activity>` (Experimental)

`<Activity>` lets you **hide and restore UI along with its internal state**.

Unlike conditional rendering, state is preserved while inactive.

> This API is experimental and subject to change.

<details>
<summary>Example</summary>

```js
function App({ isVisible }) {
  return (
    <Activity mode={isVisible ? 'visible' : 'hidden'}>
      <ChatWindow />
    </Activity>
  );
}
```

</details>

**Why it matters:**

* Preserve component state while hidden
* Useful for tabs, offscreen UI, and transitions

---

## Summary

* Built-in components solve structural and performance problems
* They do not render like normal components
* Some are development-only (`StrictMode`, `Profiler`)
* Some manage async or UI lifecycle (`Suspense`, `Activity`)

Use them deliberately to improve correctness, performance, and maintainability.
