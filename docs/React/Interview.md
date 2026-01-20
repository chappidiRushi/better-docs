---
id: interview
title: Interview
sidebar_position: 2
-------------------
# React Interview Questions & Answers
---

## Fundamentals

**Q: What is React?**
React is a JavaScript library for building component-based user interfaces using declarative rendering.

**Q: Why was React created?**
React was created to solve UI complexity and performance issues caused by frequent DOM updates.

**Q: What problems does React solve?**
It simplifies UI composition, state management, and efficient DOM updates.

**Q: Why use React instead of vanilla JS?**
React provides abstraction for state-driven UI, reusability, and predictable rendering.

**Q: What is JSX?**
JSX is a syntax extension that allows writing UI markup inside JavaScript.

**Q: Is JSX required in React?**
No, JSX is optional and compiles to React.createElement calls.

**Q: What is a component?**
A component is a reusable unit of UI that returns React elements.

**Q: Difference between element and component?**
An element is an immutable object, while a component is a function or class that creates elements.

**Q: What are props?**
Props are read-only inputs passed from parent to child components.

**Q: Can props be modified?**
No, props are immutable and owned by the parent.

**Q: What is state?**
State is mutable data owned by a component that triggers re-renders.

**Q: Why is state immutable?**
Immutability enables predictable updates and efficient change detection.

**Q: What is one-way data flow?**
Data flows from parent to child components.

**Q: What is declarative programming?**
You describe what the UI should look like, not how to update it.

---

## Rendering & Lifecycle

**Q: What happens during rendering?**
React calculates the next UI tree based on props and state.

**Q: What is reconciliation?**
Reconciliation is how React compares previous and next UI trees to decide updates.

**Q: What is the virtual DOM?**
The virtual DOM is an in-memory representation used to optimize real DOM updates.

**Q: What is diffing?**
Diffing is the process of comparing two virtual DOM trees.

**Q: What are keys and why are they important?**
Keys help React identify list items between renders.

**Q: Why not use array index as key?**
Indexes can cause incorrect UI updates when list order changes.

**Q: What are lifecycle phases?**
Mounting, updating, and unmounting.

**Q: When does React re-render a component?**
When state or props change, or when its parent re-renders.

**Q: What is the render phase?**
The phase where React calculates UI without side effects.

**Q: What is the commit phase?**
The phase where React applies changes to the DOM.

---

## Hooks

**Q: What are hooks?**
Hooks let functional components use state, lifecycle, and other React features.

**Q: Why were hooks introduced?**
To reuse logic without HOCs or render props and remove class complexity.

**Q: Rules of hooks?**
Hooks must be called at the top level and only inside React functions.

**Q: What does useState do?**
It adds local state to a functional component.

**Q: Why is setState async?**
To enable batching and better performance.

**Q: What does useEffect do?**
It runs side effects after rendering.

**Q: When does useEffect run?**
After render and after DOM paint.

**Q: What is effect cleanup?**
A function returned from useEffect to clean resources.

**Q: Difference between useEffect and useLayoutEffect?**
useLayoutEffect runs synchronously before paint.

**Q: What is the dependency array?**
It controls when effects or memoized values re-run.

**Q: What happens if dependencies are wrong?**
It can cause stale data or infinite loops.

**Q: What is useRef used for?**
To store mutable values without causing re-renders.

**Q: useMemo vs useCallback?**
useMemo memoizes values; useCallback memoizes functions.

**Q: What is a custom hook?**
A reusable function that encapsulates hook logic.

---

## State Management

**Q: What is lifting state up?**
Moving shared state to the nearest common ancestor.

**Q: What is prop drilling?**
Passing props through unnecessary intermediate components.

**Q: How does Context help?**
Context shares data without prop drilling.

**Q: When should you avoid Context?**
When frequent updates cause wide re-renders.

**Q: Difference between local and global state?**
Local state belongs to a component; global state is shared.

---

## Performance Optimization

**Q: What causes re-renders?**
State changes, prop changes, or parent re-renders.

**Q: What is React.memo?**
A HOC that skips re-renders when props don’t change.

**Q: What is memoization?**
Caching results to avoid unnecessary recomputation.

**Q: When should you not memoize?**
When computation is cheap or props change often.

**Q: What is over-memoization?**
Using memoization without measurable benefit.

**Q: What is batching?**
Grouping multiple state updates into one render.

---

## Events & Forms

