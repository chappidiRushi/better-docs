---

id: hooks
title: Hooks
sidebar_position: 5
-------------------

# React Lifecycles & Hooks

> A clear, concise guide to **class component lifecycles** and their **hook-based equivalents** in function components — organized by mental model and execution order.

---

## Mental Model

### Class Components

* Explicit **lifecycle phases** (mount, update, unmount)
* Logic split across multiple methods
* Harder to reuse lifecycle logic

### Function Components (Hooks)

* No lifecycle methods
* Use **hooks** to express intent
* Logic is grouped by concern, not phase

> **Key rule:** Rendering is **pure**. Side effects run **after commit**.

---

# Part 1: Class Components (Lifecycle-Based)

## 1. Mounting Phase

Executed once when the component is created.

| Method                     | Purpose                        |
| -------------------------- | ------------------------------ |
| `constructor`              | Initialize state, bind methods |
| `getDerivedStateFromProps` | Sync state with props (rare)   |
| `render`                   | Describe UI                    |
| `componentDidMount`        | Run side effects               |

<details>
<summary>Example: Mounting</summary>

```js
class MountDemo extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  componentDidMount() {
    console.log('Mounted');
  }

  render() {
    return <p>Count: {this.state.count}</p>;
  }
}
```

</details>

---

## 2. Updating Phase

Triggered by **state** or **props** changes.

| Method                     | Purpose                |
| -------------------------- | ---------------------- |
| `getDerivedStateFromProps` | Sync state with props  |
| `shouldComponentUpdate`    | Skip re-render (perf)  |
| `render`                   | Describe updated UI    |
| `getSnapshotBeforeUpdate`  | Read DOM before change |
| `componentDidUpdate`       | Run side effects       |

<details>
<summary>Example: Updating</summary>

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

---

## 3. Unmounting Phase

Executed once before the component is removed.

| Method                 | Purpose              |
| ---------------------- | -------------------- |
| `componentWillUnmount` | Cleanup side effects |

<details>
<summary>Example: Cleanup</summary>

```js
class UnmountDemo extends React.Component {
  componentDidMount() {
    this.id = setInterval(() => console.log('tick'), 1000);
  }

  componentWillUnmount() {
    clearInterval(this.id);
  }

  render() {
    return <p>Running...</p>;
  }
}
```

</details>

---

## 4. Error Handling (Classes Only)

| Method                     | Purpose          |
| -------------------------- | ---------------- |
| `getDerivedStateFromError` | Update UI        |
| `componentDidCatch`        | Log/report error |

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

---

# Part 2: Function Components (Hook-Based)

Hooks let you express **what you want to do** (state, effects, memoization) instead of **when** in the lifecycle.

---

## 1. Core Hooks (Most Common)

### `useState`

**Purpose:** Manage local, isolated component state.

**Key behaviors:**

* State updates are async & batched
* Re-render happens on state change
* Initial state runs only on first render

<details>
<summary>Example: Basic state</summary>

```js
const [count, setCount] = useState(0);

<button onClick={() => setCount(c => c + 1)}>
  Increment
</button>
```

</details>

<details>
<summary>Caveats</summary>

* Never mutate state directly
* Use functional updates when based on previous state

</details>

---

### `useEffect`

**useEffect** is a React Hook that lets you synchronize a component with an external system.

**Replaces:**

* `componentDidMount`
* `componentDidUpdate`
* `componentWillUnmount`

<details>
<summary>Example: Data fetching + cleanup</summary>

```js
function Users() {
  const [users, setUsers] = useState([]);
  const [error, setError] = useState(null);

  useEffect(() => {
    // Create an AbortController so we can cancel the request
    const controller = new AbortController();

    async function fetchUsers() {
      try {
        const res = await fetch('/api/users', {
          signal: controller.signal,
        });

        if (!res.ok) throw new Error('Request failed');

        const data = await res.json();
        setUsers(data); // Safe: updates state after fetch resolves
      } catch (err) {
        // Ignore abort errors, handle real ones
        if (err.name !== 'AbortError') {
          setError(err.message);
        }
      }
    }

    fetchUsers();

    // Cleanup runs on unmount OR before next effect
    return () => {
      controller.abort(); // Prevents setting state after unmount
    };
  }, []); // Empty deps → run once on mount

  if (error) return <p>Error: {error}</p>;
  return users.map(u => <div key={u.id}>{u.name}</div>);
}
```

</details>

<details>
<summary>Bad Example</summary>

```js
function BadExample({ userId }) {
  const [user, setUser] = useState(null);

  // ❌ Missing dependency → stale userId
  useEffect(() => {
    fetch(`/api/users/${userId}`)
      .then(r => r.json())
      .then(setUser);
  }, []); // userId should be in deps

  // ❌ Derived state in effect
  const [fullName, setFullName] = useState('');
  useEffect(() => {
    if (user) {
      // This should be derived during render instead
      setFullName(user.first + ' ' + user.last);
    }
  }, [user]);

  // ❌ Effect doing synchronous work
  useEffect(() => {
    console.log('This does not need useEffect');
  });

  return <div>{fullName}</div>;
}
```

