---
id: introductions
title: Introduction
sidebar_position: 3
-------------------



# Introduction & Basics

> Core concepts and building blocks of React

---

## What React Is

React is a declarative, component-based library for building UIs. It lets you break the UI into reusable components.

<details>
<summary>Examples</summary>

```js
import React from 'react';
import ReactDOM from 'react-dom/client';

function App() {
  return <h1>Hello React!</h1>;
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

</details>

---

## JSX

JSX is a syntax to write HTML-like code in JavaScript. React compiles it into `createElement` calls.

<details>
<summary>Examples</summary>

```js
const name = 'React';
const element = <h1>Hello, {name}!</h1>;   // JSX with expressions
const list = <ul>{['A', 'B', 'C'].map(item => <li key={item}>{item}</li>)}</ul>; // JSX with map
```

</details>

---

## Rendering & Re-rendering

Rendering inserts components into the DOM. Re-rendering updates the DOM efficiently when state or props change.

<details>
<summary>Examples</summary>

```js
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

</details>

---

## Strict Mode

StrictMode detects potential problems and highlights unsafe patterns. It runs only in development mode.

<details>
<summary>Examples</summary>

```js
import React from 'react';
import ReactDOM from 'react-dom/client';

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

</details>

---

## Fragments

Fragments let you return multiple elements without extra DOM nodes. Useful to group elements without adding `<div>` wrappers.

<details>
<summary>Examples</summary>

```js
function List() {
  return (
    <>
      <li>Item 1</li>
      <li>Item 2</li>
      <li>Item 3</li>
    </>
  );
}
```

</details>

---

## Notes & Caveats

* JSX must have one root element per expression.
* StrictMode runs checks only in development, not production.
* Fragments can use `<></>` shorthand or `<React.Fragment>`.
* Re-rendering occurs only when state or props change.
