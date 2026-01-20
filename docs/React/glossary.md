---
id: glossary
title: Glossary
sidebar_position: 1
-------------------
# React Glossary (Interview‑Friendly)

---

* **React** is a JavaScript library for building component‑based user interfaces using declarative rendering.

* **JSX** is a syntax extension that lets you write UI markup directly inside JavaScript.

* **Element** is an immutable description of what the UI should look like at a point in time.

* **Component** is a reusable function or class that returns React elements.

* **Functional Component** is a JavaScript function that renders UI and uses hooks for state and lifecycle.

* **Class Component** is a component defined using ES6 classes with lifecycle methods.

* **Props** are read‑only inputs passed from parent components to child components.

* **State** is mutable data owned by a component that triggers a re‑render when it changes.

* **Local State** is state that belongs to a single component.

* **Global State** is state shared across multiple parts of an application.

* **Derived State** is state computed from props or other state values.

* **Controlled Component** is a form element whose value is controlled by React state.

* **Uncontrolled Component** is a form element that manages its own state in the DOM.

* **One‑Way Data Flow** means data flows from parent components down to children.

* **Declarative UI** describes what the UI should look like instead of how to update it.

* **Imperative Code** explicitly controls DOM updates or logic step by step.

* **Reconciliation** is the process React uses to compare previous and next UI trees to determine updates.

* **Virtual DOM** is an in‑memory representation of the real DOM used for efficient updates.

* **Diffing Algorithm** is the heuristic React uses to minimize DOM mutations.

* **Key** is a stable identifier that helps React track list items between renders.

* **Index as Key** is using array indexes as keys, which can cause rendering bugs.

* **Render** is the phase where React calculates what the UI should look like.

* **Re‑Render** is when a component executes again due to state or prop changes.

* **Commit Phase** is when React applies calculated changes to the DOM.

* **Render Cycle** is the full process of render and commit phases.

* **Mounting** is when a component is inserted into the DOM.

* **Updating** is when a component re‑renders due to changes.

* **Unmounting** is when a component is removed from the DOM.

* **Side Effect** is any operation that affects something outside the render output.

* **Hook** is a function that lets components use React features like state and lifecycle.

* **Rules of Hooks** are constraints that ensure hooks run in a consistent order.

* **Custom Hook** is a reusable function that encapsulates shared hook logic.

* **useState** is a React Hook that lets you add local state to a functional component.

* **useEffect** is a React Hook that lets you run side effects after rendering.

* **Effect Cleanup** is a function returned from an effect to clean up resources.

* **Dependency Array** controls when an effect or memoized value re‑runs.

* **useLayoutEffect** is a React Hook that runs side effects synchronously after DOM mutations.

* **useInsertionEffect** is a React Hook for injecting styles before layout effects run.

* **useRef** is a React Hook that lets you persist mutable values across renders without re‑rendering.

* **DOM Ref** is a reference to a DOM element managed by React.

* **Callback Ref** is a function that receives a DOM node reference.

* **Forward Ref** allows passing refs through components to child elements.

* **useImperativeHandle** customizes the instance value exposed by a ref.

* **useMemo** is a React Hook that lets you cache a computed value between re‑renders.

* **useCallback** is a React Hook that lets you cache a function definition between re‑renders.

* **useContext** is a React Hook that lets you read and subscribe to context values.

* **Context** is a mechanism for sharing data without passing props through every level.

* **Provider** is a component that supplies a context value to its descendant components.

* **Consumer** is a component that reads values from React context.

* **Prop Drilling** is the practice of passing props through multiple intermediate components.

* **State Lifting** means moving shared state to the closest common ancestor.

* **Memoization** is a performance technique used to cache results and avoid repeated computation.

* **React.memo** is a higher‑order component that prevents re‑renders when props have not changed.

* **Pure Component** renders the same output for the same props and state.

* **Shallow Comparison** compares values by reference at the top level.

* **Referential Equality** means two variables point to the same object reference.

* **Immutability** is the practice of replacing state instead of mutating existing values.

* **Batching** groups multiple state updates into a single render.

* **Automatic Batching** batches updates across events, promises, and effects.

* **Synthetic Event** is a cross‑browser wrapper around native browser events.

* **Event Delegation** attaches events at the root instead of individual nodes.

* **Children** is a special prop that represents nested JSX passed into a component.

* **Composition** is the practice of building complex UIs from smaller components.

* **Render Props** is a pattern where a function prop controls rendering logic.

* **Higher‑Order Component** is a function that enhances a component by wrapping it.

* **Fragment** lets you group elements without adding extra nodes to the DOM.

* **Portal** lets you render children into a DOM node outside the parent hierarchy.

* **Error Boundary** is a component that catches JavaScript errors during rendering.

* **Suspense** lets you delay rendering while waiting for asynchronous data or code.

* **Suspense Boundary** defines where loading fallback UI is shown.

* **Fallback UI** is the UI displayed while content is loading.

* **Lazy Loading** is a technique that loads components only when they are needed.

* **Code Splitting** breaks a bundle into smaller chunks for better performance.

* **Bundle** is the compiled JavaScript shipped to the browser.

* **Tree Shaking** removes unused code during bundling.

* **Concurrent Rendering** allows React to interrupt and prioritize rendering work.

* **Fiber** is React’s internal architecture that enables incremental rendering.

* **Time Slicing** splits rendering work into interruptible chunks.

* **Scheduler** manages task priority inside React.

* **Transition** marks state updates as non‑urgent.

* **useTransition** lets you defer non‑urgent state updates.

* **Pending State** indicates a transition is in progress.

* **useDeferredValue** defers updating a value to keep the UI responsive.

