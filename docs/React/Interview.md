# React Interview Definitions

---

## React Fundamentals

**What is React?**
React is a JavaScript library for building user interfaces using component-based, declarative rendering.

**Why was React created?**
React was created to manage complex UIs efficiently by minimizing direct DOM manipulation.

**What problems does React solve?**
It simplifies UI composition, state management, and optimizes rendering performance.

**Why use React instead of vanilla JavaScript?**
React provides predictable state-driven UI, reusability, and better scalability.

**What is JSX?**
JSX is a syntax extension that allows writing HTML like syntax inside JavaScript.

**Is JSX required?**
No, JSX is optional and compiles to `React.createElement`.

---

## Components, Props & State

**What is a component?**
A component is a reusable unit of UI that returns React elements.

**Difference between element and component?**
An element is an immutable object describing UI, while a component produces elements.

**What are props?**
Props are read-only data passed from parent to child components.

**Can props be modified?**
No, props are immutable and controlled by the parent.

**What is state?**
State is mutable data owned by a component that triggers re-renders.

**Why is state treated as immutable?**
Immutability enables predictable updates and efficient change detection.

**What is one-way data flow?**
Data flows from parent to child, making state changes predictable.

---

## Rendering & Lifecycle

**What happens during rendering?**
React calculates a new UI tree based on current props and state.

**What is reconciliation?**
Reconciliation is React’s process of comparing UI trees to determine minimal DOM updates.

**What is the Virtual DOM?**
An in-memory representation of the UI used to optimize real DOM updates.

**What is the render phase?**
A pure, interruptible phase where React computes UI changes.

**What is the commit phase?**
The phase where React applies changes to the DOM and runs effects.

---

## Lists & Keys

**What are keys in React?**
Keys uniquely identify list items across renders.

**Why are keys important?**
They help React update lists efficiently and correctly.

**Why avoid array index as key?**
Indexes change when lists reorder, causing incorrect updates and bugs.

---

## Hooks

**What are hooks?**
Hooks are functions that let React function components access state, lifecycle behavior, and other React features without using classes.

**Why were hooks introduced?**
To reuse logic easily and remove class component complexity.

**Rules of hooks?**
Hooks must be called unconditionally and only inside React functions.

**What does `useState` do?**
Adds local state to a function component.

**Why is state update asynchronous?**
To enable batching and improve performance.

**What does `useEffect` do?**
Runs side effects after rendering.

**When does `useEffect` run?**
After render and browser paint.

**What is effect cleanup?**
A function that cleans up resources before re-running or unmounting.

**`useEffect` vs `useLayoutEffect`?**
`useLayoutEffect` runs before paint; `useEffect` runs after paint.

**What is `useRef` used for?**
Storing mutable values without triggering re-renders.

**`useMemo` vs `useCallback`?**
`useMemo` memoizes values, `useCallback` memoizes functions.

**What is a custom hook?**
A reusable function that encapsulates hook logic.

---

## State Management

**What is lifting state up?**
Moving shared state to the closest common parent.

**What is prop drilling?**
Passing props through unnecessary intermediate components.

**What is Context API used for?**
Sharing low-frequency global data without prop drilling.

**When should Context be avoided?**
For frequently changing state that causes widespread re-renders.

**Local vs global state?**
Local state belongs to a component; global state is shared.

---

## Performance Optimization

**What causes re-renders?**
State changes, prop changes, parent renders, or context updates.

**What is `React.memo`?**
A HOC that skips re-rendering when props haven’t changed.

**What is memoization?**
Caching results to avoid unnecessary computation.

**When should memoization be avoided?**
When computations are cheap or props change frequently.

**What is batching?**
Grouping multiple state updates into a single render.

---

## Events & Forms

**What is a synthetic event?**
React’s cross-browser wrapper around native events.

**Controlled vs uncontrolled components?**
Controlled components use React state; uncontrolled use the DOM.

---

## React 18 & Concurrent Rendering

**What is concurrent rendering?**
React’s ability to interrupt and prioritize rendering work.

**What problem does it solve?**
Keeps the UI responsive during heavy updates.

**What is a transition?**
A non-urgent update that can be interrupted.

