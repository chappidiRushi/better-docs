---

id: hooks
title: Hooks
sidebar_position: 5
-------------------

# Lifecycle & Effects

> A clear, concise guide to **class component lifecycles** and their **hook-based equivalents** in function components.

---

## Mental Model First

* **Class components** use *lifecycle methods* (explicit phases).
* **Function components** use *hooks* to express the same ideas.
* Rendering is **pure**; side effects run **after commit**.

---

## Class Component Lifecycle (Execution Order)

### Mounting (component is created)

| Method                     | Simple Definition                           |
| -------------------------- | ------------------------------------------- |
| `constructor`              | Initialize state and bind methods           |
| `getDerivedStateFromProps` | Sync state with props (rare)                |
| `render`                   | Describe UI                                 |
| `componentDidMount`        | Run side effects (API calls, subscriptions) |

<details>
<summary>Example: Mounting lifecycle</summary>

```js
class MountDemo extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  componentDidMount() {
    console.log('Component mounted');
  }

  render() {
    return <p>Count: {this.state.count}</p>;
  }
}
```

</details>

------|------------------|
| `constructor` | Initialize state and bind methods |
| `getDerivedStateFromProps` | Sync state with props (rare) |
| `render` | Describe UI |
| `componentDidMount` | Run side effects (API calls, subscriptions) |

---

### Updating (props or state change)

| Method                     | Simple Definition                          |
| -------------------------- | ------------------------------------------ |
| `getDerivedStateFromProps` | Update state from new props                |
| `shouldComponentUpdate`    | Decide whether to re-render (optimization) |
| `render`                   | Describe updated UI                        |
| `getSnapshotBeforeUpdate`  | Read DOM before update                     |
| `componentDidUpdate`       | Run side effects after update              |

<details>
<summary>Example: Updating lifecycle</summary>

```js
class UpdateDemo extends React.Component {
  componentDidUpdate(prevProps) {
    if (prevProps.value !== this.props.value) {
      console.log('Value changed');
    }
  }

  render() {
    return <p>Value: {this.props.value}</p>;
  }
}
```

</details>

------|------------------|
| `getDerivedStateFromProps` | Update state from new props |
| `shouldComponentUpdate` | Decide whether to re-render (optimization) |
| `render` | Describe updated UI |
| `getSnapshotBeforeUpdate` | Read DOM before update |
| `componentDidUpdate` | Run side effects after update |

---

### Unmounting

| Method                 | Simple Definition                          |
| ---------------------- | ------------------------------------------ |
| `componentWillUnmount` | Cleanup (timers, listeners, subscriptions) |

<details>
<summary>Example: Unmounting cleanup</summary>

```js
class UnmountDemo extends React.Component {
  componentDidMount() {
    this.id = setInterval(() => console.log('tick'), 1000);
  }

  componentWillUnmount() {
    clearInterval(this.id);
    console.log('Cleaned up');
  }

  render() {
    return <p>Running...</p>;
  }
}
```

</details>

------|------------------|
| `componentWillUnmount` | Cleanup (timers, listeners, subscriptions) |

---

### Concurrent & Modern React Hooks

| Hook               | Simple Definition                     |
| ------------------ | ------------------------------------- |
| `useTransition`    | Mark updates as non-urgent            |
| `useDeferredValue` | Defer expensive re-renders            |
| `useOptimistic`    | Optimistic UI updates (React 19+)     |
| `useActionState`   | Manage async action state (React 19+) |

---

## Error Handling

| Method                     | Simple Definition     |
| -------------------------- | --------------------- |
| `getDerivedStateFromError` | Update UI after error |
| `componentDidCatch`        | Log error / report    |

<details>
<summary>Example: Error Boundary</summary>

```js
class ErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError() {
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    console.error(error, info);
  }

  render() {
    if (this.state.hasError) return <h1>Something went wrong</h1>;
    return this.props.children;
  }
}
```

</details>

------|------------------|
| `getDerivedStateFromError` | Update UI after error |
| `componentDidCatch` | Log error / report |

---

## Function Component Hooks (What You’ll Actually Use)

### Core Hooks