</details>

<details>
<summary>Caveats</summary>

* Effects run after paint
* Missing dependencies cause stale values
* Avoid putting derived state in effects

</details>

---
### `useLayoutEffect`

**useLayoutEffect** is a React Hook that lets you run effects **synchronously after DOM mutations but before the browser paints**.

<details>
<summary>Example: Measuring layout safely</summary>

```js
function ExpandablePanel({ children }) {
  const ref = useRef(null);
  const [height, setHeight] = useState(0);

  useLayoutEffect(() => {
    // Runs after DOM is updated but before paint
    // Safe for reading layout and synchronously updating state
    if (ref.current) {
      const rect = ref.current.getBoundingClientRect();
      setHeight(rect.height);
    }
  }, [children]); // Re-measure when content changes

  return (
    <div>
      <div ref={ref}>{children}</div>
      {/* Height is consistent with DOM before user sees it */}
      <p>Measured height: {height}px</p>
    </div>
  );
}
```

</details>

<details>
<summary>Caveats</summary>

* Blocks paint (can hurt performance)
* State updates here delay rendering
* Use only for DOM measurements or imperative layout work
* Prefer `useEffect` for async or non-visual side effects

</details>

---

### `useRef`

**Purpose:** Persist values across renders **without causing re-renders**.

<details>
<summary>Example: DOM access + mutable value</summary>

```js
function SearchInput() {
  const inputRef = useRef(null);      // Access DOM element
  const renderCount = useRef(0);      // Persist mutable value across renders

  // Count how many times the component rendered
  renderCount.current += 1;

  useEffect(() => {
    // Safe DOM interaction after mount
    inputRef.current.focus();
  }, []);

  function selectText() {
    // Imperative DOM action without re-rendering
    inputRef.current.select();
  }

  return (
    <div>
      <input ref={inputRef} placeholder="Search..." />
      <button onClick={selectText}>Select text</button>

      {/* Updating ref does NOT cause re-render */}
      <p>Render count: {renderCount.current}</p>
    </div>
  );
}
```

</details>

<details>
<summary>Caveats</summary>

* Updating `.current` does not trigger re-render
* Mutations are not reactive
* Avoid using refs to store UI state

</details>

---

### `useReducer`

**useReducer** is a React Hook that lets you add a reducer to your component.

<details>
<summary>Example: Real-world form state management</summary>

```js
function counterReducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return { count: 0 };
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(counterReducer, { count: 0 });

  return (
    <div>
      <p>Count: {state.count}</p>

      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </div>
  );
}
```

</details>

<details>
<summary>When to use</summary>

* Complex state transitions
* Multiple related state values
* When state changes depend on *previous* state
* When you want predictable, testable logic

</details>

---

## 2. Performance & Composition Hooks

### `useMemo`

**useMemo** is a React Hook that lets you cache the result of a calculation between re-renders.

<details>
<summary>Example</summary>

```js
function SearchResults({ items, query }) {
  // Normalize input outside of useMemo (cheap operation)
  const normalizedQuery = query.trim().toLowerCase();

  // Expensive derived data: filter + sort on a potentially large list
  const results = useMemo(() => {
    // This code only runs when `items` or `normalizedQuery` changes

    // 1. Filter items based on search query
    const filtered = items.filter(item =>
      item.name.toLowerCase().includes(normalizedQuery)
    );

    // 2. Sort results (expensive: O(n log n))
    filtered.sort((a, b) => b.score - a.score);

    return filtered;
  }, [items, normalizedQuery]);

  return (
    <ul>
      {results.map(item => (
        <li key={item.id}>
          {item.name} ({item.score})
        </li>
      ))}
    </ul>
  );
}
```

</details>

**Caveat:** Not for side effects.

---

### `useCallback`

**Purpose:** useCallback is a React Hook that lets you cache a function definition between re-renders.

<details>
<summary>Example</summary>

```js
function Example() {
  const [open, setOpen] = useState(false);
  const [count, setCount] = useState(0);

  // Cached callback with no dependencies
  const handleClick = useCallback(() => {
    // Functional update avoids stale state
    setCount(c => c + 1);
    setOpen(true);
  }, []);

  // Cached callback that depends on `count`
  const handleReset = useCallback(() => {
    if (count > 0) {
      setCount(0);
      setOpen(false);
    }
  }, [count]);

  return (
    <div>
      <button onClick={handleClick}>Open & Increment</button>
      <button onClick={handleReset}>Reset</button>

      {open && <p>Count: {count}</p>}
    </div>
  );
}
```

</details>

**Caveat:** Useful mainly when passing callbacks to memoized children.

---

### `useContext`