**What does `useTransition` do?**
Marks updates as non-urgent.

**What is `useDeferredValue`?**
Defers updating a value to keep the UI responsive.

---

## Suspense & Code Splitting

**What is Suspense?**
A mechanism for waiting on async dependencies before rendering.

**What is lazy loading?**
Loading components only when needed.

---

## Error Handling

**What is an error boundary?**
A component that catches rendering errors in its subtree.

**What errors do error boundaries catch?**
Errors during render, lifecycle methods, and constructors.

**Can hooks catch errors?**
No.

---

## SSR & Server Components

**What is server-side rendering (SSR)?**
Rendering React components on the server before sending HTML.

**Benefits of SSR?**
Faster initial load and improved SEO.

**What is hydration?**
Attaching React to server-rendered HTML.

**What causes hydration mismatch?**
Differences between server and client renders.

**What are Server Components?**
Components that run only on the server and ship no JS to the browser.

---

## Architecture & Scaling

**What makes a React app scalable?**
Feature-based structure, clear separation of concerns, and predictable state.

**How should large React apps be structured?**
By features rather than file types.

---

## React Fiber (Internals)

**What is React Fiber?**
React’s internal reconciliation engine enabling incremental rendering.

**Why was Fiber introduced?**
To support prioritization, interruption, and concurrency.

**What is a Fiber node?**
An object representing a component and its state in the render tree.

**What are lanes?**
Priority levels used to schedule updates.

**Why must hooks be called in order?**
Because hook state is matched by call order, not name.

---

## Class Components

**What is a class component?**
A component defined using ES6 classes with lifecycle methods.

**Why were class components needed?**
They enabled state and lifecycle before hooks.

**When are class components still used?**
For error boundaries and legacy code.

---

## Anti-Patterns

**Why is derived state an anti-pattern?**
It duplicates data and causes synchronization bugs.

**Why is mutating state bad?**
It breaks React’s change detection.

**Why avoid side effects in render?**
Render must be pure and predictable.

---

## Senior-Level Understanding

**What separates a senior React engineer?**
Architectural thinking, performance awareness, and ability to simplify systems.

---

# Hardcore / Senior Level

---

## Core Philosophy

**What is React fundamentally?**
React is a UI rendering library focused on describing state → UI mappings, not a full framework.

**What does “React is declarative” actually mean?**
You describe the desired UI state, and React determines how to update the DOM.

**What does “UI = f(state)” imply?**
The UI is a pure function of application state and props.

---

## Rendering Model

**What is rendering in React?**
Rendering is the process of executing components to produce a description of UI, not DOM updates.

**Why must render be pure?**
Because React may run, pause, restart, or discard renders.

**What is idempotent rendering?**
Multiple renders with the same inputs must produce the same output.

**Why are side effects forbidden during render?**
They can cause inconsistent UI and infinite loops.

---

## Reconciliation & Diffing

**What is reconciliation?**
Reconciliation is React’s algorithm for comparing two UI trees to compute minimal updates.

**How does React diff efficiently?**
By assuming:

* Elements of different types produce different trees
* Keys uniquely identify siblings

**What is a bailout?**
When React skips rendering a subtree because inputs haven’t changed.

---

## Fiber Architecture

**What is React Fiber?**
Fiber is React’s internal architecture that enables incremental, interruptible rendering.

**What problem did Fiber solve?**
The legacy stack reconciler was synchronous and blocked the main thread.

**What is a Fiber node?**
A JavaScript object representing a unit of work in the component tree.

**What does a Fiber node store?**
Type, props, state, hooks, effect flags, and tree relationships.

**What is the “alternate” Fiber?**
The previous committed version used for double buffering.

**Why does React use double buffering?**
To prepare a new UI tree without mutating the committed one.

---

## Scheduling & Concurrency

**What is cooperative scheduling?**
React yields control back to the browser between units of work.

**What is a unit of work?**
Processing a single Fiber node during rendering.

**What is time slicing?**
Splitting rendering into chunks to avoid blocking the main thread.

**What are lanes?**
Priority buckets that determine update scheduling.

**Why doesn’t concurrency mean parallelism?**
All React rendering runs on the single main thread.

