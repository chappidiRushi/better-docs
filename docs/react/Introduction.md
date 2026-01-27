---

id: introduction
title: Introduction
sidebar_position: 2
-------------------

# Introduction & Core Basics

> High-signal React fundamentals for experienced developers. Minimal prose, maximal coverage.

---

## What React Is

React is a **declarative**, **component-driven** UI library focused on predictable rendering and state-driven views.

**Key properties**

* Declarative UI via render functions
* Component composition over inheritance
* Unidirectional data flow
* Virtual DOM + reconciliation
* Platform-agnostic core (DOM, Native, Server)

<details>
<summary>Examples</summary>

```js
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';

// Function component (recommended)
function App() {
  return <h1>Hello React</h1>;
}

// Root API (React 18+)
const container = document.getElementById('root');
const root = createRoot(container);

root.render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

</details>

---

## JSX

 A syntax extension for JavaScript that allows you to write HTML-like code within JavaScript.

**Capabilities**

* Expressions via `{}` (not statements)
* Attribute camelCasing
* Spread props
* Conditional rendering
* Children as values

<details>
<summary>Examples</summary>

```js
// Compiles to React.createElement calls
const name = 'React';

const element = (
  <section
    id="main"
    className="container"
    data-env={process.env.NODE_ENV}
  >
    <h1>Hello, {name.toUpperCase()}</h1>

    {/* Conditional rendering */}
    {name && <p>Visible if truthy</p>}
    {name ? <A /> : <B />}

    {/* Lists */}
    <ul>
      {['A', 'B', 'C'].map((item, index) => (
        <li key={item /* prefer stable keys over index */}>{item}</li>
      ))}
    </ul>

    {/* Spreading props */}
    <Button {...buttonProps} disabled />
  </section>
);
```

</details>

---

## Rendering & Re-rendering

Rendering is **pure execution of components** → React elements. Re-rendering is triggered by **state, props, or context changes**.

**Facts**

* Re-render ≠ DOM update (diffing + batching apply)
* Entire subtree re-executes unless memoized
* State updates are async & batched (by default)

<details>
<summary>Examples</summary>

```js
import { useState, useCallback } from 'react';

function Counter({ initial = 0 }) {
  const [count, setCount] = useState(initial);

  // Functional update (safe for concurrent rendering)
  const increment = useCallback(() => {
    setCount(c => c + 1);
  }, []);

  return (
    <div>
      <p>{count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}

// Causes re-render when parent state/props change
```

</details>

---
## Notes & Caveats

* JSX requires a **single return value** (Fragment allowed)
* Re-rendering is cheap; premature memoization is not
* Keys must be **stable, predictable, unique**
* Avoid side effects during render
* Prefer function components + hooks
* Think in **state transitions**, not DOM mutations
