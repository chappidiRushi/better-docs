---
id: under-the-hood
title: Under the Hood
sidebar_position: 3
-------------------

# From JSX to Pixels: How React Renders (Chronological Mental Model)

---

## First: A Critical Clarification (Reconciliation vs Fiber)

Before the pipeline, let’s fix a common misconception:

* **Reconciliation** is the *process* of figuring out **what changed**
* **Fiber** is the *engine* that **runs reconciliation and schedules work**

In short:

> **Reconciliation decides *what* should change**
> **Fiber decides *when* and *how* that work runs**

Fiber does **not replace** reconciliation — it **implements and powers it**.

Keep this distinction in mind as we walk through the pipeline.

---

## The Big Picture (Keep This Mental Model)

```txt
JSX
 → React Elements (UI description)
 → Fiber Trees (current & work-in-progress)
 → Render Phase (reconciliation: find changes)
 → Commit Phase (apply changes)
 → Effects
```

Everything below fits into this flow.

---

## 1. Authoring Phase: Writing JSX

Developers write **JSX** to describe what the UI should look like for a given state.

Key ideas:

* JSX is **JavaScript**, not HTML
* Components describe **UI intent**, not DOM steps
* Function components are just functions that return UI descriptions

```js
function App({ name }) {
  return <h1>Hello {name}</h1>;
}
```

At this stage:

* ❌ No rendering
* ❌ No DOM
* ✅ Just declarative UI intent

---

## 2. Build Time: JSX Compilation

Before your app ever runs, JSX is compiled by tools like **Babel** or **SWC**.

What happens:

* JSX is transformed into function calls
* This happens at **build time**, not runtime

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

Important:

* JSX itself never reaches the browser
* The browser only sees plain JavaScript

---

## 3. Runtime: React Elements (UI Descriptions)

`React.createElement` produces **React elements**.

React elements are:

* Plain JavaScript objects
* Immutable
* Descriptions of *what the UI should look like*

They are often informally called the **Virtual DOM**, but they are **not** the real DOM and do nothing by themselves.

```js
{
  $$typeof: Symbol(react.element),
  type: 'h1',
  key: null,
  props: {
    children: 'Hello'
  }
}
```

At this stage:

* ❌ No DOM updates
* ❌ No effects
* ✅ Just a tree of UI descriptions

---

## 4. Starting the Render: Root Creation

Rendering begins when you create a **root** and call `render`.

```js
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'));
root.render(<App name="React" />);
```

What React does here:

* Creates the **root Fiber**
* Connects React to the host environment (DOM)
* Schedules the **initial render**

This is the entry point into the Fiber system.

---

## 5. Fiber: The Internal Representation

React does not work directly with React elements.

Instead, it builds and maintains **Fiber trees**.

Each Fiber node represents:

* A component instance
* A DOM node
* Or an internal wrapper (like fragments)

React maintains **two trees**:

* **Current tree** → what’s on the screen now
* **Work-in-progress tree** → what React is building next

These trees are the foundation for reconciliation.

---

## 6. Render Phase: Reconciliation (Finding Changes)

### What Reconciliation Is

**Reconciliation** is the process of comparing:

* The **current Fiber tree**
* With a **new work-in-progress tree** built from React elements

Its job is to determine:

* What can be reused
* What needs to change
* What needs to be created or removed

### What Happens During Reconciliation

* Function components are re-executed
* Child elements are compared by **type and key**
* Fibers are marked with **flags** describing required changes

What reconciliation **does not do**:

* ❌ Mutate the DOM
* ❌ Run effects
* ❌ Guarantee a perfect minimal diff (it’s heuristic-based)

---

### Characteristics of the Render Phase

* Must be **pure** (no side effects)
* **Interruptible and restartable**
* Can be paused, resumed, or abandoned

```txt
- Components execute top to bottom
- Hooks must be called in the same order
- No DOM mutations occur here
```

This is where **Fiber’s scheduling power matters**.

---

## 7. Scheduling, Priorities, and Concurrency (Fiber’s Job)

Fiber allows React to control *when* work happens.

Each update is assigned a **priority** (internally via lanes):

* **Urgent** — user input (typing, clicks)
* **Transition** — non-blocking updates (`startTransition`)
* **Default** — normal updates
* **Idle** — background work

Higher-priority work can:

* Interrupt lower-priority renders
* Cause React to restart reconciliation

This enables:

* Time slicing
* Concurrent rendering
* Smooth user interactions

---

## 8. Commit Phase: Applying Changes

Once React finishes reconciliation, it enters the **commit phase**.

This phase:

* Applies DOM mutations
* Updates refs
* Runs `useLayoutEffect`

Key properties:

* **Synchronous**
* **Non-interruptible**
* Always reflects the latest completed render

This is the **only phase** where the DOM is touched.

---

## 9. Effects and Browser Paint

After the commit phase:

1. `useLayoutEffect` runs (before paint)
2. Browser paints the screen
3. `useEffect` runs (after paint)

```js
useEffect(() => {
  console.log('DOM committed');
  return () => console.log('cleanup');
}, []);
```

This separation prevents layout thrashing and visual tearing.

---

## 10. Updates: State, Props, and Re-renders

When state or props change:

```js
setCount(c => c + 1);
```

What actually happens:

* An update is **scheduled**
* A priority is assigned
* Reconciliation may restart
* DOM updates happen later during commit