---

## Commit Phase Internals

**Why is the commit phase not interruptible?**
Interrupting DOM mutations would lead to inconsistent UI.

**What happens during commit?**

* DOM mutations
* Ref updates
* Layout effects
* Passive effects scheduling

**What are effect flags?**
Markers describing what work must be done during commit.

---

## Hooks Internals

**How are hooks stored internally?**
As a linked list attached to the Fiber node.

**Why must hooks be called in the same order?**
Because React matches hook state by position, not name.

**What breaks if hook order changes?**
React associates state with the wrong hook.

**What is a stale closure?**
When a function captures outdated state due to closures.

**How do functional updates fix stale closures?**
They receive the latest state directly from React.

---

## Effects Deep Dive

**Why does `useEffect` run after paint?**
To avoid blocking visual updates.

**Why does `useLayoutEffect` block paint?**
It must run synchronously for DOM measurements.

**Why does Strict Mode double-invoke effects?**
To expose unsafe side effects and missing cleanup.

**When does cleanup run?**
Before the next effect and on unmount.

---

## State Management Theory

**What is a single source of truth?**
Each piece of data should be owned in exactly one place.

**Why is duplicated state dangerous?**
Multiple sources of truth drift out of sync.

**What is derived state?**
State computed from other state or props.

**Why should derived state usually not be stored?**
It introduces synchronization bugs.

---

## Server vs Client State

**What is server state?**
Remote data that requires caching, synchronization, and revalidation.

**What is client state?**
Ephemeral UI state like modals, inputs, and tabs.

**Why separate them?**
They have different lifecycles and consistency requirements.

---

## Suspense Internals

**How does Suspense work internally?**
By throwing promises during render.

**What does React do when a promise is thrown?**
Pauses rendering and shows a fallback.

**Why must Suspense integrate with rendering?**
To avoid loading waterfalls.

---

## SSR & Hydration Internals

**What is hydration from React’s perspective?**
Matching Fiber nodes to existing DOM instead of creating new ones.

**What happens on hydration mismatch?**
React discards server markup and re-renders on the client.

**Why are random values dangerous in SSR?**
They cause mismatched renders.

---

## Server Components

**What are React Server Components?**
Components that run only on the server and never ship JS.

**What problems do Server Components solve?**

* Smaller bundles
* Direct data access
* Faster startup

**How are Server Components different from SSR?**
SSR still ships JS; Server Components do not.

---

## Performance Pathologies

**What is over-rendering?**
Rendering more of the tree than necessary.

**What is over-memoization?**
Adding memoization without measurable benefit.

**What is layout thrashing?**
Repeated DOM reads and writes causing reflows.

**What is virtualization?**
Rendering only visible items in large lists.

---

## Event System

**What is event delegation in React?**
React attaches a single event listener at the root.

**Why does React use synthetic events?**
Consistency and performance.

---

## Advanced Patterns

**What is component composition?**
Building UIs by combining components instead of inheritance.

**What is a headless component?**
Logic without UI.

**What is a render prop?**
A function prop controlling rendering.

**What is ref forwarding?**
Passing refs through components to DOM nodes.

**What problem do portals solve?**
Rendering outside the DOM hierarchy while preserving context.

---

## Anti-Patterns (Advanced)

**Why is excessive `useEffect` a smell?**
It often indicates misplaced logic.

**Why are inline objects/functions problematic?**
They break referential equality.

**Why is syncing props to state dangerous?**
It creates redundant sources of truth.

---

## Architecture & Large Systems

**What makes frontend architecture scalable?**

* Clear ownership
* Predictable data flow
* Minimal shared state
* Strong conventions

**What is micro-frontend architecture?**
Independently deployed frontend modules.

**Tradeoffs of micro-frontends?**
Autonomy vs complexity and performance cost.

---

## Senior-Level Signals

**What distinguishes senior React engineers?**

* Understand rendering deeply
* Anticipate performance issues
* Choose simplicity over cleverness
* Reason about tradeoffs

**What distinguishes staff/principal engineers?**

* System-wide thinking
* Cross-team architecture
* Long-term maintainability decisions

---
