**Beginner**

- [**Hardcore**](#hardcore)
- [Introduction](#introduction)
- [Beginner Concepts](#beginner-concepts)
  - [How React Works](#how-react-works)
  - [Components](#components)
  - [2. JSX (JavaScript XML)](#2-jsx-javascript-xml)
  - [3. Props (Properties)](#3-props-properties)
  - [5. Events Handling](#5-events-handling)
    - [Synthetic Events](#synthetic-events)
    - [Event Summary](#event-summary)
  - [4. State](#4-state)
- [📘 React Hooks](#-react-hooks)
  - [🪝 Basic Hooks](#-basic-hooks)
    - [1. `useState`](#1-usestate)
    - [2. `useEffect`](#2-useeffect)
    - [3. `useContext`](#3-usecontext)
  - [⚙️ More Useful Hooks](#️-more-useful-hooks)
    - [4. `useReducer`](#4-usereducer)
    - [5. `useRef`](#5-useref)
    - [6. `useMemo`](#6-usememo)
    - [7. `useCallback`](#7-usecallback)
    - [8. `useLayoutEffect`](#8-uselayouteffect)
    - [9. `useImperativeHandle`](#9-useimperativehandle)
  - [🧩 Custom Hooks](#-custom-hooks)
  - [🧠 Rules of Hooks](#-rules-of-hooks)
  - [📦 Common Hooks from Libraries](#-common-hooks-from-libraries)
  - [🔍 Hook Summary](#-hook-summary)
  - [Classed based hooks](#classed-based-hooks)
- [Intermediate](#intermediate)
  - [Lifting State Up](#lifting-state-up)
  - [React Router (Routing \& Navigation)](#react-router-routing--navigation)
  - [Code Splitting \& Lazy Loading](#code-splitting--lazy-loading)
  - [Error Boundaries](#error-boundaries)
  - [Portals](#portals)
  - [Higher-Order Components (HOC)](#higher-order-components-hoc)
  - [React Compound Components](#react-compound-components)
    - [Why Use Compound Components?](#why-use-compound-components)
  - [Summary](#summary)
- [Advanced](#advanced)
  - [React Performance Optimization (Memoization, `useMemo`, `useCallback`)](#react-performance-optimization-memoization-usememo-usecallback)
  - [Concurrent Mode](#concurrent-mode)
    - [Benefits](#benefits)
    - [Enabling Concurrent Mode](#enabling-concurrent-mode)
  - [React Query / SWR](#react-query--swr)
  - [State Management (Redux, Zustand, Jotai, Recoil)](#state-management-redux-zustand-jotai-recoil)
  - [Prop Drilling vs Context API vs Redux](#prop-drilling-vs-context-api-vs-redux)
  - [Micro-Frontends with React](#micro-frontends-with-react)
    - [Benefits of Micro-Frontends](#benefits-of-micro-frontends)
    - [Implementing Micro-Frontends with React](#implementing-micro-frontends-with-react)
    - [Challenges](#challenges)
- [Redux](#redux)
  - [Core Concepts](#core-concepts)
  - [Modern Redux with Redux Toolkit](#modern-redux-with-redux-toolkit)
  - [Example: Counter App](#example-counter-app)
    - [1. Create a Redux Slice](#1-create-a-redux-slice)
    - [2. Set Up the Redux Store](#2-set-up-the-redux-store)
    - [3. Provide the Store to React](#3-provide-the-store-to-react)
    - [4. Use Redux in Components](#4-use-redux-in-components)
  - [Summary](#summary-1)
  - [Advantages of Redux](#advantages-of-redux)
  - [Async Redux-thunk](#async-redux-thunk)
- [📘 React Functional Components vs Class Components](#-react-functional-components-vs-class-components)
- [React Router](#react-router)
  - [Key Concepts and Definitions](#key-concepts-and-definitions)
  - [Installation](#installation)
  - [Basic Usage Example](#basic-usage-example)
  - [Detailed Features](#detailed-features)
    - [1. Dynamic URL Parameters](#1-dynamic-url-parameters)
    - [2. Nested Routes and Outlet](#2-nested-routes-and-outlet)
    - [3. Programmatic Navigation](#3-programmatic-navigation)
    - [4. Redirects with Navigate](#4-redirects-with-navigate)
    - [5. Not Found (404) Route](#5-not-found-404-route)
  - [Useful Hooks Summary](#useful-hooks-summary)
  - [Summary](#summary-2)
  - [Official Documentation](#official-documentation)

## **Hardcore**

- Advanced Suspense (Streaming & SSR)
- Concurrent Rendering Strategies
- React Server Components
- Optimizing Large-Scale Applications
- Custom Renderers (React Reconciler)
- Monorepos in React (Turborepo, NX)
- WebAssembly with React
- Advanced Animations (Framer Motion, GSAP)
- React Compiler (Future of React Optimization)


## Introduction

React is a JavaScript library for building user interfaces. It allows developers to create reusable UI components and manage the state of those components efficiently.

## Beginner Concepts

### [How React Works](https://www.youtube.com/watch?v=i793Qm6kv3U)

1. React Use a Babel compiler to transform JSX into regular JS function calls that create react elements.
2. When you can create function it will crete an instance.
3. the instance return an react element(virtual dom).
4. React elements are plain JS objects that describe what should appear on the screen.

    ```jsx
    function App() {
      console.log(MyComp())
      return (<><MyComp></MyComp></>);
      }
      function MyComp() {
        return <h1>Hello world <p>meow P</p></h1>
        }
        // react element object AKA virtual DOM.
        {
          $$typeof : Symbol(react.transitional.element),
          key: null,
          type: 'h1'
          props:{
            children: ["Hello world", {p virtual dom node.}],
            // events ids class other attrs goes here.
          },
          _owner : 'internal thing',
          _store: 'internal thing'
        }
    ```

5. React compares the new virtual DOM tree with the previous one to determine the minimal set of changes needed to update the actual DOM. This process known as reconciliation.
   1. React Uses multiple Strategies for efficiently.
      1. `Element type comparison` updates the attributes if the type is same, replaces element if different.
      2. `Component type comparison` keeps the component if the type is same, re-renders if different.
      3. `Keys in list` Helps React track and reorder elements efficiently.
      4. `Tree Diffing` only updates the subtrees that changed instead of re-rendering everything.
      5. `State Preservation` Tries to keep the state when moving elements. If elements are moved around but retain the same key, React keeps their state.
6. React uses the Fiber reconciliation algorithm to efficiently update the UI.
   1. **Incremental Rendering**
      1. it `split rendering work into small units` and can pause and resume as needed.
   2. **Scheduling & Prioritization**
      1. it assign priority levels to rendering tasks.
      2. High priority happens immediately vice-versa.
      3. `Animation, Transitions` have `high` priority.
      4. `Data fetching` have `low` priority.
      5. `Off-screen` updates have `least` priority
   3. **Concurrent Mode**
      1. it allows multiple tasks to run **Parallel**
      2. it can pause work, handle urgent updates, and then continue rendering.
   4. **Batching Updates**
      1. It **Batches multiple updates** together to improve performance.
   5. **Asynchronous Rendering**
      1. React can Pause/Resume work instead of blocking the UI.
   6. **Error Boundaries**
      1. Prevents Entire App crashes by catching component errors.
   7. **Enhanced SSR**
      1. improves page load speed by streaming content.
   8. **Suspense & Lazy loading**
      1. Allows components to wait for async data before rendering.

7. Re renders and UI Updates
   1. when state or props or state changes, reacts triggers re renders.
   2. The re-render updates the Virtual DOM.
   3. The diffing algorithm identifies changes.
   4. React updates only the necessary parts of the real DOM.

| Feature / Aspect              | **Old Reconciler (React ≤15)**          | **React Fiber (React 16+)**                     |
| ----------------------------- | --------------------------------------- | ----------------------------------------------- |
| **Purpose**                   | Reconciliation (diffing & updating DOM) | Reconciliation (diffing & updating DOM)         |
| **Engine Name**               | Stack Reconciler                        | React Fiber                                     |
| **Execution Model**           | Synchronous (all updates in one go)     | Asynchronous & interruptible                    |
| **Scheduling**                | Not supported                           | Built-in priority-based scheduling              |
| **Interruption / Pause**      | ❌ Not possible                          | ✅ Yes — work can be paused/resumed              |
| **Time Slicing**              | ❌ Not available                         | ✅ Supported                                     |
| **Granularity**               | Whole component tree in one step        | Breaks updates into units of work (Fiber nodes) |
| **Support for Suspense**      | ❌ Not supported                         | ✅ Yes                                           |
| **Support for Concurrent UI** | ❌ Not possible                          | ✅ Yes                                           |
| **Code Architecture**         | Single call stack                       | Linked list of fibers (fiber tree)              |
| **Error Handling**            | Basic                                   | Improved (e.g., `componentDidCatch`)            |




### Components

Components are the building blocks of React applications. They are independent and reusable pieces of code that represent a part of the UI.
Components allow you to focus on describing the UI you want, rather than focusing on the details of how the UI actually gets inserted into the page.

**Function Components:**

Simple JavaScript functions that return JSX.

    ```jsx
    function Welcome(props) {
      return <h1>Hello, {props.name}</h1>;
    }
    ```

### 2. JSX (JavaScript XML)

**What is JSX?**

- A syntax extension for JavaScript that allows you to write HTML-like code within JavaScript.
- It's transformed into regular JavaScript function calls.

```jsx
const element = <h1>Hello, World!</h1>;
```

**Embedding Expressions:**

Embedding expressions in JSX allows you to render dynamic content by evaluating JavaScript expressions inside curly braces {} within a React component.

```jsx
const name = 'Alice';
const element = <h1>Hello, {name}</h1>;
  ```

### 3. Props (Properties)

**What are props?**

- In a react component, Props are used to pass data from a parent component to a child component.
- They are read-only.

  ```jsx
  function Greeting(props) {
    return <p>Hello, {props.message}!</p>;
  }
  function App() {
    return <Greeting message="Welcome to React!" />;
  }
  ```
### 5. Events Handling

Event handling refers to the process of responding to the user interactions or system generated events. like clicks mouse movements etc.

Synthetic events wrap native events for performance and compatibility.

```jsx
import React from 'react';

function ButtonComponent() {
  function handleClick() { alert('Button clicked!'); }
  function handleClick1(name) {alert(`Hello, ${name}!`)}
  return <>
  <button onClick={handleClick}>Click Me</button>
  {/* Passing Arguments to Event Handlers */}
<button onClick={() => handleClick1('John')}>Click Me</button>;
  </>;
}
```

#### Synthetic Events

A Synthetic event is a cross-browser wrapper around native dom event. it provide consist API regardless of browser.

- cross-browser compatibility.
- event pooling.
  - event pooling is an optimization technique where event objects were reused for performance reasons. Instead of creating the new event object every event, react would `Pool` and reuse them.

```jsx
function InputComponent() {
  function handleChange(event) {
    event.persist(); // Prevents event object from being nullified
    setTimeout(() => console.log(event.target.value), 1000);
  }
  return <input type="text" onChange={handleChange} />;
}
```

#### Event Summary

- Use camelCase event names.
- Pass event handlers as functions.
- Use synthetic events (`SyntheticEvent`).
- Bind `this` in class components.
- Prevent default behavior using `event.preventDefault()`.


### 4. State

- In react, state refers to a variable managed by react hook.
- State is used to store data that can change over time within a component.
- It's managed within the component itself.
**useState Hook:**
- Used in functional components to manage state.

    ```jsx
    import React, { useState } from 'react';

    function Counter() {
      const [count, setCount] = useState(0);

      return (
        <div>
          <p>Count: {count}</p>
          <button onClick={() => setCount(count + 1)}> Increment </button>
        </div>
      );
    }
    ```

## 📘 React Hooks

Hooks are special functions in React that allow you to "hook into" React features such as **state**, **lifecycle methods**, **context**, and **refs** from *functional components*, making class components largely obsolete.

### 🪝 Basic Hooks

#### 1. `useState`

**What:** Lets your component store data (like count, form input, etc.)

**Use it when:** You want to keep track of a value that changes over time.

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0); // count is the current value, setCount updates it

  return (
    <>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Add</button>
    </>
  );
}
```

---

#### 2. `useEffect`

**What:** Runs code after the component renders (side effects like fetch, timer).

**Use it when:** You want to fetch data, use timers, or update the DOM.

```jsx
import { useEffect, useState } from 'react';

function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    const timer = setInterval(() => setSeconds(s => s + 1), 1000);
    return () => clearInterval(timer); // clean up on unmount
  }, []); // empty array means run only once

  return <p>{seconds} seconds</p>;
}
```

---

#### 3. `useContext`

**What:** Lets you use values from a React Context.

**Use it when:** You need to share data (like theme, user) across components.

```jsx
import { createContext, useContext, useState } from 'react';
import './App.css';

const CC = createContext("light")

const Child = function () {
  const {theme, setTheme} = useContext(CC);
  return <>
  <button onClick={()=>{(theme === "light"? setTheme("dark"): setTheme("light"))}}>click me</button>
    <h1>hello world {theme}</h1>
  </>
}

function App() {
  const [theme, setTheme] = useState("light")
  return (
    <>
    <CC.Provider value={{ theme, setTheme}}>
      <div>
        <h1>start</h1>
        <Child></Child>
      </div>
    </CC.Provider>
    </>
  )
}

export default App

```

---

### ⚙️ More Useful Hooks

#### 4. `useReducer`

**What:** Like `useState`, but better for complex or multi-step updates.

**Use it when:** You need to manage complex state (like a form or counter with many actions).

```jsx
function reducer(state, action) {
  switch (action.type) {
    case 'add': return { count: state.count + 1 };
    case 'subtract': return { count: state.count - 1 };
    default: return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <>
      <p>{state.count}</p>
      <button onClick={() => dispatch({ type: 'add', otherData: {} })}>+</button>
      <button onClick={() => dispatch({ type: 'subtract' })}>-</button>
    </>
  );
}
```

---

#### 5. `useRef`

**What:** Stores a value that doesn’t cause re-renders when changed. Often used to access DOM elements.

**Use it when:** You need to focus an input or store a value across renders.

```jsx
import { useRef, useEffect } from 'react';

function FocusInput() {
  const inputRef = useRef(); // points to input element

  useEffect(() => {
    inputRef.current.focus(); // focuses input on load
  }, []);

  return <input ref={inputRef} />;
}
```

---

#### 6. `useMemo`

**What:** Saves a calculated value so it doesn’t get recalculated on every render.

**Use it when:** You have a slow function and only want it to run when inputs change.

```jsx
import { useMemo } from 'react';

function ExpensiveComponent({ num }) {
  const result = useMemo(() => slowFunction(num), [num]);

  return <div>Result: {result}</div>;
}
```

---

#### 7. `useCallback`

**What:** Saves a function so it doesn’t get recreated every time the component re-renders.

**Use it when:** You pass functions to child components and want to avoid re-renders.

```jsx
import { useCallback, useState } from 'react';

function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    setCount(c => c + 1);
  }, []);

  return <Child onClick={handleClick} />;
}
```

---

#### 8. `useLayoutEffect`

**What:** Like `useEffect`, but runs earlier—**before the screen is updated**.

**Use it when:** You need to measure or adjust layout before the user sees it.

```jsx
import { useLayoutEffect, useRef } from 'react';

function Layout() {
  const boxRef = useRef();

  useLayoutEffect(() => {
    console.log(boxRef.current.getBoundingClientRect());
  }, []);

  return <div ref={boxRef}>Measure this</div>;
}
```

---

#### 9. `useImperativeHandle`

**What:** Used with `forwardRef` to expose methods from a component to its parent.

**Use it when:** You want to give the parent component custom control over a child.

```jsx
import { forwardRef, useImperativeHandle, useRef } from 'react';

const CustomInput = forwardRef((props, ref) => {
  const inputRef = useRef();

  useImperativeHandle(ref, () => ({
    focus: () => inputRef.current.focus()
  }));

  return <input ref={inputRef} />;
});
```

---

### 🧩 Custom Hooks

**What:** A way to reuse hook logic across components.

**Use it when:** You repeat the same state/effect logic in multiple places.

```jsx
import { useState, useEffect } from 'react';

function useWindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const resize = () => setWidth(window.innerWidth);
    window.addEventListener('resize', resize);
    return () => window.removeEventListener('resize', resize);
  }, []);

  return width;
}
```

---

### 🧠 Rules of Hooks

- ✅ Only call hooks **at the top level** of a function.
- ✅ Only use hooks **inside React functions** (components or custom hooks).
- ❌ Don’t use hooks in loops, conditions, or regular functions.

---

### 📦 Common Hooks from Libraries

| Library         | Hooks                        | Purpose                 |
| --------------- | ---------------------------- | ----------------------- |
| React Router    | `useNavigate`, `useParams`   | Page routing/navigation |
| React Redux     | `useSelector`, `useDispatch` | Global state management |
| React Hook Form | `useForm`                    | Easy form handling      |
| React Query     | `useQuery`, `useMutation`    | Data fetching & caching |

---

### 🔍 Hook Summary

| Hook                  | What it does                                 |
| --------------------- | -------------------------------------------- |
| `useState`            | Store and update state                       |
| `useEffect`           | Run side effects (fetch, timers, etc.)       |
| `useContext`          | Read values from context                     |
| `useReducer`          | Handle complex state logic                   |
| `useRef`              | Store a value or DOM reference               |
| `useMemo`             | Save expensive calculation result            |
| `useCallback`         | Save function to prevent unnecessary renders |
| `useLayoutEffect`     | Like `useEffect` but runs before paint       |
| `useImperativeHandle` | Let parent call methods in child             |
| Custom Hook           | Reuse stateful logic across components       |

### Classed based hooks

| Lifecycle Method           | Phase           | Purpose                        |
| -------------------------- | --------------- | ------------------------------ |
| `constructor`              | Mounting        | Initialize state, bind methods |
| `getDerivedStateFromProps` | Mounting/Update | Sync state with props          |
| `render`                   | Mounting/Update | Render JSX                     |
| `componentDidMount`        | Mounting        | Side effects after mount       |
| `shouldComponentUpdate`    | Updating        | Control re-rendering           |
| `getSnapshotBeforeUpdate`  | Updating        | Capture DOM info before update |
| `componentDidUpdate`       | Updating        | Side effects after update      |
| `componentWillUnmount`     | Unmounting      | Cleanup before removal         |
| `getDerivedStateFromError` | Error           | Update UI on error             |
| `componentDidCatch`        | Error           | Log error and side effects     |

---

## Intermediate

### Lifting State Up

Lifting state up is the process of moving state from a child component to a common parent component, allowing multiple child components to share and synchronize the same state.

To keep state in a single place for better management. Enable communication between sibling components and Avoids redundant state

```jsx
unction App() {
  const [count, setCount] = useState(0);
  return (
    <>
      <h1>In parent is {count}</h1>
      <Child count={count} setCount={setCount} ></Child>
      <Child count={count} setCount={setCount} ></Child>
    </>
  );
}

function Child({ count, setCount }) {
  return <button onClick={() => setCount((oldVal) => oldVal + 1)}>
    count in child  {count}
  </button>
}
```

### React Router (Routing & Navigation)

### Code Splitting & Lazy Loading

- Code splitting is a technique to split javascript bundle into multiple chunks. this will reduce the initial bundle size and improve performance.
- lazy loading is a technique to load components only when they are needed, reducting the initial bundle size. React uses `React.lazy()` and `Suspence` to achieve this.

```jsx
import React, { lazy, Suspense } from "react";
// Lazy load the component
const LazyComponent = lazy(() => import("./LazyComponent"));
function App() {
  return (
    <div>
      <h1>Welcome</h1>
      <Suspense fallback={<p>Loading...</p>}>
        <LazyComponent />
      </Suspense>
    </div>
  );
}

```

### Error Boundaries

Error boundaries are react components that catch JavaScript errors in their child component tree, log the errors and display a fallback UI instead of crashing the whole app.

A class component becomes an error boundary if it defines either (or both) `static getDerivedStateFromError()` or `componentDidCatch()` methods.

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Update state so the next render will show the fallback UI.
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // log the error to an error reporting service
    console.log(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}
```

### Portals

Portals let to render some children into a different part of the DOM.

```jsx
import { createPortal } from "react-dom";
function App() {
  return (
    <>
      <h1>Parent</h1>
        {createPortal(<Child ></Child>, document.body)}
    </>
  );
}
```

### Higher-Order Components (HOC)

A higher order component is a function that takes a component and return a new component.
[for more](https://legacy.reactjs.org/docs/higher-order-components.html)

### React Compound Components
Compound Components are a React pattern where multiple components work together.

- One parent component holds the shared logic (like state).
- Child components access and use that logic automatically.
- This pattern makes it easier to build flexible and reusable UIs.

#### Why Use Compound Components?

- Cleaner and more readable code
- Easy for others to use and understand
- Allows flexible layouts with shared behavior

```jsx
import React, { useState, useContext, createContext } from 'react';

// 1. Create context to share toggle state
const ToggleContext = createContext();

// 2. Main component that provides state
function Toggle({ children }) {
  const [on, setOn] = useState(false);
  const toggle = () => setOn(prev => !prev);

  return (
    <ToggleContext.Provider value={{ on, toggle }}>
      {children}
    </ToggleContext.Provider>
  );
}

// 3. Sub-components using the shared context

Toggle.On = function On({ children }) {
  const { on } = useContext(ToggleContext);
  return on ? children : null;
};

Toggle.Off = function Off({ children }) {
  const { on } = useContext(ToggleContext);
  return !on ? children : null;
};

Toggle.Button = function Button() {
  const { on, toggle } = useContext(ToggleContext);
  return <button onClick={toggle}>{on ? 'Turn OFF' : 'Turn ON'}</button>;
};

export default Toggle;
```

### Summary

- The main component shares logic using React Context.
- Child components consume that context and behave accordingly.
- This pattern keeps components flexible, reusable, and clean.


## Advanced

### React Performance Optimization (Memoization, `useMemo`, `useCallback`)

- React.memo()
- useCallBack()
- useMemo()
- React.lazy() & Suspense
- Debouncing and throttling Expensive Operations.
- React-window
  - rendering a large list directly affects performance. `Virtualization` renders only visible items.

  ```jsx
  // npm i react-window
  import { FixedSizeList as List } from "react-window";
  const items = Array.from({ length: 1000 }, (_, i) => `Item ${i}`);

  function VirtualizedList() {
    return (
      <List height={400} itemCount={items.length} itemSize={35} width={300}>
        {({ index, style }) => <div style={style}>{items[index]}</div>}
      </List>
    );
  }
  ```

### Concurrent Mode

Concurrent Mode is a set of features that improve the performance and responsiveness of React applications by rendering multiple UI updates simultaneously without blocking the main thread.

#### Benefits

- **Interruptible Rendering**: React can pause and resume rendering based on priority.
- **Improved Performance**: Enables seamless updates without freezing the UI.
- **Automatic Batching**: Groups multiple state updates together to minimize re-renders.

#### Enabling Concurrent Mode

Concurrent Mode is enabled automatically when using features like `Suspense` and `useTransition`, but you can explicitly enable it with React 18's `createRoot` API:

1. `useTransition`
Allows marking state updates as non-urgent, improving UI responsiveness.

    ```jsx
    import React, { useState, useTransition } from 'react';

    function App() {
      const [isPending, startTransition] = useTransition();
      const [text, setText] = useState('');

      const handleChange = (e) => {
        startTransition(() => {
          setText(e.target.value);
        });
      };

      return (
        <div>
          <input type="text" onChange={handleChange} />
          {isPending ? <p>Loading...</p> : <p>Results for: {text}</p>}
        </div>
      );
    }
    ```

2. `useDeferredValue`
Helps defer updates for expensive computations without blocking user input.

      ```jsx
      import React, { useState, useDeferredValue } from 'react';

      function SearchResults({ query }) {
        const deferredQuery = useDeferredValue(query);

        return <p>Showing results for: {deferredQuery}</p>;
      }

      function App() {
        const [query, setQuery] = useState('');

        return (
          <div>
            <input onChange={(e) => setQuery(e.target.value)} />
            <SearchResults query={query} />
          </div>
        );
      }
      ```

### React Query / SWR

data fetching and state management library.
key features: `Data Fetching & Caching`, `Automatic Refreshing`, `Background Updates`, `Pagination & Infinite Queries`, `Optimistic Updates`, `Error Handling & Retries`

### State Management (Redux, Zustand, Jotai, Recoil)

### Prop Drilling vs Context API vs Redux

- **Prop Drilling:** Passing data from a parent component to deeply nested child components through intermediate components.
- **Context API:** A React feature that allows global state sharing without prop drilling.
- **Redux:** An external library for managing global state using a single store and actions to update state predictably.

| Aspect         | Prop Drilling                | Context API                              | Redux                                      |
| -------------- | ---------------------------- | ---------------------------------------- | ------------------------------------------ |
| Purpose        | Pass data through components | Share global state without prop drilling | Manage global state predictably            |
| Complexity     | Low (for small apps)         | Medium                                   | High (more setup and boilerplate)          |
| Performance    | Good (for small data)        | Can cause unnecessary re-renders         | High (if optimized with middleware)        |
| Use Case       | Simple, small apps           | Medium apps with simple global state     | Large, complex apps needing predictability |
| Learning Curve | Low                          | Medium                                   | High                                       |



### Micro-Frontends with React

Micro-frontends are an architectural approach to breaking down a frontend application into smaller, independent pieces, each responsible for a specific feature or domain. This pattern brings the concept of microservices to the frontend, allowing teams to build, deploy, and maintain separate parts of a large application independently.

#### Benefits of Micro-Frontends
- **Scalability:** Each team can work on a different part of the application simultaneously.
- **Independent Deployment:** Update or deploy a single micro-frontend without affecting others.
- **Tech Stack Flexibility:** Different micro-frontends can use different frameworks or libraries.
- **Isolation:** Reduces the risk of breaking the entire application due to a bug in one part.

#### Implementing Micro-Frontends with React
1. **Module Federation (Webpack 5):** Allows dynamic import of remote modules at runtime.
2. **Single SPA:** Manages multiple frameworks on the same page without refreshing.
3. **Custom Elements:** Uses Web Components to encapsulate each micro-frontend.
4. **iframes:** Offers strong isolation but has performance drawbacks.

#### Challenges
- **Shared State Management:** Synchronizing global state between micro-frontends can be tricky.
- **Routing:** Managing routes across different micro-frontends requires coordination.
- **Versioning:** Ensuring compatibility between shared dependencies.

Micro-frontends with React enable scaling and team autonomy while maintaining a consistent user experience. Choosing the right integration technique depends on factors like performance, isolation, and complexity.

## Redux
Redux is a **state management library** for JavaScript apps. It helps you manage **global state** (state that many components need to share) in a predictable way.

- Many components need access to the same state (like user info or theme).
- You want **one source of truth** for state.
- Your app is complex and needs better organization.

### Core Concepts

| Concept  | Description                                           |
| -------- | ----------------------------------------------------- |
| Store    | Holds the app’s state                                 |
| Action   | An object describing what happened                    |
| Reducer  | A function that updates the state based on the action |
| Dispatch | A function to send actions to the reducer             |
| Selector | A function to read specific data from the store       |

### Modern Redux with Redux Toolkit

Redux Toolkit is the recommended way to use Redux.

Install it:

```bash
npm install @reduxjs/toolkit react-redux
```

### Example: Counter App

#### 1. Create a Redux Slice

```js
// src/features/counter/counterSlice.js
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: state => { state.value += 1 },
    decrement: state => { state.value -= 1 },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    }
  }
});

export const { increment, decrement, incrementByAmount } = counterSlice.actions;
export default counterSlice.reducer;
```

#### 2. Set Up the Redux Store

```js
// src/app/store.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../features/counter/counterSlice';

export const store = configureStore({
  reducer: {
    counter: counterReducer
  }
});
```

#### 3. Provide the Store to React

```jsx
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import { Provider } from 'react-redux';
import { store } from './app/store';
import App from './App';

ReactDOM.createRoot(document.getElementById('root')).render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

#### 4. Use Redux in Components

```jsx
// src/App.js
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement, incrementByAmount } from './features/counter/counterSlice';

function App() {
  const count = useSelector(state => state.counter.value);
  const dispatch = useDispatch();

  return (
    <div>
      <h1>Counter: {count}</h1>

      <button onClick={() => dispatch(decrement())}>-</button>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(incrementByAmount(5))}>+5</button>
    </div>
  );
}

export default App;
```

### Summary

| Part        | Role                                               |
| ----------- | -------------------------------------------------- |
| Slice       | Defines state and actions                          |
| Store       | Combines slices and makes state available app-wide |
| Provider    | Connects React to Redux                            |
| useSelector | Reads data from the store                          |
| useDispatch | Sends actions to the store to change state         |

### Advantages of Redux

- Centralized state
- Predictable updates
- Time-travel debugging
- Middleware support (e.g. for logging, async, etc.)


### Async Redux-thunk

```js
// store.js
import { configureStore, createSlice, createAsyncThunk } from '@reduxjs/toolkit';

// Simulated async API
const fetchUserAPI = () =>
new Promise((resolve) =>setTimeout(() => resolve({ id: 1, name: 'John Doe' }), 1000));

export const fetchUser = createAsyncThunk('user/fetchUser', async () => {
  const response = await fetchUserAPI();
  return response;
});

const userSlice = createSlice({
  name: 'user',
  initialState: {
    user: null,
    status: 'idle',
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchUser.pending, (state) => {
        state.status = 'loading';
      })
      .addCase(fetchUser.fulfilled, (state, action) => {
        state.status = 'succeeded';
        state.user = action.payload;
      })
      .addCase(fetchUser.rejected, (state) => {
        state.status = 'failed';
      });
  },
});

export const store = configureStore({
  reducer: {
    user: userSlice.reducer,
  },
});

// App.js
import React from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { fetchUser } from './store';

function App() {
  const dispatch = useDispatch();
  const { user, status } = useSelector((state) => state.user);
  return (
    <div>
      <button onClick={() => dispatch(fetchUser())}>Fetch User</button>
      {status === 'loading' && <p>Loading...</p>}
      {user && (
        <div><p>ID: {user.id}</p><p>Name: {user.name}</p></div>
      )}
    </div>
  );
}
export default App;

// index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import { Provider } from 'react-redux';
import { store } from './store';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

## 📘 React Functional Components vs Class Components

React components can be created as either **Functional Components** or **Class Components**. Here's a comparison to help you understand the differences.

| Feature                      | Functional Component               | Class Component                                 |
| ---------------------------- | ---------------------------------- | ----------------------------------------------- |
| Syntax                       | Function-based                     | Class-based                                     |
| State Management             | `useState` hook                    | `this.state` and `this.setState()`              |
| Lifecycle Methods            | `useEffect` and other hooks        | `componentDidMount`, `componentDidUpdate`, etc. |
| `this` Keyword               | Not used                           | Required                                        |
| Code Reusability             | Custom hooks                       | Higher-order components (HOC), render props     |
| Readability                  | Concise and cleaner                | More verbose                                    |
| Performance (initial render) | Slightly better (less boilerplate) | Slightly heavier                                |
| Introduced In                | React 16.8 (Hooks)                 | React 0.13+                                     |
| Preferred for New Code       | ✅ Yes                              | ❌ Not recommended for new projects              |

```jsx
import React, { useState, useEffect } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `Count: ${count}`;
  }, [count]);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default Counter;
// class based
import React, { Component } from 'react';

class LifecycleDemo extends Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0,
      showMessage: false,
    };
    console.log('constructor: Initialize state');
  }

  // Runs once, right after the component is inserted into the DOM
  componentDidMount() {
    console.log('componentDidMount: Component mounted');
  }

  // Runs before rendering, after receiving new props or state
  static getDerivedStateFromProps(props, state) {
    console.log('getDerivedStateFromProps: Sync state with props if needed');
    // Return null to indicate no change to state.
    return null;
  }

  // Determines whether component should update (re-render)
  shouldComponentUpdate(nextProps, nextState) {
    console.log('shouldComponentUpdate: Decide to re-render or not');
    return true; // Return false to prevent update
  }

  // Runs right before the DOM is updated, snapshot can be returned and used in componentDidUpdate
  getSnapshotBeforeUpdate(prevProps, prevState) {
    console.log('getSnapshotBeforeUpdate: Capture info before DOM changes');
    return null; // Return value passed to componentDidUpdate as snapshot param
  }

  // Runs after the component updates (re-rendered)
  componentDidUpdate(prevProps, prevState, snapshot) {
    console.log('componentDidUpdate: Component updated');
    if (prevState.count !== this.state.count) {
      document.title = `Count: ${this.state.count}`;
    }
  }

  // Runs right before the component is removed from the DOM
  componentWillUnmount() {
    console.log('componentWillUnmount: Cleanup before unmount');
  }

  // Runs if there is an error during rendering, in a lifecycle method, or in the constructor of any child component
  static getDerivedStateFromError(error) {
    console.log('getDerivedStateFromError: Handle error');
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    console.log('componentDidCatch: Log error details', error, info);
  }

  increment = () => {
    this.setState((prevState) => ({ count: prevState.count + 1 }));
  };

  toggleMessage = () => {
    this.setState((prevState) => ({ showMessage: !prevState.showMessage }));
  };

  render() {
    if (this.state.hasError) {
      return <h2>Something went wrong.</h2>;
    }

    const { count, showMessage } = this.state;

    console.log('render: Render UI');

    return (
      <div>
        <h1>Lifecycle Demo</h1>
        <p>Count: {count}</p>
        <button onClick={this.increment}>Increment</button>
        <button onClick={this.toggleMessage} style={{ marginLeft: '10px' }}>
          {showMessage ? 'Hide' : 'Show'} Message
        </button>

        {showMessage && <p>Hello! This is a simple message.</p>}
      </div>
    );
  }
}

export default LifecycleDemo;

```

## React Router

React Router is a standard library for routing in React applications. It enables navigation among views or components, allows changing the browser URL, and keeps the UI in sync with the URL.

### Key Concepts and Definitions

* **Router**: The core component that keeps the UI and URL in sync. Common routers:

  * `BrowserRouter`: Uses HTML5 history API (recommended for web apps).
  * `HashRouter`: Uses URL hash (for legacy browser support).

* **Route**: Defines a mapping between a URL path and a component to render.

* **Link**: Used to create navigation links without full page reloads.

* **Navigate**: Used for programmatic navigation (redirects).

* **Outlet**: Placeholder component for nested routes.

* **useParams**: Hook to access dynamic URL parameters.

* **useNavigate**: Hook to perform navigation programmatically.

### Installation

```bash
npm install react-router-dom
```

or with Yarn:

```bash
yarn add react-router-dom
```

### Basic Usage Example

```jsx
import React from "react";
import {
  BrowserRouter as Router,
  Routes,
  Route,
  Link
} from "react-router-dom";

function Home() {
  return <h2>Home Page</h2>;
}

function About() {
  return <h2>About Page</h2>;
}

function App() {
  return (
    <Router>
      <nav>
        <Link to="/">Home</Link> |{" "}
        <Link to="/about">About</Link>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </Router>
  );
}

export default App;
```

### Detailed Features

#### 1. Dynamic URL Parameters

Define routes with parameters using `:paramName` syntax.

```jsx
<Route path="/users/:userId" element={<UserProfile />} />
```

Access parameters in the component with `useParams()`:

```jsx
import { useParams } from "react-router-dom";

function UserProfile() {
  const { userId } = useParams();
  return <h2>User ID: {userId}</h2>;
}
```

#### 2. Nested Routes and Outlet

Nested routes allow hierarchical UI.

```jsx
import { Outlet, Link } from "react-router-dom";

function Dashboard() {
  return (
    <div>
      <h2>Dashboard</h2>
      <nav>
        <Link to="stats">Stats</Link> |{" "}
        <Link to="settings">Settings</Link>
      </nav>
      <Outlet /> {/* Renders child routes */}
    </div>
  );
}

function Stats() {
  return <h3>Stats Page</h3>;
}

function Settings() {
  return <h3>Settings Page</h3>;
}

<Routes>
  <Route path="dashboard" element={<Dashboard />}>
    <Route path="stats" element={<Stats />} />
    <Route path="settings" element={<Settings />} />
  </Route>
</Routes>
```

#### 3. Programmatic Navigation

Use the `useNavigate` hook for navigation on events.

```jsx
import { useNavigate } from "react-router-dom";

function Login() {
  const navigate = useNavigate();

  function handleLogin() {
    // Perform login logic
    navigate("/dashboard");
  }

  return <button onClick={handleLogin}>Login</button>;
}
```

#### 4. Redirects with Navigate

Redirect users conditionally.

```jsx
import { Navigate } from "react-router-dom";

function ProtectedRoute({ isLoggedIn, children }) {
  if (!isLoggedIn) {
    return <Navigate to="/login" replace />;
  }
  return children;
}

// Usage in routes
<Route
  path="/dashboard"
  element={
    <ProtectedRoute isLoggedIn={userLoggedIn}>
      <Dashboard />
    </ProtectedRoute>
  }
/>
```

#### 5. Not Found (404) Route

Catch all unmatched routes:

```jsx
<Route path="*" element={<h2>404 Page Not Found</h2>} />
```

### Useful Hooks Summary

| Hook          | Description                  |
| ------------- | ---------------------------- |
| `useParams`   | Access route params          |
| `useNavigate` | Navigate programmatically    |
| `useLocation` | Get current location object  |
| `useMatch`    | Match the current route path |

### Summary

React Router enables seamless client-side routing in React apps with declarative routes, nested layouts, and hooks for advanced control. Use it to create SPA navigation without full page reloads.

### Official Documentation

[React Router Documentation](https://reactrouter.com/en/main)

*This guide covers basic to intermediate usage of React Router v6.*
