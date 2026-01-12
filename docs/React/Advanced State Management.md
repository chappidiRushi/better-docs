---
id: advanced-state-management
title: Advanced State Management
sidebar_position: 6
-------------------

# Advanced State Management

> Guide covering context, custom hooks, reducers, and patterns to manage state effectively

---

## Context API

Context provides a way to share values globally without prop drilling.

| Method / Hook                       | Purpose                                                 |
| ----------------------------------- | ------------------------------------------------------- |
| `React.createContext(defaultValue)` | Creates a context object with optional default value    |
| `Context.Provider`                  | Wraps components to provide a value to descendants      |
| `useContext(Context)`               | Hook to access the context value in function components |

<details>
<summary>Example: Context</summary>

```js
import React, { createContext, useContext } from 'react';

const ThemeContext = createContext('light');

function Toolbar() {
  const theme = useContext(ThemeContext);
  return <button style={{ background: theme === 'dark' ? '#333' : '#eee' }}>Theme Button</button>;
}

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}
```

</details>

---

## Custom Hooks

Custom hooks encapsulate reusable logic for function components.

<details>
<summary>Example: Custom Hook</summary>

```js
import { useState, useEffect } from 'react';

function useWindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return width;
}

function App() {
  const width = useWindowWidth();
  return <p>Window width: {width}</p>;
}
```

</details>

---

## Reducers (`useReducer`)

`useReducer` is an alternative to `useState` for complex state logic.

<details>
<summary>Example: useReducer</summary>

```js
import { useReducer } from 'react';

function reducer(state, action) {
  switch(action.type) {
    case 'increment': return { count: state.count + 1 };
    case 'decrement': return { count: state.count - 1 };
    default: return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });
  return (
    <>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </>
  );
}
```

</details>

---

## Prop Drilling vs Context Pattern

| Pattern       | Description                                         | When to Use                                               |
| ------------- | --------------------------------------------------- | --------------------------------------------------------- |
| Prop Drilling | Passing props through multiple layers of components | Simple apps, few levels deep                              |
| Context API   | Provides data globally to component tree            | Complex apps, many nested components, shared global state |

<details>
<summary>Example: Prop Drilling vs Context</summary>

```js
// Prop Drilling
function Child({ theme }) { return <p>{theme}</p>; }
function Parent({ theme }) { return <Child theme={theme} />; }
function App() { return <Parent theme="dark" />; }

// Context
const ThemeContext = React.createContext('light');
function ChildContext() {
  const theme = useContext(ThemeContext);
  return <p>{theme}</p>;
}
function AppContext() {
  return (
    <ThemeContext.Provider value="dark">
      <ChildContext />
    </ThemeContext.Provider>
  );
}
```

</details>

---

## Notes & Caveats

* Use context for global/shared state to avoid prop drilling.
* Custom hooks promote code reuse and clean logic.
* `useReducer` is preferred when state updates depend on previous state or are complex.
* Avoid overusing context for frequently changing values; can trigger unnecessary re-renders.