**useContext** is a React Hook that lets you read and subscribe to context from your component.

<details>
<summary>Example</summary>

```js
// Context is created once at module level
const ThemeContext = React.createContext('light');

function App() {
  // Provider supplies the current value to all descendants
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar() {
  // No props needed — context flows automatically
  return <Button />;
}

function Button() {
  // Reads the nearest ThemeContext value
  const theme = useContext(ThemeContext);

  // Component re-renders when context value changes
  return (
    <button className={`btn btn-${theme}`}>
      Current theme: {theme}
    </button>
  );
}
```

</details>

---

## 3. Concurrent & Modern Hooks

### `useTransition`

**Purpose:** Mark updates as non-urgent.

```js
const [isPending, startTransition] = useTransition();

startTransition(() => {
  setList(filter(items));
});
```

---

### `useDeferredValue`

**useDeferredValue** is a React Hook that lets you defer updating a part of the UI.

```js
function Search() {
  const [query, setQuery] = useState('');

  // Deferred value lags behind `query` during fast updates
  const deferredQuery = useDeferredValue(query);

  // This expensive computation will run less often
  const results = useMemo(() => {
    // Simulate heavy filtering logic
    return largeList.filter(item =>
      item.name.toLowerCase().includes(deferredQuery.toLowerCase())
    );
  }, [deferredQuery]);

  return (
    <div>
      {/* Immediate update for typing responsiveness */}
      <input
        value={query}
        onChange={e => setQuery(e.target.value)}
        placeholder="Search…"
      />

      {/* UI may briefly show results for an older query */}
      <ul>
        {results.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

---

### `useOptimistic` (React 19+)

**Purpose:** Update UI optimistically before server confirmation.

```js
const [optimistic, setOptimistic] = useOptimistic(data);
```

---

### `useActionState` (React 19+)

**Purpose:** Manage async server actions.

```js
const [state, action] = useActionState(saveUser, initial);
```

---

## 4. Advanced / Rare Hooks

### `useImperativeHandle`

**useImperativeHandle** is a React Hook that lets you customize the handle exposed as a ref.

```js
// Child component controls what the parent can access
const Input = React.forwardRef(function Input(props, ref) {
  const inputRef = useRef(null);

  // Expose a controlled, limited API to the parent
  useImperativeHandle(ref, () => ({
    focus() {
      inputRef.current.focus();
    },
    clear() {
      inputRef.current.value = '';
    },
  }));

  return <input ref={inputRef} placeholder="Type here" />;
});

function Parent() {
  const inputRef = useRef(null);

  return (
    <div>
      {/* Parent can only call methods explicitly exposed */}
      <Input ref={inputRef} />

      <button onClick={() => inputRef.current.focus()}>
        Focus Input
      </button>
      <button onClick={() => inputRef.current.clear()}>
        Clear Input
      </button>
    </div>
  );
}
```

### `useSyncExternalStore`

Subscribe safely to external stores.

### `useInsertionEffect`

Inject styles before DOM mutations.

### `useId`

**useId** is a React Hook for generating unique IDs that can be passed to accessibility attributes.

```js
function FormField() {
  // Generates a stable, unique ID per component instance
  const id = useId();

  return (
    <div>
      {/* Label and input are correctly associated */}
      <label htmlFor={id}>Email</label>
      <input id={id} type="email" />
    </div>
  );
}

function Form() {
  // Multiple instances will not collide
  return (
    <>
      <FormField />
      <FormField />
    </>
  );
}
```


### `useDebugValue`

**useDebugValue** is a React Hook that lets you display a custom label for your hook in **React DevTools**.

<details>
<summary>Example</summary>

```js
// Custom hook
function useOnlineStatus() {
  const [isOnline, setIsOnline] = React.useState(true);

  // Adds a label visible only in React DevTools
  useDebugValue(isOnline ? 'Online' : 'Offline');

  // You can still return normal hook values
  return isOnline;
}

function StatusIndicator() {
  // When inspecting this component in DevTools,
  // you'll see: useOnlineStatus: Online / Offline
  const isOnline = useOnlineStatus();

  return (
    <p>Status: {isOnline ? 'Online' : 'Offline'}</p>
  );
}
```

</details>

---

---

# Lifecycle ↔ Hook Mapping

| Class Lifecycle           | Hook Equivalent               |
| ------------------------- | ----------------------------- |
| `componentDidMount`       | `useEffect(() => {}, [])`     |
| `componentDidUpdate`      | `useEffect(() => {}, [deps])` |
| `componentWillUnmount`    | Cleanup in `useEffect`        |
| `shouldComponentUpdate`   | `React.memo`, `useMemo`       |
| `getSnapshotBeforeUpdate` | `useLayoutEffect`             |

---

## Key Takeaways

* Hooks express **intent**, not lifecycle phase
* Effects run **after commit**
* Prefer simple hooks; reach for advanced ones sparingly