* **Blocking Render** is rendering that prevents user interaction.

* **Non‑Blocking Render** allows rendering to be interrupted.

* **Strict Mode** is a development‑only tool that highlights potential problems in an app.

* **Double Rendering** is intentional re‑rendering in development to detect bugs.

* **Idempotent Render** ensures rendering produces no side effects.

* **Hydration** attaches React behavior to server‑rendered HTML.

* **Server‑Side Rendering** renders React components on the server.

* **Static Site Generation** pre‑renders pages at build time.

* **Client‑Side Rendering** renders the UI entirely in the browser.

* **Streaming SSR** sends HTML to the client incrementally.

* **Selective Hydration** prioritizes hydration of interactive components.

* **Progressive Hydration** hydrates parts of the UI gradually.

* **Server Components** are components that run only on the server.

* **Client Components** are components that run in the browser.

* **Server Actions** are server‑executed functions triggered from the UI.

* **Data Fetching** retrieves data needed for rendering UI.

* **Waterfall Fetching** performs data requests sequentially.

* **Parallel Fetching** performs data requests concurrently.

* **Caching** stores data to avoid repeated fetching.

* **Revalidation** refreshes cached data when it becomes stale.

* **Optimistic UI** updates the UI before server confirmation.

* **Escape Hatch** is a way to interact with non‑React systems like the DOM.

* **Anti‑Pattern** is a practice that leads to bugs or performance issues in React.

* **Legacy Lifecycle Methods** are deprecated class lifecycle APIs like componentWillMount.

* **componentDidMount** is a class lifecycle method that runs after the component mounts.

* **componentDidUpdate** is a class lifecycle method that runs after updates.

* **componentWillUnmount** is a class lifecycle method used for cleanup.

* **setState** is a class API that schedules a state update.

* **State Updater Function** is a functional form of state update based on previous state.

* **Asynchronous Rendering** allows React to pause and resume rendering work.

* **Priority Update** is an urgent update that React processes immediately.

* **Lane** is an internal priority bucket used by React Fiber.

* **Work Loop** is the internal process that performs rendering tasks.

* **Render Phase** is the pure calculation phase without side effects.

* **Commit Phase Effects** are effects that run after DOM updates.

* **Passive Effects** are effects scheduled by useEffect.

* **Layout Effects** are effects scheduled by useLayoutEffect.

* **Strict Effects Mode** intentionally re-runs effects in development.

* **Bailout** is when React skips rendering a subtree.

* **State Snapshot** is the state value captured during a render.

* **Stale State** is outdated state referenced inside closures.

* **Closure** is a function retaining access to its lexical scope.

* **Ref Stability** means refs remain consistent across renders.

* **Element Identity** determines whether React treats elements as the same.

* **Component Identity** is based on position and key in the tree.

* **Tree Reconciliation** matches elements between renders.

* **DOM Mutation** is a change applied to the real DOM.

* **Layout Thrashing** is repeated layout recalculation causing performance issues.

* **Paint** is the browser step that draws pixels to the screen.

* **Commit Blocking** delays DOM updates.

* **Hydration Mismatch** occurs when server and client HTML differ.

* **Checksum** is used internally to validate hydration.

* **Client Boundary** separates server and client components.

* **Server Boundary** defines server-only execution scope.

* **Flight Protocol** is React’s wire format for Server Components.

* **Serialization** converts server component output for transport.

* **Deserialization** reconstructs UI from serialized output.

* **Cache Boundary** scopes cached server data.

* **Request Memoization** deduplicates fetches per request.

* **Reactivity** is automatic UI updates based on state changes.

* **Signal** is an external reactive primitive sometimes integrated with React.

* **External Store** is state managed outside React.

* **useSyncExternalStore** subscribes React to external stores safely.

* **Snapshot Consistency** ensures external store reads are consistent.

* **Controlled Re-render** limits rendering to necessary updates.

* **Over-Rendering** is rendering more often than required.

* **Throttling** limits how often a function can run.

* **Debouncing** delays function execution until activity stops.

* **Idle Rendering** schedules work during browser idle time.

* **requestIdleCallback** allows low-priority work scheduling.

* **Suspense for Data Fetching** integrates async data into rendering.

* **Promise Throwing** is how Suspense pauses rendering.

* **Error Propagation** bubbles errors to nearest boundary.

* **Retry Rendering** retries rendering after an error or suspend.

* **UI Tearing** is inconsistent UI during concurrent updates.

* **Consistency Guarantees** ensure stable UI output.

* **Hydration Priority** determines which components hydrate first.

* **Interaction Tracing** tracks user interactions through renders.

* **Profiler** measures render performance.

* **React DevTools** is the official debugging tool for React apps.

* **Why Did You Render** detects avoidable re-renders.

* **Strict Equality** compares values using ===.

* **Structural Sharing** reuses unchanged parts of state.

* **Normalized State** stores data in a flat structure.

* **Colocation** places logic near where it’s used.

* **Separation of Concerns** divides logic into distinct responsibilities.

* **UI State** controls visual aspects of the interface.

* **Server State** represents remote data from APIs.

* **Form State** represents user input data.

* **Derived UI** is UI computed from state instead of stored.

* **Anti-Pattern: Derived State** storing computable values in state.

* **Anti-Pattern: Mutating State** directly modifying state objects.

* **Anti-Pattern: Excessive Effects** overusing useEffect.

* **Anti-Pattern: Over-Memoization** memoizing everything unnecessarily.

---

### Caveats

* State updates are asynchronous
* Keys must be stable and predictable
* Effects run after render
* Memoization is a performance optimization, not a guarantee
