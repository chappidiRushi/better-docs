---
id: under-the-hood
title: Under The Hood
sidebar_position: 4
-------------------

# How React Works Under the Hood

> Step-by-step explanation of React's internal mechanisms for rendering and updating UIs

---

## React Rendering Flow Diagram (Markdown Table)

| Step                                | Description                                                                 |
| ----------------------------------- | --------------------------------------------------------------------------- |
| JSX Compilation                     | JSX is compiled into `React.createElement` calls creating Virtual DOM nodes |
| Virtual DOM Creation                | React builds an in-memory representation of the UI                          |
| Initial Rendering                   | React mounts Virtual DOM to real DOM using `createRoot().render`            |
| State/Props Changes                 | Any state or props change marks a component for re-rendering                |
| Reconciliation (Diffing)            | React compares new Virtual DOM with previous to compute minimal updates     |
| Fiber Architecture & Prioritization | Updates are split into units with high-priority work processed first        |
| Commit Phase                        | Changes are applied to the real DOM only where needed                       |
| Effects & Lifecycle Hooks           | useEffect and other hooks run after DOM updates                             |
| Event Delegation                    | Single root listener handles events efficiently                             |

---

## Step 1: JSX Compilation

React JSX is compiled into `React.createElement` calls, creating a Virtual DOM representation.

<details>
<summary>Example</summary>

```js
const element = <h1>Hello</h1>;
// Compiled to:
const elementCompiled = React.createElement('h1', null, 'Hello');
```

</details>

---

## Step 2: Virtual DOM Creation

React maintains an in-memory tree of UI elements, the Virtual DOM, representing the UI state at any point.

<details>
<summary>Example</summary>

```js
const vDom = React.createElement('div', null, 'Hello');
```

</details>

---

## Step 3: Initial Rendering

React mounts the Virtual DOM to the real DOM for the first time using `ReactDOM.render` or `createRoot().render`.

<details>
<summary>Example</summary>

```js
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

</details>

---

## Step 4: State/Props Changes

When a component's state or props change, React marks the component as needing an update and triggers re-rendering.

<details>
<summary>Example</summary>

```js
const [count, setCount] = useState(0);
setCount(count + 1); // triggers re-render
```

</details>

---

## Step 5: Reconciliation (Diffing)

React compares the new Virtual DOM with the previous one to compute the minimal set of changes required for the real DOM.

<details>
<summary>Example</summary>

```js
const oldVDom = <div>1</div>;
const newVDom = <div>2</div>;
// React updates only the text node
```

</details>

---

## Step 6: Fiber Architecture & Prioritization

React Fiber breaks updates into units of work, allowing high-priority updates (like user input) to interrupt low-priority tasks for smooth rendering.

<details>
<summary>Example</summary>

```js
// High priority: typing input
// Low priority: animation frame updates
```

</details>

---

## Step 7: Commit Phase

After diffing, React applies changes to the real DOM in the commit phase, updating only the affected nodes.

<details>
<summary>Example</summary>

```js
// React updates text nodes, attributes, or adds/removes elements as needed
```

</details>

---

## Step 8: Effects & Lifecycle Hooks

React runs useEffect, useLayoutEffect, and other lifecycle-related hooks after DOM updates to handle side effects.

<details>
<summary>Example</summary>

```js
useEffect(() => {
  console.log('DOM updated');
}, [count]);
```

</details>

---

## Step 9: Event Delegation

React attaches a single listener to the root and delegates events to components efficiently.

<details>
<summary>Example</summary>

```js
<button onClick={() => console.log('clicked')}>Click</button>
// Handled through React's event delegation system
```

</details>

---

## Notes & Caveats

* Virtual DOM minimizes direct DOM manipulation.
* Fiber enables incremental rendering and task prioritization.
* State and hooks are tracked internally per fiber.
* Event delegation reduces memory usage and improves performance.