**Q: What is a synthetic event?**
A cross-browser wrapper around native browser events.

**Q: Controlled vs uncontrolled components?**
Controlled use React state; uncontrolled use the DOM.

**Q: When to use uncontrolled components?**
For simple forms or integrating with non-React code.

---

## Advanced Rendering (React 18)

**Q: What is concurrent rendering?**
It allows React to interrupt and prioritize rendering work.

**Q: What problem does concurrent rendering solve?**
It keeps the UI responsive during heavy updates.

**Q: What is a transition?**
A non-urgent update that can be interrupted.

**Q: What does useTransition do?**
It defers non-urgent updates.

**Q: What is useDeferredValue?**
It delays updating a value for smoother UI.

**Q: What is Fiber?**
React’s internal architecture for incremental rendering.

---

## Suspense & Code Splitting

**Q: What is Suspense?**
It lets React wait for async dependencies before rendering.

**Q: How does Suspense work internally?**
By throwing promises during render.

**Q: What is lazy loading?**
Loading components only when needed.

**Q: Difference between Suspense and loading state?**
Suspense integrates loading into rendering.

---

## Error Handling

**Q: What is an error boundary?**
A component that catches rendering errors in its subtree.

**Q: What errors do error boundaries catch?**
Errors during render, lifecycle, and constructors.

**Q: Can hooks catch errors?**
No, only error boundaries can.

---

## SSR & Server Components

**Q: What is server-side rendering?**
Rendering React components on the server.

**Q: Benefits of SSR?**
Faster initial load and better SEO.

**Q: What is hydration?**
Attaching React to server-rendered HTML.

**Q: What causes hydration mismatch?**
Different server and client renders.

**Q: What are Server Components?**
Components that run only on the server.

**Q: Difference between Server and Client Components?**
Server Components don’t ship JS to the browser.

---

## Internals & Advanced Concepts

**Q: What are lanes in React?**
Internal priority buckets for scheduling updates.

**Q: What is a bailout?**
When React skips rendering a subtree.

**Q: What is strict mode double rendering?**
A development-only check to detect side effects.

**Q: What is time slicing?**
Breaking rendering work into interruptible chunks.

---

## Anti-Patterns

**Q: Why is derived state an anti-pattern?**
It causes duplication and synchronization bugs.

**Q: Why mutating state is bad?**
It breaks React’s change detection.

**Q: Why excessive useEffect is problematic?**
It indicates misplaced logic.

---

## Tooling & Debugging

**Q: What is React DevTools?**
An official tool for inspecting React components.

**Q: What does the Profiler do?**
It measures render performance.

**Q: How do you debug re-renders?**
Using React DevTools and memoization analysis.

---

## System Design & Architecture

**Q: How do you structure a large React app?**
By feature-based folders and component colocation.

**Q: How do you manage side effects at scale?**
By centralizing effects and avoiding unnecessary ones.

**Q: How do you optimize initial load?**
Using code splitting, SSR, and caching.

---

---

## 1. What is frontend architecture?

**Answer:**
Frontend architecture is how a frontend application is **structured**, including:

* How components are organized
* How state is managed
* How data flows
* How logic is separated from UI
* How the app scales and stays maintainable

Good architecture makes an app **easy to extend, test, and debug**.

---

## 2. How does data flow in React?

**Answer:**
React uses **one-way (unidirectional) data flow**:

* Parent components pass data to children via **props**
* Children can notify parents via **callbacks**

This makes state changes predictable and easier to debug.

---

## 3. What is the difference between props and state?

**Answer:**

* **Props**: Read-only data passed from a parent component
* **State**: Internal, mutable data owned by a component

Props control **what a component receives**, state controls **how it behaves**.

---

## 4. What is “lifting state up”?

**Answer:**
When multiple components need the same data, you:

1. Move the state to their **closest common parent**
2. Pass it down via props

This avoids duplicated or inconsistent state.

---

## 5. When should you use Context API?

**Answer:**
Use Context when data is:

* Global or semi-global
* Needed by many components at different levels

Examples:

* Auth user
* Theme
* Language
* Feature flags

Avoid using Context for **high-frequency updates** or everything, as it can hurt performance.

---

## 6. How do you structure a React project?

**Answer (common approach):**

```
src/
  components/
  features/
    auth/
      AuthPage.tsx
      authSlice.ts
      authApi.ts
  hooks/
  services/
  utils/
  pages/
```

