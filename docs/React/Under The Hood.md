---

id: under-the-hood
title: Under the Hood
sidebar_position: 3
-------------------

# From JSX to Pixels: How React Renders

> A concise, end‑to‑end view of what happens from writing JSX to seeing updates on the screen.

---

## Important Clarification (Reconciliation vs Fiber)

Before diving in, let’s correct a common misconception:

* **Reconciliation** is the **process** that determines *what changed* between renders.
* **Fiber** is the **internal architecture (data structure + reconciler + scheduler)** that *executes reconciliation* and *controls when work happens*.

In short:

> **Reconciliation decides *what* should change.**
> **Fiber decides *when* and *how* those changes are processed.**

Fiber does **not** replace reconciliation — it **powers it**.

---

## 1. Writing JSX (Authoring Phase)

Developers write JSX to describe **what the UI should look like for a given state**.

Key points:

* JSX is JavaScript, not HTML
* Components express **UI intent**, not DOM instructions
* Function components are just functions that return UI descriptions

<details>
<summary>Example</summary>

```js
function App({ name }) {
  return <h1>Hello {name}</h1>;
}
```

</details>

---

## 2. JSX Compilation (Build Time)

JSX is compiled by tools like **Babel** or **SWC**.

* Happens at **build time**, not runtime
* Transforms JSX into function calls
* Produces plain JavaScript objects

<details>
<summary>Example</summary>

```js
// JSX
<h1 className="title">Hello</h1>;

// Compiled output (conceptual)
React.createElement(
  'h1',
  { className: 'title' },
  'Hello'
);
```

</details>

---

## 3. React Elements (UI Description Phase)

`React.createElement` returns **React elements** — immutable descriptions of the UI.

Key points:

* Elements are plain JavaScript objects
* They describe *what the UI should look like*
* Often (informally) called the **Virtual DOM**
* **No DOM updates or effects happen here**

<details>
<summary>Example</summary>

```js
function MyComp() {
  return <h1>Hello world <p>meow P</p></h1>;
}

// Simplified React element structure
{
  $$typeof: Symbol(react.element),
  type: 'h1',
  key: null,
  props: {
    children: [
      'Hello world ',
      { type: 'p', props: { children: 'meow P' } }
    ]
  }
}
```

</details>

---

## 4. Root Creation & Initial Render

Rendering begins when a **root** is created and `render` is called.

What happens:

* React creates the **root Fiber node**
* Connects React to the host environment (DOM)
* Starts the first reconciliation

<details>
<summary>Example</summary>

```js
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'));
root.render(<App name="React" />);
```

</details>

---

## 5. Reconciliation (Render Phase — Finding Changes)

### What Reconciliation Is

**Reconciliation** is the algorithmic process React uses to determine **what changed** between renders.

It works by:

* Comparing the **current Fiber tree**
* With a **new work‑in‑progress Fiber tree** built from React elements
* Deciding what can be **reused, updated, or replaced**

What reconciliation **does**:

* Re-runs function components
* Compares elements by type and key
* Marks Fibers with **flags** describing required changes

What reconciliation **does NOT do**:

* ❌ Mutate the DOM
* ❌ Run effects
* ❌ Guarantee a perfect minimal diff (it is heuristic-based)

---

### Characteristics of the Render Phase

* Expected to be **pure** (no side effects)
* **Interruptible** and restartable
* Work can be **paused or discarded** if higher-priority updates arrive

<details>
<summary>Notes</summary>

```txt
- Function components execute top to bottom
- Hooks must be called in the same order
- No DOM mutations occur here
```

</details>

---

## 6. React Fiber (The Engine Behind Rendering)

### What Fiber Is

**Fiber** is React’s internal architecture consisting of:

* A **data structure** (Fiber nodes)
* A **reconciler** (diffing logic)
* A **scheduler** (priorities and timing)

Each component instance corresponds to a **Fiber node**, forming a Fiber tree.

---

### What Fiber Enables

Fiber allows React to:

* Split work into **small units**
* Pause, resume, or abandon rendering work
* Assign **priorities** to updates
* Support **concurrent rendering**

Without Fiber, rendering would be fully synchronous and blocking.

---

### Scheduling & Priorities

React assigns priorities to updates:

* **Urgent** — user input (typing, clicks)
* **Transition** — non-blocking UI updates (`startTransition`)
* **Default** — normal updates
* **Idle** — background or offscreen work

Higher-priority work can interrupt lower-priority work.

---

### Capabilities Enabled by Fiber

* Incremental rendering (time slicing)
* Concurrent rendering
* Automatic batching
* Suspense & lazy loading
* Streaming SSR
* Improved error handling via error boundaries

---

## 7. Commit Phase (Applying Changes)

Once reconciliation completes, React enters the **commit phase**.

What happens here:

* DOM mutations
* Ref updates
* `useLayoutEffect` callbacks

Characteristics:

* **Synchronous and non-interruptible**
* Always reflects the latest finished render

---

## 8. Effects & Lifecycle Timing

After the commit phase:

1. `useLayoutEffect` runs **before paint**
2. Browser paints
3. `useEffect` runs **after paint**

<details>
<summary>Example</summary>

```js
useEffect(() => {
  console.log('DOM committed');
  return () => console.log('cleanup');
}, []);
```

</details>

---

## 9. Updates: State & Props Changes

When state or props change:

* An update is **scheduled**
* Fiber assigns it a priority
* Reconciliation may restart

<details>
<summary>Example</summary>

```js
setCount(c => c + 1); // schedules work, not an immediate DOM change
```

</details>

---

## 10. Event System (Synthetic Events)

React uses a **delegated synthetic event system**.

Key points:

* One listener per event type per root container
* Cross-browser normalization
* Event-triggered updates are batched and scheduled via Fiber

---

## Mental Model Summary

```txt
JSX
 → React Elements
 → Fiber Tree
 → Reconciliation (render phase — find changes)
 → Commit Phase (apply DOM updates)
 → Effects
```

---

## Stack Reconciler vs Fiber

| Feature        | Stack (≤15) | Fiber (16+)      |
| -------------- | ----------- | ---------------- |
| Execution      | Synchronous | Interruptible    |
| Scheduling     | ❌ None      | ✅ Priority-based |
| Time slicing   | ❌           | ✅                |
| Suspense       | ❌           | ✅                |
| Concurrent UI  | ❌           | ✅                |
| Architecture   | Call stack  | Fiber node tree  |
| Error handling | Limited     | Error boundaries |

---

## Key Takeaways

* **Reconciliation determines what changed**
* **Fiber schedules and executes rendering work**
* React elements are descriptions, not DOM nodes
* Render and commit phases are strictly separated
* DOM mutations happen only during the commit phase
