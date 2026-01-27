---

id: advanced-state-management
title: Advanced State Management
sidebar_position: 6
-------------------

# Advanced State Management

> A practical guide to **scaling state** in React using Context, reducers, custom hooks, and modern patterns — with clear trade‑offs.

---

## Mental Model

Before tools, understand *where state should live*:

* **Local state** → component (`useState`)
* **Shared state (few levels)** → lift state up
* **Shared state (many levels)** → Context
* **Complex transitions** → Reducers
* **Reusable logic** → Custom hooks
* **App‑wide / external** → External stores

---

## Context API

The **Context API** lets you share values across the component tree **without prop drilling**.

### When to Use Context

* Theme, locale, auth user
* Feature flags
* Global configuration

❌ Not ideal for rapidly changing values (can cause re‑renders).

---

### Core Context APIs

| API / Hook      | Simple Definition                  |
| --------------- | ---------------------------------- |
| `createContext` | Create a context object            |
| `Provider`      | Supply a value to descendants      |
| `useContext`    | Consume the nearest provider value |

<details>
<summary>Example: Context</summary>

```js
import { createContext, useContext } from 'react';

const ThemeContext = createContext('light');

function Toolbar() {
  const theme = useContext(ThemeContext);
  return <button className={theme}>Theme Button</button>;
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

## Context Performance Patterns (Often Missed)

### Splitting Contexts

* One context per concern
* Avoid "mega contexts"

```js
const AuthContext = createContext(null);
const ThemeContext = createContext('light');
```

### Memoizing Provider Values

```js
const value = useMemo(() => ({ user, login }), [user]);
```

---

## Custom Hooks

**Custom hooks** extract and reuse stateful logic.

### Rules

* Must start with `use`
* Can call other hooks
* No JSX inside custom hooks

<details>
<summary>Example: Custom Hook</summary>

```js
import { useState, useEffect } from 'react';

function useWindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const onResize = () => setWidth(window.innerWidth);
    window.addEventListener('resize', onResize);
    return () => window.removeEventListener('resize', onResize);
  }, []);

  return width;
}
```

</details>

---

## Reducers (`useReducer`)

Reducers centralize **state transitions** and make updates predictable.

### When to Prefer `useReducer`

* Multiple related state values
* Complex update logic
* State transitions depend on previous state

<details>
<summary>Example: useReducer</summary>

```js
import { useReducer } from 'react';

function reducer(state, action) {
  switch (action.type) {
    case 'inc': return { count: state.count + 1 };
    case 'dec': return { count: state.count - 1 };
    default: return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <>
      <p>{state.count}</p>
      <button onClick={() => dispatch({ type: 'inc' })}>+</button>
      <button onClick={() => dispatch({ type: 'dec' })}>-</button>
    </>
  );
}
```

</details>

---

## Reducer + Context (Global State Pattern)

This pattern replaces small Redux‑like use cases.

<details>
<summary>Example: Context + Reducer</summary>

```js
const CountContext = createContext();

function reducer(state, action) {
  if (action.type === 'inc') return state + 1;
  return state;
}

function Provider({ children }) {
  const [state, dispatch] = useReducer(reducer, 0);
  return (
    <CountContext.Provider value={{ state, dispatch }}>
      {children}
    </CountContext.Provider>
  );
}
```

</details>

---

## Prop Drilling vs Context

| Pattern       | Description               | When to Use |
| ------------- | ------------------------- | ----------- |
| Prop Drilling | Pass props through layers | Small trees |
| Context       | Implicit dependency       | Deep trees  |

<details>
<summary>Example Comparison</summary>

```js
// Prop drilling
function Child({ theme }) { return <p>{theme}</p>; }
function Parent({ theme }) { return <Child theme={theme} />; }

// Context
const ThemeContext = createContext('light');
function ChildCtx() {
  return <p>{useContext(ThemeContext)}</p>;
}
```

</details>

---

## External State & Stores (Often Missing)

For **app‑wide or cross‑framework state**:

* Redux / Redux Toolkit
* Zustand
* Jotai
* MobX

### React Integration Hook

| Hook                   | Purpose                              |
| ---------------------- | ------------------------------------ |
| `useSyncExternalStore` | Safe subscription to external stores |

---

## Common Mistakes

* Overusing context for frequently changing state
* Putting UI‑local state in global stores
* Creating new provider values on every render
* Mixing unrelated concerns in one reducer

---

## Key Takeaways

* Start local, scale state **only when needed**
* Context solves **prop drilling**, not all state
* Reducers make complex updates predictable
* Custom hooks are the foundation of reuse
* External stores are for **cross‑app state**