Feature-based structure scales better than grouping by file type.

---

## 7. What are controlled vs uncontrolled components?

**Answer:**

* **Controlled**: Form values are controlled by React state
* **Uncontrolled**: Form values are handled by the DOM (using refs)

Controlled components are preferred for validation and predictable behavior.

---

## 8. What is a custom hook and why use it?

**Answer:**
A custom hook is a function that:

* Starts with `use`
* Encapsulates reusable logic

Example use cases:

* Data fetching
* Form handling
* Debouncing
* Authentication logic

They improve **code reuse and separation of concerns**.

---

## 9. How do you handle side effects in React?

**Answer:**
With the `useEffect` hook:

* API calls
* Subscriptions
* Timers
* DOM interactions

You must:

* Specify dependencies correctly
* Clean up effects to avoid memory leaks

---

## 10. What is memoization and when should you use it?

**Answer:**
Memoization avoids unnecessary re-computation or re-rendering using:

* `React.memo`
* `useMemo`
* `useCallback`

Use it when:

* Components re-render frequently
* Computations are expensive

Don’t overuse it, as it adds complexity.

---

## 11. What is the difference between client-side and server-side rendering?

**Answer:**

* **CSR**: Browser downloads JS and renders UI (React SPA)
* **SSR**: Server renders HTML before sending it (Next.js)

SSR improves:

* SEO
* Initial load performance

---

## 12. How do you manage global state in React?

**Answer:**
Options include:

* Context API
* Redux / Redux Toolkit
* Zustand / Jotai
* React Query (for server state)

Use the **simplest solution** that meets the app’s needs.

---

## 13. What is the difference between server state and client state?

**Answer:**

* **Server state**: Data from APIs (cache, sync, revalidation)
* **Client state**: UI state (modals, tabs, inputs)

Libraries like **React Query** are optimized for server state.

---

## 14. How do you prevent unnecessary re-renders?

**Answer:**

* Keep state minimal
* Use keys correctly
* Memoize components and callbacks
* Avoid inline object/function props
* Split components logically

---

## 15. What makes a React app scalable?

**Answer:**

* Feature-based architecture
* Clear separation of concerns
* Predictable state management
* Reusable components and hooks
* Good naming and conventions

---

## 16. What problem do keys solve in React lists?

**Answer:**
Keys help React identify which items changed, were added, or removed.
They allow React to **reconcile efficiently** and avoid incorrect re-renders.

---

## 17. Why is using array index as a key usually a bad idea?

**Answer:**
Because when the list changes (insert, delete, reorder), indices change, causing:

* Incorrect DOM updates
* Broken component state

Use a **stable unique ID** instead.

---

## 18. What is reconciliation in React?

**Answer:**
Reconciliation is React’s process of:

* Comparing the previous Virtual DOM with the new one
* Calculating the minimal set of DOM updates

This is why React is fast.

---

## 19. What is the Virtual DOM?

**Answer:**
A lightweight JavaScript representation of the real DOM.
React updates the Virtual DOM first, then efficiently updates the real DOM.

---

## 20. What causes a component to re-render?

**Answer:**

* State changes
* Props changes
* Parent re-renders
* Context value changes

---

## 21. What is prop drilling and how do you avoid it?

**Answer:**
Prop drilling is passing props through many levels unnecessarily.

Solutions:

* Context API
* Component composition
* State management libraries

---

## 22. What is the difference between `useEffect` and `useLayoutEffect`?

**Answer:**

* `useEffect`: Runs **after paint** (non-blocking)
* `useLayoutEffect`: Runs **before paint** (blocking)

Use `useLayoutEffect` only when you must measure or mutate the DOM synchronously.

---

## 23. What is a render prop?

**Answer:**
A pattern where a component receives a **function as a prop** to control what it renders.

Used for:

* Logic reuse
* Flexible composition

---

## 24. What is component composition in React?

**Answer:**
Building complex UIs by combining small components instead of inheritance.

Examples:

* `children`
* Slot-like APIs
* Wrapper components

---

## 25. What is hydration in React?

**Answer:**
Hydration is when React attaches event listeners and state to server-rendered HTML (SSR).

Common in frameworks like **Next.js**.

---

## 26. What is code splitting and why is it important?

**Answer:**
Code splitting loads JavaScript **only when needed**.

Benefits:

* Faster initial load
* Smaller bundles

Common tools:

* `React.lazy`
* Dynamic imports

---

