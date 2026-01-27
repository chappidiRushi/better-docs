---

id: patterns-and-architecture
title: Patterns and Architecture
sidebar_position: 8
-------------------

# Patterns & Architecture

> Common patterns and architectural approaches for building **scalable, maintainable, and predictable** React applications.

This document focuses on **design intent**, trade-offs, and when each pattern should (or should not) be used.

---

## Higher-Order Components (HOC)

A **Higher-Order Component** is a function that takes a component and returns a new component with enhanced behavior.

**Key characteristics**:

* Logic reuse through wrapping
* Props are passed through to the wrapped component
* Common in pre-hooks React

**When to use**:

* Cross-cutting concerns (logging, permissions)
* Legacy codebases

**When to avoid**:

* New code (prefer hooks)
* Deep wrapper chains ("wrapper hell")

<details>
<summary>Example: HOC</summary>

```js
function withLogger(WrappedComponent) {
  return function LoggerWrapper(props) {
    console.log('Rendering', WrappedComponent.name);
    return <WrappedComponent {...props} />;
  };
}
```

</details>

---

## Render Props Pattern

A **render prop** is a function passed as a prop that determines what a component renders.

**Key idea**:

> Invert control of rendering while sharing logic.

**Pros**:

* Very flexible
* Explicit data flow

**Cons**:

* Can lead to deeply nested JSX
* Harder to read at scale

<details>
<summary>Example: Render Props</summary>

```js
function MouseTracker({ render }) {
  const [pos, setPos] = React.useState({ x: 0, y: 0 });

  return (
    <div onMouseMove={e => setPos({ x: e.clientX, y: e.clientY })}>
      {render(pos)}
    </div>
  );
}
```

</details>

---

## Container / Presentational Pattern

Separates **data & logic** from **UI rendering**.

* **Container**: state, side effects, data fetching
* **Presentational**: pure UI, props only

**Benefits**:

* Easier testing
* Clear separation of concerns

**Modern note**:

* Often replaced by hooks, but still useful conceptually

<details>
<summary>Example: Container vs Presentational</summary>

```js
function UserList({ users }) {
  return <ul>{users.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
}

function UserListContainer() {
  const [users, setUsers] = React.useState([]);

  React.useEffect(() => {
    fetch('/api/users').then(r => r.json()).then(setUsers);
  }, []);

  return <UserList users={users} />;
}
```

</details>

---

## Component Composition

Composition builds complex UIs from **small, reusable pieces**.

**Core principle**:

> Prefer composition over inheritance.

Common techniques:

* `children`
* Slot props
* Conditional composition

<details>
<summary>Example: Composition</summary>

```js
function Card({ title, children }) {
  return (
    <div className="card">
      <h2>{title}</h2>
      {children}
    </div>
  );
}
```

</details>

---

## Compound Components Pattern

Multiple components share state implicitly via context.

**Benefits**:

* Flexible APIs
* Declarative usage

**Used by**: Radix UI, Reach UI

<details>
<summary>Example: Compound Components</summary>

```js
const TabsContext = React.createContext();

function Tabs({ children }) {
  const [active, setActive] = React.useState(0);
  return (
    <TabsContext.Provider value={{ active, setActive }}>
      {children}
    </TabsContext.Provider>
  );
}
```

</details>

---

## Controlled vs Uncontrolled Components

| Pattern      | Description       | Example                                |
| ------------ | ----------------- | -------------------------------------- |
| Controlled   | Parent owns state | `<input value={x} onChange={...} />`   |
| Uncontrolled | DOM owns state    | `<input defaultValue="x" ref={ref} />` |

**Rule of thumb**:

* Forms → controlled
* Simple inputs / perf-critical → uncontrolled

---

## Provider / Dependency Injection Pattern

Uses Context to inject dependencies (theme, auth, services).

**Good for**:

* Global config
* App-wide services

**Avoid**:

* High-frequency changing values

---

## Feature-Based Architecture (Recommended)

Organize code by **feature**, not by type.

```txt
features/
 └─ auth/
    ├─ components/
    ├─ hooks/
    ├─ api.ts
    └─ slice.ts
```

**Benefits**:

* Scales well
* Clear ownership
* Easier refactoring

---

## State Ownership Rules

Ask:

1. Who needs this state?
2. How often does it change?
3. Is it derived or source-of-truth?

Lift state **only as high as necessary**.

---

## Anti-Patterns to Avoid

* God components
* Deep prop drilling without reason
* Overusing context
* Premature abstractions
* Tight coupling between components

---

## Key Takeaways

* Prefer **hooks + composition** over HOCs
* Architecture is about **intent**, not files
* Patterns are tools — not rules
* Optimize for readability first, performance second
