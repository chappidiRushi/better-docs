---
id: react-internals
title: React Internals
sidebar_position: 15
-------------------


# React Internals Cheat Sheet

> Deep dive into React's internal architecture, rendering process, and debugging techniques

---

## Fiber Architecture & Reconciliation

React Fiber is a reimplementation of the React core algorithm to enable incremental rendering and prioritization of updates.

| Concept        | Description                                                                            |
| -------------- | -------------------------------------------------------------------------------------- |
| Fiber          | Lightweight object representing a component and its state, props, and effects          |
| Reconciliation | Process of comparing virtual DOM with previous render to determine minimal DOM updates |
| Fiber Tree     | Data structure representing the component tree with fibers for each node               |

<details>
<summary>Conceptual Fiber Flow</summary>

```
Virtual DOM diff -> Fiber nodes created/updated -> Changes scheduled based on priority -> Commit phase updates the real DOM
```

</details>

---

## Render Phases: Render vs Commit

| Phase        | Purpose                                                                                 |
| ------------ | --------------------------------------------------------------------------------------- |
| Render Phase | React calculates which changes are needed; pure and interruptible; no DOM mutations yet |
| Commit Phase | Changes are applied to the DOM; side effects run; not interruptible                     |

<details>
<summary>Flow Example</summary>

```text
Render Phase:
- Compare old vs new virtual DOM
- Generate Fiber effect list
- Can be paused/interrupted

Commit Phase:
- Apply DOM updates
- Run useLayoutEffect and class component lifecycle methods (componentDidMount/Update)
- Cannot be interrupted
```

</details>

---

## Update Scheduling & Prioritization

React schedules updates with priorities using the Fiber architecture.

| Priority Type           | Description                                    |
| ----------------------- | ---------------------------------------------- |
| Synchronous (Immediate) | High-priority updates like user input          |
| Discrete                | Clicks, typing events that need quick feedback |
| Continuous              | Animations or transitions                      |
| Idle                    | Low-priority background updates                |

<details>
<summary>Example: Update Scheduling</summary>

```js
// Example: useTransition allows marking updates as low-priority
import { useState, useTransition } from 'react';

function App() {
  const [text, setText] = useState('');
  const [isPending, startTransition] = useTransition();

  const handleChange = e => {
    startTransition(() => setText(e.target.value)); // Low-priority update
  };

  return (
    <>
      <input onChange={handleChange} placeholder="Type here" />
      {isPending && <p>Updating...</p>}
      <p>{text}</p>
    </>
  );
}
```

</details>

---

## React Internal Debugging (react-dom source)

| Tool / Technique                 | Purpose                                                                     |
| -------------------------------- | --------------------------------------------------------------------------- |
| React DevTools                   | Inspect component tree, props, state, hooks, and re-renders                 |
| `__REACT_DEVTOOLS_GLOBAL_HOOK__` | Access DevTools hook in browser for debugging React internals               |
| Logging Fiber nodes              | Developers can log internal fibers in development builds for deep debugging |
| Profiling                        | Measure commit times, render durations, and effect executions               |

<details>
<summary>Example: Debug Fiber Node</summary>

```js
// Accessing Fiber in dev mode (not recommended for production)
const root = document.getElementById('root');
const fiberNode = Object.values(root)[0];
console.log(fiberNode); // Logs the internal Fiber structure
```

</details>

---

## Notes & Caveats

* Fiber architecture allows React to split rendering work into units and prioritize updates efficiently.
* Render phase is pure and can be paused; commit phase is synchronous and applies changes to the DOM.
* Understanding update scheduling helps optimize app performance, especially for large trees.
* Internal Fiber debugging is mostly educational; avoid relying on it in production.
