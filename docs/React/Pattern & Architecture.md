---
id: pattern-and-architecture
title: Pattern And Architecture
sidebar_position: 8
-------------------
# Patterns & Architecture

> Common patterns and architectural approaches for building scalable and maintainable React applications

---

## Higher-Order Components (HOC)

A HOC is a function that takes a component and returns a new enhanced component.

```js
import React from 'react';

function withLogger(WrappedComponent) {
  return function (props) {
    console.log('Rendering', WrappedComponent.name);
    return <WrappedComponent {...props} />;
  };
}

function Button({ label }) {
  return <button>{label}</button>;
}

const ButtonWithLogger = withLogger(Button);

function App() {
  return <ButtonWithLogger label="Click Me" />;
}
```

---

## Render Props

A render prop is a function prop that a component uses to know what to render.

```js
function MouseTracker({ render }) {
  const [x, setX] = React.useState(0);
  const [y, setY] = React.useState(0);

  const handleMouseMove = e => {
    setX(e.clientX);
    setY(e.clientY);
  };

  return <div onMouseMove={handleMouseMove}>{render({ x, y })}</div>;
}

function App() {
  return (
    <MouseTracker render={({ x, y }) => <p>Mouse at ({x}, {y})</p>} />
  );
}
```

---

## Container / Presentational Pattern

Separates logic and data fetching (container) from UI rendering (presentational).

```js
// Presentational
function UserList({ users }) {
  return <ul>{users.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
}

// Container
function UserListContainer() {
  const [users, setUsers] = React.useState([]);

  React.useEffect(() => {
    fetch('/api/users').then(res => res.json()).then(setUsers);
  }, []);

  return <UserList users={users} />;
}
```

---

## Component Composition Patterns

Combining smaller components to build complex UIs. Can include slots, children props, or flexible APIs.

```js
function Card({ title, children }) {
  return (
    <div className="card">
      <h2>{title}</h2>
      {children}
    </div>
  );
}

function App() {
  return (
    <Card title="My Card">
      <p>This is content inside the card.</p>
      <button>Click</button>
    </Card>
  );
}
```

---

## Other Important Patterns

| Pattern                   | Description                                                                   |
| ------------------------- | ----------------------------------------------------------------------------- |
| Compound Components       | Components communicate implicitly via context to allow flexible composition   |
| Controlled/Uncontrolled   | Parent manages state (controlled) vs component manages its own (uncontrolled) |
| Hooks & Custom Hooks      | Encapsulate reusable logic for cleaner architecture                           |
| Provider/Consumer Pattern | Used with context to manage global state or dependencies                      |

---

## Notes & Caveats

* HOCs can add layers of nesting; prefer hooks and composition when possible.
* Render props are flexible but may lead to nested functions (prop drilling within children).
* Container/Presentational pattern helps separation of concerns and improves testability.
* Use composition over inheritance for flexible and maintainable React components.
