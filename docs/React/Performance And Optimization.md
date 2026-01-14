---

id: performance-and-optimization
title: Performance And Optimization
sidebar_position: 7
-------------------

# Performance & Optimization

> A practical guide to **making React apps fast** by avoiding unnecessary work, reducing re-renders, and prioritizing the right updates.

---

## Mental Model

Performance optimization in React is about **reducing work**, not micro‑tweaks.

Ask these questions first:

* Did this component **need to re-render**?
* Did this calculation **need to re-run**?
* Is this update **urgent or deferrable**?
* Is the work happening **during render** or **after commit**?

---

## React.memo

`React.memo` memoizes a **function component** and skips re-rendering when props are **shallowly equal**.

### When to Use

* Pure components
* Same props most of the time
* Expensive child renders

❌ Not useful if props always change.

<details>
<summary>Example: <code>React.memo</code></summary>

```js
const Child = React.memo(function Child({ name }) {
  console.log('Child rendered');
  return <p>Hello {name}</p>;
});

function App() {
  const [count, setCount] = React.useState(0);
  return (
    <>
      <Child name="Alice" />
      <button onClick={() => setCount(c => c + 1)}>{count}</button>
    </>
  );
}
```

</details>

---

## useCallback

`useCallback` memoizes a **function reference**.

### When to Use

* Passing callbacks to memoized children
* Stable dependencies

❌ Not needed for inline handlers unless passed down.

<details>
<summary>Example: <code>useCallback</code></summary>

```js
function Button({ onClick }) {
  console.log('Button rendered');
  return <button onClick={onClick}>Click</button>;
}

const MemoButton = React.memo(Button);

function App() {
  const [count, setCount] = React.useState(0);

  const handleClick = React.useCallback(() => {
    setCount(c => c + 1);
  }, []);

  return <MemoButton onClick={handleClick} />;
}
```

</details>

---

## useMemo

`useMemo` memoizes the **result of a computation**.

### When to Use

* Expensive calculations
* Derived data

❌ Don’t use for cheap calculations.

<details>
<summary>Example: <code>useMemo</code></summary>

```js
function App({ numbers }) {
  const [multiplier, setMultiplier] = React.useState(2);

  const doubled = React.useMemo(() => {
    return numbers.map(n => n * multiplier);
  }, [numbers, multiplier]);

  return <p>{doubled.join(', ')}</p>;
}
```

</details>

---

## Concurrent Performance Hooks (Often Missed)

### useTransition

Marks updates as **non-urgent**, allowing React to keep the UI responsive.

<details>
<summary>Example: <code>useTransition</code></summary>

```js
const [isPending, startTransition] = React.useTransition();

startTransition(() => {
  setList(filterItems(input));
});
```

</details>

---

### useDeferredValue

Defers re-rendering of **derived values**.

<details>
<summary>Example: <code>useDeferredValue</code></summary>

```js
const deferredQuery = React.useDeferredValue(query);
```

</details>

---

## Code Splitting & Lazy Loading

Reduce initial load time by splitting bundles.

| API          | Purpose                     |
| ------------ | --------------------------- |
| `React.lazy` | Lazy-load components        |
| `Suspense`   | Show fallback while loading |

<details>
<summary>Example: Lazy Loading</summary>

```js
const Dashboard = React.lazy(() => import('./Dashboard'));

<Suspense fallback={<Spinner />}>
  <Dashboard />
</Suspense>
```

</details>

---

## List Rendering Optimization

* Always use **stable keys**
* Avoid array index as key
* Memoize list items when large

```js
items.map(item => <Row key={item.id} item={item} />)
```

---

## Avoiding Expensive Work in Render

❌ Bad:

```js
const value = heavyCalculation(props);
```

✅ Good:

```js
const value = useMemo(() => heavyCalculation(props), [props]);
```

---

## Batching & Automatic Optimizations

React automatically batches updates in:

* Event handlers
* Timeouts
* Promises

This reduces unnecessary renders.

---

## Measuring Performance (Critical)

### React DevTools Profiler

Use it to:

* Identify slow components
* Detect wasted renders
* Verify optimizations

### `Profiler` API

```js
<Profiler id="App" onRender={callback}>
  <App />
</Profiler>
```

---

## Common Mistakes

* Using `useMemo` / `useCallback` everywhere
* Memoizing components with unstable props
* Optimizing before profiling
* Ignoring list keys
* Doing heavy work during render

---

## Performance Best Practices (Summary)

| Technique           | Why It Helps             |
| ------------------- | ------------------------ |
| Memoization         | Avoids unnecessary work  |
| Component splitting | Limits re-render scope   |
| Lazy loading        | Reduces initial bundle   |
| Concurrent hooks    | Keeps UI responsive      |
| Profiling           | Guides real optimization |

---

## Key Takeaways

* Optimize **only when needed**
* Measure before and after
* Reduce work before memoizing
* Concurrent React changes *when* work happens
* Performance is about **architecture**, not tricks

---

## Advanced Optimizations (Less Common, High Impact)

### useId (Hydration & Lists)

Generates stable, unique IDs that work across server and client.

Useful for:

* Accessibility attributes
* Avoiding hydration mismatches

```js
const id = React.useId();
<label htmlFor={id}>Name</label>
<input id={id} />
```

---

### useSyncExternalStore

Safely subscribe to **external stores** (Redux, Zustand, custom stores) in concurrent React.

Why it matters:

* Prevents tearing
* Works correctly with concurrent rendering

```js
const state = useSyncExternalStore(store.subscribe, store.getSnapshot);
```

---

## Server & Rendering Optimizations

### Server Components (Conceptual)

Server Components:

* Run only on the server
* Reduce bundle size
* Avoid sending unnecessary JS

Key idea:

> Move non-interactive logic to the server.

---

### Streaming & Suspense on the Server

Suspense enables:

* Progressive rendering
* Faster TTFB
* Better perceived performance

---

## Rendering Behavior Clarifications

### What Causes a Re-render

A component re-renders when:

* Its state changes
* Its props change
* Its parent re-renders
* Its context value changes

Re-render ≠ DOM update.

---

### Render vs Commit Cost

* **Render phase**: JS execution, can be interrupted
* **Commit phase**: DOM mutations, blocking

Optimize render cost first.

---

## Large Lists & Virtualization

For very large lists:

* Use windowing libraries (`react-window`, `react-virtualized`)
* Render only visible rows

This is often a bigger win than memoization.

---

## Final Optimization Checklist

Before optimizing, ask:

1. Is this component rendering too often?
2. Is the work expensive?
3. Can I move work out of render?
4. Can I defer or split the work?
5. Have I measured the problem?

---

## Final Takeaway

> The fastest React code is the code that never runs.

Optimize by:

* Reducing work
* Deferring work
* Measuring results