## 27. What is the difference between `useRef` and `useState`?

**Answer:**

* `useState`: Triggers re-render when updated
* `useRef`: Persists value without re-rendering

Used for DOM refs or mutable values.

---

## 28. What are error boundaries?

**Answer:**
Components that catch JavaScript errors in the component tree and render fallback UI.

They:

* Catch render errors
* Prevent app crashes

---

## 29. How do you handle performance issues in large React apps?

**Answer:**

* Memoization
* Virtualization (e.g., react-window)
* Code splitting
* Avoid unnecessary state
* Profiling with React DevTools

---

## 30. What is the difference between SPA and MPA?

**Answer:**

* **SPA**: Single page, client-side routing
* **MPA**: Multiple pages, full page reloads

SPAs feel faster but need good architecture for performance and SEO.

---

## 31. What is the difference between controlled state and derived state?

**Answer:**

* **Controlled state**: Explicitly stored and updated via `useState`
* **Derived state**: Computed from props or other state

Avoid storing derived state when possible to prevent inconsistencies.

---

## 32. Why is duplicating state considered an anti-pattern?

**Answer:**
Because multiple sources of truth can get out of sync, causing bugs and complex logic.

---

## 33. How do you design a scalable state management strategy?

**Answer:**

* Local state for local UI
* Context for low-frequency global state
* Dedicated libraries for complex or shared state
* Separate server state from client state

---

## 34. What is server-driven UI?

**Answer:**
A pattern where the backend controls **what UI to render** via configuration or schema.

Benefits:

* Faster iteration
* Feature flags
* A/B testing

---

## 35. What is the difference between debounce and throttle?

**Answer:**

* **Debounce**: Runs after events stop firing
* **Throttle**: Runs at most once per interval

Used for inputs, scroll, resize, search.

---

## 36. Why should side effects be avoided during render?

**Answer:**
Because render should be:

* Pure
* Predictable
* Idempotent

Side effects can cause infinite loops or inconsistent UI.

---

## 37. What is the difference between optimistic and pessimistic updates?

**Answer:**

* **Optimistic**: Update UI immediately, rollback on failure
* **Pessimistic**: Wait for server response

Optimistic updates improve perceived performance.

---

## 38. What is the difference between `useCallback` and `useMemo`?

**Answer:**

* `useCallback`: Memoizes a function
* `useMemo`: Memoizes a value

Both help prevent unnecessary re-renders.

---

## 39. What is tree shaking?

**Answer:**
Removing unused code from the final bundle during build time.

Works best with:

* ES modules
* Named exports

---

## 40. What is the role of a bundler like Webpack or Vite?

**Answer:**

* Bundle modules
* Transform code
* Optimize assets
* Enable code splitting and HMR

---

## 41. How do you handle authentication in frontend apps?

**Answer:**

* Store tokens securely (httpOnly cookies preferred)
* Handle refresh tokens
* Protect routes
* Sync auth state across tabs

---

## 42. What are race conditions in frontend apps?

**Answer:**
When multiple async operations resolve out of order, causing stale or incorrect UI.

Solutions:

* Cancellation
* Request deduplication
* Latest-only logic

---

## 43. What is a “headless” component?

**Answer:**
A component that provides logic but no UI.

Benefits:

* Reusability
* Custom design control

---

## 44. What is accessibility (a11y) and why does it matter?

**Answer:**
Making apps usable for everyone, including assistive technologies.

Examples:

* Keyboard navigation
* ARIA labels
* Semantic HTML

---

## 45. How do you architect a frontend for a large team?

**Answer:**

* Clear folder structure
* Design system
* Linting and conventions
* Ownership boundaries
* Documentation

---

## 46. What is micro-frontend architecture?

**Answer:**
Breaking a frontend into independently deployed pieces.

Pros:

* Team autonomy
  Cons:
* Complexity
* Performance challenges

---

## 47. What is hydration mismatch?

**Answer:**
When server-rendered HTML doesn’t match client-rendered output.

Common causes:

* Random values
* Date/time differences
* Browser-only APIs

---

## 48. What is the difference between BFF and direct API calls?

**Answer:**

* **BFF (Backend for Frontend)**: Tailored API for UI needs
* **Direct API**: Shared backend endpoints

BFF improves performance and simplifies frontend logic.

---

## 49. How do you measure frontend performance?

**Answer:**

* Lighthouse
* Web Vitals
* React Profiler
* Real User Monitoring