State updates **schedule work** — they do not directly mutate the DOM.

---

## 11. Event System (Synthetic Events)

React uses a delegated **synthetic event system**.

Key points:

* One listener per event type per root
* Cross-browser normalization
* Event-triggered updates are automatically batched
* Updates flow through Fiber like any other update

Events don’t bypass the rendering pipeline — they feed into it.

---

## Stack Reconciler vs Fiber (Why This Matters)

| Feature       | Stack (≤15) | Fiber (16+)      |
| ------------- | ----------- | ---------------- |
| Execution     | Synchronous | Interruptible    |
| Scheduling    | ❌ None      | ✅ Priority-based |
| Time slicing  | ❌           | ✅                |
| Suspense      | ❌           | ✅                |
| Concurrent UI | ❌           | ✅                |
| Architecture  | Call stack  | Fiber tree       |

Fiber is what makes modern React possible.

---

## Key Takeaways

* JSX describes **intent**, not instructions
* React elements are **immutable UI descriptions**
* Fiber is React’s internal execution engine
* Reconciliation finds **what changed**
* Commit phase applies **actual DOM updates**
* Effects always run **after commit**
* Scheduling and priorities enable smooth, concurrent UIs

---
## Visual Diagram: Current Tree vs Work-In-Progress Tree

This diagram shows **what React is actually doing internally during rendering**.

### Key Idea

React **never mutates the current UI directly**.
Instead, it builds a **new tree in memory**, then swaps it in during commit.

---

### Before an Update (Only Current Tree Exists)

```txt
Current Fiber Tree (on screen)
--------------------------------
<App>
 ├─ <Header>
 ├─ <Main>
 │   ├─ <Post />
 │   └─ <Sidebar />
 └─ <Footer>
```

* This tree **matches the DOM**
* User is seeing this UI
* No work is in progress

---

### During an Update (Render Phase)

```txt
Current Tree (read-only)        Work-In-Progress Tree (being built)
-------------------------       -----------------------------------
<App>                            <App>
 ├─ <Header>        ─────▶        ├─ <Header>        (reused)
 ├─ <Main>          ─────▶        ├─ <Main>
 │   ├─ <Post />    ─────▶        │   ├─ <Post />    (updated)
 │   └─ <Sidebar /> ─────▶        │   └─ <Sidebar /> (unchanged)
 └─ <Footer>        ─────▶        └─ <Footer>
```

What’s happening:

* React **re-runs components**
* Fibers are:

  * reused
  * cloned
  * or newly created
* Changes are recorded as **flags**, not DOM mutations

⚠️ The DOM is **untouched** during this phase.

---

### After Commit (Tree Swap)

```txt
Old Current Tree ❌ (discarded)

New Current Fiber Tree ✅ (now on screen)
----------------------------------------
<App>
 ├─ <Header>
 ├─ <Main>
 │   ├─ <Post />   ← updated DOM node
 │   └─ <Sidebar />
 └─ <Footer>
```

* Work-in-progress becomes the **new current tree**
* DOM mutations already applied
* React is ready for the next update

---

### Why This Matters

This dual-tree model enables:

* Interruptible rendering
* Safe concurrency
* Restartable renders
* Predictable commits

Without it, React would be forced to mutate the DOM mid-render.

---

## Hook-by-Hook Timing Table

This table answers one of the **most common sources of confusion**:

> *“When exactly does each hook run?”*

---

### Hook Execution Timeline

| Hook                             | Phase          | Runs When                        | Can Read DOM? | Can Mutate DOM? | Blocks Paint? |
| -------------------------------- | -------------- | -------------------------------- | ------------- | --------------- | ------------- |
| `useState`                       | Render         | During render                    | ❌             | ❌               | ❌             |
| `useReducer`                     | Render         | During render                    | ❌             | ❌               | ❌             |
| `useMemo`                        | Render         | During render                    | ❌             | ❌               | ❌             |
| `useCallback`                    | Render         | During render                    | ❌             | ❌               | ❌             |
| `useContext`                     | Render         | During render                    | ❌             | ❌               | ❌             |
| `useRef` (read/write `.current`) | Render         | During render                    | ❌             | ❌               | ❌             |
| `useLayoutEffect`                | Commit         | After DOM mutation, before paint | ✅             | ✅               | ✅             |
| `useEffect`                      | Post-Commit    | After paint                      | ✅             | ❌               | ❌             |
| `useInsertionEffect`             | Commit (early) | Before layout effects            | ❌             | ✅ (styles only) | ✅             |

---

### Simplified Timeline View

```txt
Render Phase
------------
useState
useReducer
useMemo
useCallback
useContext
useRef

Commit Phase
------------
DOM mutations
useInsertionEffect
useLayoutEffect

Browser Paint
-------------
(pixels update)

Post-Commit
-----------
useEffect
```

---

### Practical Rules of Thumb

* **Never cause side effects in render**
* **DOM reads/writes that must be synchronous → `useLayoutEffect`**
* **Most side effects → `useEffect`**
* **CSS-in-JS libraries → `useInsertionEffect`**
* **Performance optimization only → `useMemo` / `useCallback`**

---

### Common Mistake (Worth Calling Out)

```js
useLayoutEffect(() => {
  setState(...) // ⚠️ can cause layout thrashing
});
```

* Blocks paint
* Can cause visible jank
* Should be rare and intentional

---