| Hook              | Simple Definition             |
| ----------------- | ----------------------------- |
| `useState`        | Local component state         |
| `useEffect`       | Run side effects after render |
| `useLayoutEffect` | Run effects *before paint*    |
| `useReducer`      | State logic with actions      |
| `useRef`          | Persist values or access DOM  |

<details>
<summary>Example: <code>useState</code></summary>

```js
const [count, setCount] = useState(0);
<button onClick={() => setCount(c => c + 1)} />
```

</details>

<details>
<summary>Example: <code>useEffect</code> (side effects + cleanup)</summary>

```js
useEffect(() => {
  const id = setInterval(() => console.log('tick'), 1000);
  return () => clearInterval(id);
}, []);
```

</details>

<details>
<summary>Example: <code>useLayoutEffect</code></summary>

```js
useLayoutEffect(() => {
  const { height } = ref.current.getBoundingClientRect();
  setHeight(height);
}, []);
```

</details>

<details>
<summary>Example: <code>useRef</code></summary>

```js
const inputRef = useRef(null);
inputRef.current.focus();
```

</details>

<details>
<summary>Example: <code>useReducer</code></summary>

```js
function reducer(state, action) {
  if (action.type === 'inc') return state + 1;
  return state;
}
const [count, dispatch] = useReducer(reducer, 0);
```

</details>

---

### Performance & Composition

| Hook          | Simple Definition            |
| ------------- | ---------------------------- |
| `useMemo`     | Cache expensive calculations |
| `useCallback` | Cache function references    |
| `useContext`  | Read context values          |

<details>
<summary>Example: <code>useMemo</code></summary>

```js
const total = useMemo(() => heavyCalc(items), [items]);
```

</details>

<details>
<summary>Example: <code>useCallback</code></summary>

```js
const onClick = useCallback(() => setOpen(true), []);
```

</details>

<details>
<summary>Example: <code>useContext</code></summary>

```js
const theme = useContext(ThemeContext);
```

</details>

---

### Concurrent & Modern Hooks

| Hook               | Simple Definition                     |
| ------------------ | ------------------------------------- |
| `useTransition`    | Mark updates as non-urgent            |
| `useDeferredValue` | Defer expensive re-renders            |
| `useOptimistic`    | Optimistic UI updates (React 19+)     |
| `useActionState`   | Manage async action state (React 19+) |

<details>
<summary>Example: <code>useTransition</code></summary>

```js
const [isPending, startTransition] = useTransition();
startTransition(() => setList(filter(items)));
```

</details>

<details>
<summary>Example: <code>useDeferredValue</code></summary>

```js
const deferredQuery = useDeferredValue(query);
```

</details>

<details>
<summary>Example: <code>useOptimistic</code></summary>

```js
const [state, setOptimistic] = useOptimistic(data);
setOptimistic(d => [...d, newItem]);
```

</details>

<details>
<summary>Example: <code>useActionState</code></summary>

```js
const [state, action] = useActionState(saveUser, initialState);
```

</details>

---

### Advanced / Rare Hooks

| Hook                   | Simple Definition                   |
| ---------------------- | ----------------------------------- |
| `useImperativeHandle`  | Expose custom ref API               |
| `useDebugValue`        | Show debug info in DevTools         |
| `useInsertionEffect`   | Inject styles before DOM mutations  |
| `useSyncExternalStore` | Subscribe to external stores safely |
| `useId`                | Generate stable unique IDs          |

<details>
<summary>Example: <code>useSyncExternalStore</code></summary>

```js
const value = useSyncExternalStore(store.subscribe, store.getSnapshot);
```

</details>

<details>
<summary>Example: <code>useId</code></summary>

```js
const id = useId();
<label htmlFor={id} />
<input id={id} />
```

</details>

<details>
<summary>Example: <code>useInsertionEffect</code></summary>

```js
useInsertionEffect(() => {
  injectStyles();
}, []);
```

</details>

---

## Key Takeaways

* Lifecycles and hooks solve the **same problems**

* Hooks are more composable and easier to reuse

* `useEffect` replaces most lifecycle methods

* Rendering is pure; effects run **after commit**

* Lifecycles and hooks solve the **same problems**

* Hooks are more composable and easier to reuse

* `useEffect` replaces most lifecycle methods

* Rendering is pure; effects run **after commit**