---

## 50. What separates a mid-level from a senior frontend engineer?

**Answer:**

* Architectural thinking
* Tradeoff awareness
* Performance and DX focus
* Ability to simplify complex systems
* Mentoring others

---

## 51. Why must hooks be called unconditionally and in the same order?

**Answer:**
React relies on **call order** to associate hook state with components.
Calling hooks conditionally breaks this order and causes bugs.

---

## 52. What happens if you update state during render?

**Answer:**
It causes an infinite render loop.
State updates must happen in effects or event handlers.

---

## 53. What is a stale closure in React?

**Answer:**
When a function “captures” old state values due to JavaScript closures.

Common fix:

* Correct dependency arrays
* Functional state updates

---

## 54. Why are dependency arrays important in `useEffect`?

**Answer:**
They tell React **when the effect should re-run**.
Missing dependencies can cause stale data or inconsistent behavior.

---

## 55. What is the difference between functional and direct state updates?

**Answer:**

```js
setCount(count + 1)       // direct (can be stale)
setCount(prev => prev+1) // functional (safe)
```

Functional updates are safer for async or repeated updates.

---

## 56. What is Strict Mode and why does it double-invoke effects?

**Answer:**
Strict Mode intentionally re-runs renders and effects in development to:

* Detect unsafe side effects
* Surface hidden bugs

It does not affect production.

---

## 57. What is concurrent rendering?

**Answer:**
React’s ability to **pause, resume, or abandon renders** to keep the UI responsive.

Introduced with React 18.

---

## 58. What problem does `useTransition` solve?

**Answer:**
It marks updates as **non-urgent**, allowing React to prioritize urgent UI updates.

Used for:

* Filtering
* Searching
* Large list updates

---

## 59. What is `useDeferredValue`?

**Answer:**
It delays updating a value so the UI stays responsive while heavy renders happen.

---

## 60. What is the difference between layout thrashing and batching?

**Answer:**

* **Layout thrashing**: Repeated DOM reads/writes causing reflows
* **Batching**: Grouping updates to reduce renders

React automatically batches updates.

---

## 61. Why should effects clean up after themselves?

**Answer:**
To prevent:

* Memory leaks
* Duplicate subscriptions
* Unexpected behavior

Cleanup runs on unmount or before re-running the effect.

---

## 62. What is ref forwarding?

**Answer:**
Passing a ref through a component to a child DOM element.

Used in:

* Component libraries
* Focus management

---

## 63. What problem do portals solve?

**Answer:**
Rendering components outside the DOM hierarchy while keeping React context.

Used for:

* Modals
* Tooltips
* Dropdowns

---

## 64. What is event delegation in React?

**Answer:**
React attaches event listeners at the root and handles events centrally for performance.

---

## 65. Why are inline functions in JSX sometimes problematic?

**Answer:**
They create a new function on every render, potentially causing unnecessary re-renders.

---

## 66. What is the difference between `memo` and `PureComponent`?

**Answer:**

* `memo`: Functional components
* `PureComponent`: Class components

Both use shallow comparison.

---

## 67. How do you safely access browser-only APIs in SSR apps?

**Answer:**

* Check for `window`
* Use `useEffect`
* Dynamic imports with `ssr: false`

---

## 68. What is render-as-you-fetch?

**Answer:**
A data-fetching pattern where requests start before rendering to reduce waterfalls.

---

## 69. How do you avoid waterfall requests?

**Answer:**

* Parallel fetching
* Prefetching
* Server-side data loading

---

## 70. What is the biggest mistake engineers make with hooks?

**Answer:**
Treating hooks as magic instead of understanding:

* Closures
* Render cycles
* Dependency tracking

---

## React Fiber & Internals – Questions + Answers

---

## 71. What is React Fiber?

**Answer:**
React Fiber is React’s **reconciliation engine** introduced in React 16.

Its goals:

* Incremental rendering
* Prioritization of updates
* Pause, resume, and abort work
* Enable concurrency features

Fiber is **not the public API**, but the internal architecture.

---

## 72. Why was Fiber introduced?

**Answer:**
The old stack reconciler was **synchronous and blocking**.

Fiber allows:

* Breaking work into small units
* Scheduling work based on priority
* Keeping the UI responsive during heavy renders

---

## 73. What is a Fiber node?

**Answer:**
A Fiber node is a JavaScript object representing:

