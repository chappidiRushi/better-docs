---
id: hooks
title: Hooks
sidebar_position: 5
-------------------
# Lifecycle & Effects

> Clean and structured guide covering lifecycle methods for class components and hooks for function components

---

## Class Component Lifecycle Methods (Execution Order)

| Phase          | Method                                               | Description                                                       |
| -------------- | ---------------------------------------------------- | ----------------------------------------------------------------- |
| Mounting       | `constructor(props)`                                 | Initialize state, bind methods                                    |
|                | `static getDerivedStateFromProps(props, state)`      | Update state based on props before rendering                      |
|                | `render()`                                           | Render JSX to DOM                                                 |
|                | `componentDidMount()`                                | Runs after component is mounted; API calls, timers, subscriptions |
| Updating       | `static getDerivedStateFromProps(props, state)`      | Update state from props before rendering                          |
|                | `shouldComponentUpdate(nextProps, nextState)`        | Decide if re-render is needed (optimization)                      |
|                | `render()`                                           | Render updated JSX                                                |
|                | `getSnapshotBeforeUpdate(prevProps, prevState)`      | Capture snapshot of DOM before changes                            |
|                | `componentDidUpdate(prevProps, prevState, snapshot)` | Perform side effects after update                                 |
| Unmounting     | `componentWillUnmount()`                             | Cleanup timers, listeners, subscriptions                          |
| Error Handling | `static getDerivedStateFromError(error)`             | Update state to display fallback UI                               |
|                | `componentDidCatch(error, info)`                     | Log error details or perform side effects                         |

<details>
<summary>Example Class Component</summary>

```js
import React from 'react';

class LifecycleDemo extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0, hasError: false };
  }

  static getDerivedStateFromProps(props, state) { return null; }

  componentDidMount() { console.log('Mounted'); }

  shouldComponentUpdate(nextProps, nextState) { return true; }

  getSnapshotBeforeUpdate(prevProps, prevState) { return null; }

  componentDidUpdate(prevProps, prevState, snapshot) { console.log('Updated'); }

  componentWillUnmount() { console.log('Unmounted'); }

  static getDerivedStateFromError(error) { return { hasError: true }; }

  componentDidCatch(error, info) { console.log('Error:', error, info); }

  render() {
    if (this.state.hasError) return <h1>Something went wrong</h1>;
    return <p>Count: {this.state.count}</p>;
  }
}
```

</details>

---

## Function Component Hooks

| Hook                                     | Purpose                                                  |
| ---------------------------------------- | -------------------------------------------------------- |
| `useState()`                             | Local state                                              |
| `useEffect(() => { ... }, [])`           | Mounting; runs once after mount                          |
| `useEffect(() => { ... }, [deps])`       | Updating; runs when dependencies change                  |
| `useLayoutEffect(() => { ... }, [deps])` | Synchronous effect after DOM updates but before painting |
| `useReducer()`                           | Manage complex state logic                               |
| `useRef()`                               | Persistent values and DOM refs                           |
| `useMemo()`                              | Memoize expensive calculations                           |
| `useCallback()`                          | Memoize functions                                        |
| `useContext()`                           | Access context values                                    |
| `useImperativeHandle()`                  | Customize refs with `forwardRef`                         |
| `useDebugValue()`                        | Show debug info in DevTools                              |

### Cleanup / Unmounting

* Return a cleanup function from `useEffect` or `useLayoutEffect`

### Error Handling

* No built-in hook; use class-based `ErrorBoundary`

<details>
<summary>Example Function Component with Hooks</summary>

```js
import { useState, useEffect, useLayoutEffect, useRef, useMemo, useCallback, useContext } from 'react';
import MyContext from './MyContext';

function HookDemo({ multiplier }) {
  const [count, setCount] = useState(0);
  const ref = useRef();
  const context = useContext(MyContext);

  const doubled = useMemo(() => count * multiplier, [count, multiplier]);
  const increment = useCallback(() => setCount(c => c + 1), []);

  useEffect(() => {
    console.log('Effect after mount or update');
    return () => console.log('Cleanup on unmount or before next update');
  }, [count]);

  useLayoutEffect(() => {
    console.log('Layout effect');
  });

  return (
    <div ref={ref}>
      <p>Count: {count}</p>
      <p>Doubled: {doubled}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}
```

</details>

---

## Notes & Caveats

* Class components: use explicit lifecycle methods; function components: hooks.
* `useEffect` covers mount, update, and unmount depending on dependencies.
* `useLayoutEffect` is synchronous, before browser paint.
* Error boundaries are still class-based.
* Always include dependencies in hooks to avoid unintended behavior.
