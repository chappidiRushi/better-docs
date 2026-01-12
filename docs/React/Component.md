---
id: component
title: Component
sidebar_position: 4
-------------------
# React Components Cheat Sheet

> Essential concepts and patterns for building React applications

---

## Components

Components are reusable UI building blocks in React. They can be function-based or class-based (modern React prefers function components).

<details>
<summary>Example</summary>

```js
function App() {
  return <h1>Hello World</h1>;
}
```

</details>

---

## Function Components

A function component is a JavaScript function that returns JSX.

<details>
<summary>Example</summary>

```js
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}
```

</details>

---

## Component Composition

Components can be composed by nesting or passing them as children to build complex UIs.

<details>
<summary>Example</summary>

```js
function Button({ children }) {
  return <button>{children}</button>;
}

function App() {
  return (
    <div>
      <Button>Click Me</Button>
      <Button>Submit</Button>
    </div>
  );
}
```

</details>

---

## Props

Props are inputs to components used to pass data and callbacks.

<details>
<summary>Example</summary>

```js
function User({ name, age }) {
  return <p>{name} is {age} years old.</p>;
}

<User name="Alice" age={30} />
```

</details>

---

## State (`useState`)

State stores component-specific data and triggers re-renders when updated.

<details>
<summary>Example</summary>

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

## Controlled vs Uncontrolled Components

Controlled components have React state as the source of truth; uncontrolled components rely on DOM values.

<details>
<summary>Example</summary>

```js
// Controlled
function ControlledInput() {
  const [value, setValue] = useState('');
  return <input value={value} onChange={e => setValue(e.target.value)} />;
}

// Uncontrolled
function UncontrolledInput() {
  return <input defaultValue="Hello" />;
}
```

</details>

---

## Lifting State Up

State is moved to a common parent component to share data between siblings.

<details>
<summary>Example</summary>

```js
function Parent() {
  const [count, setCount] = useState(0);
  return (
    <>
      <Child1 count={count} />
      <Child2 onIncrement={() => setCount(count + 1)} />
    </>
  );
}
```

</details>

---

## Lists & Keys

React requires a `key` prop for elements in a list to track changes efficiently.

<details>
<summary>Example</summary>

```js
const items = ['A', 'B', 'C'];
function List() {
  return (
    <ul>
      {items.map(item => <li key={item}>{item}</li>)}
    </ul>
  );
}
```

</details>

---

## Conditional Rendering

Render elements conditionally using ternaries, logical operators, or `if` statements.

<details>
<summary>Example</summary>

```js
function Greeting({ isLoggedIn }) {
  return (
    <div>
      {isLoggedIn ? <p>Welcome back!</p> : <p>Please login</p>}
    </div>
  );
}
```

</details>

---

## Event Handling

React uses camelCase props and passes event handlers as functions.

<details>
<summary>Example</summary>

```js
function Clicker() {
  function handleClick() {
    console.log('Button clicked');
  }
  return <button onClick={handleClick}>Click Me</button>;
}
```

</details>

---

## Notes & Caveats

* Props are immutable; state is mutable and local to the component.
* Always provide unique keys when rendering lists.
* Lifting state up helps share data but can increase complexity.
* Controlled components are preferred for predictable forms.