* A component instance
* Its state, props, hooks
* Its relationship to other components

Each React element maps to a Fiber node.

---

## 74. What information does a Fiber node store?

**Answer:**
A Fiber node contains:

* Type (function, class, host)
* Props and state
* Effect flags
* Pointers to parent, child, sibling
* Alternate (previous version)

---

## 75. What is the “alternate” field in Fiber?

**Answer:**
It points to the **previous Fiber tree**.

React uses:

* Current tree (committed UI)
* Work-in-progress tree (next render)

This enables **double buffering**.

---

## 76. What are the phases of React rendering?

**Answer:**

1. **Render (Reconciliation) Phase**

   * Build Fiber tree
   * Can be paused or restarted
   * No DOM mutations

2. **Commit Phase**

   * Apply DOM updates
   * Run effects
   * Cannot be interrupted

---

## 77. Why is the render phase interruptible but commit is not?

**Answer:**
Interrupting DOM mutations would cause inconsistent UI.

So React:

* Prepares work incrementally
* Commits changes atomically

---

## 78. What is cooperative scheduling?

**Answer:**
React yields control back to the browser between Fiber units of work to:

* Process user input
* Paint frames
* Avoid blocking the main thread

---

## 79. What is a “unit of work” in Fiber?

**Answer:**
The smallest piece of work React performs, usually:

* Processing a single Fiber node
* Running its render logic

---

## 80. How does Fiber enable concurrency?

**Answer:**
By:

* Splitting rendering into units
* Assigning priorities (lanes)
* Allowing React to pause low-priority work

Concurrency is about **responsiveness**, not parallel threads.

---

## 81. What are lanes in React Fiber?

**Answer:**
Lanes are **priority levels** for updates.

Examples:

* User input (high priority)
* Transitions (low priority)
* Background updates

They replaced the older expiration time model.

---

## 82. How does React decide update priority?

**Answer:**
Based on:

* Event type (click vs timeout)
* `startTransition`
* Scheduler heuristics

Urgent updates preempt non-urgent ones.

---

## 83. What is time slicing?

**Answer:**
Breaking rendering work into chunks so React can:

* Yield to the browser
* Keep animations and input responsive

---

## 84. What happens when React “yields”?

**Answer:**
React:

* Pauses work
* Lets the browser handle high-priority tasks
* Resumes rendering later

---

## 85. How does Fiber affect hooks?

**Answer:**
Hooks are stored as a **linked list on the Fiber node**.

The strict call order rule exists because:

* Hooks are matched by position, not name

---

## 86. Why do hooks break when called conditionally?

**Answer:**
Because React walks the hooks list **in order**.
Changing order means React reads the wrong state.

---

## 87. What are effect tags / flags in Fiber?

**Answer:**
They describe what needs to happen during commit:

* Placement
* Update
* Deletion
* Passive effects

---

## 88. When do `useEffect` and `useLayoutEffect` run internally?

**Answer:**

* `useLayoutEffect`: After DOM mutations, before paint
* `useEffect`: After paint, asynchronously

---

## 89. What is hydration from a Fiber perspective?

**Answer:**
Fiber attaches to existing DOM nodes instead of creating new ones.

It matches:

* Fiber nodes ↔ existing DOM

Mismatch causes re-rendering.

---

## 90. Why does Strict Mode double-render components?

**Answer:**
To simulate mounting, unmounting, and remounting:

* Exposes unsafe side effects
* Validates effect cleanup logic

---

## 91. Can Fiber rendering be aborted?

**Answer:**
Yes, during the render phase.

If higher-priority work arrives, React:

* Abandons the current render
* Restarts later

---

## 92. What is the difference between legacy and concurrent rendering?

**Answer:**

* **Legacy**: Synchronous, blocking
* **Concurrent**: Interruptible, prioritized

React 18 enables concurrent features by default.

---

## 93. Why doesn’t concurrency guarantee faster renders?

**Answer:**
Because it improves **responsiveness**, not raw speed.

Work may actually take longer overall but feels smoother.

---

## 94. What is the biggest misconception about React Fiber?

**Answer:**
That it’s multithreaded.

Fiber runs on the **single main thread** but schedules work intelligently.

---

## 95. When should a developer care about Fiber internals?

**Answer:**
When dealing with:

* Performance bottlenecks
* Large trees
* Concurrent features
* Complex rendering behavior

Most apps don’t need direct knowledge, but understanding it improves decisions.


---

## Class-Based Components – Advanced Questions + Answers

---

## 96. What is a class-based component in React?

**Answer:**
A class-based component is a JavaScript class that:

* Extends `React.Component`
* Implements a `render()` method
* Manages state and lifecycle via methods

---

## 97. What problem did class components originally solve?

**Answer:**
They introduced:

* Local state
* Lifecycle methods
* Encapsulation of logic and UI

Before hooks, they were the only way to do this.

---

## 98. What are the main lifecycle phases of a class component?

**Answer:**

1. **Mounting**
2. **Updating**
3. **Unmounting**
4. **Error handling**

---

## 99. What lifecycle methods run during mounting?

**Answer:**

* `constructor`
* `static getDerivedStateFromProps`
* `render`
* `componentDidMount`

---

## 100. What is the purpose of `constructor`?

**Answer:**
Used to:

* Initialize state
* Bind event handlers

Should not contain side effects.

---

## 101. Why should side effects not be placed in `render()`?

**Answer:**
`render()` must be:

* Pure
* Free of side effects

Because it can run multiple times and be aborted.

---

## 102. What is `componentDidMount` used for?

**Answer:**

* Data fetching
* Subscriptions
* DOM measurements

It runs once after the component is inserted into the DOM.

---

## 103. What lifecycle methods run during updates?

**Answer:**

* `static getDerivedStateFromProps`
* `shouldComponentUpdate`
* `render`
* `getSnapshotBeforeUpdate`
* `componentDidUpdate`

---

## 104. What is `shouldComponentUpdate`?

**Answer:**
Allows preventing re-renders by returning `false`.

Used for performance optimization.

---

## 105. What is `PureComponent`?

**Answer:**
A base class that implements `shouldComponentUpdate` with shallow prop/state comparison.

---

## 106. What are the dangers of shallow comparison?

**Answer:**

* Mutated objects won’t trigger updates
* Nested changes may be missed

Requires immutability.

---

## 107. What is `getSnapshotBeforeUpdate`?

**Answer:**
Captures information (e.g. scroll position) **before DOM updates**, passed to `componentDidUpdate`.

---

## 108. When does `componentDidUpdate` run?

**Answer:**
After re-render and DOM updates.

Used for:

* Responding to prop/state changes
* Side effects

---

## 109. How do you prevent infinite loops in `componentDidUpdate`?

**Answer:**
By comparing previous and current props/state before calling `setState`.

---

## 110. What lifecycle method runs during unmounting?

**Answer:**
`componentWillUnmount`

Used for:

* Cleanup
* Cancelling subscriptions
* Clearing timers

---

## 111. What are error boundaries and which lifecycle methods do they use?

**Answer:**
Class components that catch rendering errors.

They use:

* `static getDerivedStateFromError`
* `componentDidCatch`

---

## 112. Why can’t hooks replace error boundaries?

**Answer:**
Because error boundaries rely on lifecycle methods that hooks cannot implement.

---

## 113. What were the “unsafe” lifecycle methods and why were they deprecated?

**Answer:**

* `componentWillMount`
* `componentWillReceiveProps`
* `componentWillUpdate`

They were unsafe for async and concurrent rendering.

---

## 114. How do class lifecycle methods map to hooks?

**Answer:**

* `componentDidMount` → `useEffect(() => {}, [])`
* `componentDidUpdate` → `useEffect` with deps
* `componentWillUnmount` → cleanup function

---

## 115. What problems did class components have?

**Answer:**

* Logic reuse was difficult
* `this` binding issues
* Lifecycle complexity
* Hard to reason about related logic

---

## 116. What is “this” binding and why was it painful?

**Answer:**
Methods lose context when passed as callbacks.

Required:

* Manual binding
* Arrow functions

---

## 117. Why do hooks lead to better logic reuse than classes?

**Answer:**
Hooks allow:

* Extracting logic without changing component hierarchy
* Composing behavior cleanly

Classes relied on HOCs and render props.

---

## 118. How do class components interact with Fiber?

**Answer:**
Each class instance is associated with a Fiber node.

Fiber manages:

* Scheduling
* Updates
* Lifecycle invocation

---

## 119. Are class components slower than function components?

**Answer:**
Not inherently.

Performance differences usually come from:

* Architecture
* Re-renders
* State management

---

## 120. When should class components still be used?

**Answer:**

* Error boundaries
* Legacy codebases
* Libraries that predate hooks

---
