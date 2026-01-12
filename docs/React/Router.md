---
id: router
title: Router
sidebar_position: 13
-------------------

# React Router DOM Cheat Sheet

> Complete guide for client-side routing in React using react-router-dom

---

## Installation

```bash
npm install react-router-dom
```

---

## Core Components

| Component         | Purpose                                                         |
| ----------------- | --------------------------------------------------------------- |
| `<BrowserRouter>` | Wraps the app and enables HTML5 history API for routing         |
| `<Routes>`        | Container for all route definitions (replaces `<Switch>` in v6) |
| `<Route>`         | Defines a route and the component to render                     |
| `<Link>`          | Navigational link that changes URL without full page reload     |
| `<NavLink>`       | Like Link, but applies active styles when route matches         |
| `<Outlet>`        | Placeholder for nested routes                                   |
| `<Navigate>`      | Programmatic redirect to another route                          |

---

## Basic Routing Example

<details>
<summary>Example: Basic Routes</summary>

```js
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

function Home() { return <h2>Home</h2>; }
function About() { return <h2>About</h2>; }
function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link> | <Link to="/about">About</Link>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
```

</details>

---

## Nested Routes

<details>
<summary>Example: Nested Routes with Outlet</summary>

```js
import { BrowserRouter, Routes, Route, Outlet, Link } from 'react-router-dom';

function Dashboard() {
  return (
    <div>
      <h2>Dashboard</h2>
      <nav>
        <Link to="stats">Stats</Link> | <Link to="settings">Settings</Link>
      </nav>
      <Outlet />
    </div>
  );
}

function Stats() { return <p>Stats Content</p>; }
function Settings() { return <p>Settings Content</p>; }

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="dashboard" element={<Dashboard />}>
          <Route path="stats" element={<Stats />} />
          <Route path="settings" element={<Settings />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}
```

</details>

---

## Programmatic Navigation

<details>
<summary>Example: Navigate Programmatically</summary>

```js
import { useNavigate } from 'react-router-dom';

function Home() {
  const navigate = useNavigate();
  return <button onClick={() => navigate('/about')}>Go to About</button>;
}
```

</details>

---

## Route Parameters

<details>
<summary>Example: Route Parameters</summary>

```js
import { useParams } from 'react-router-dom';

function User() {
  const { id } = useParams();
  return <h2>User ID: {id}</h2>;
}

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="user/:id" element={<User />} />
      </Routes>
    </BrowserRouter>
  );
}
```

</details>

---

## Redirects & 404 Pages

<details>
<summary>Example: Redirect and 404</summary>

```js
import { Routes, Route, Navigate } from 'react-router-dom';

function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/old" element={<Navigate to="/" />} />
      <Route path="*" element={<h2>404 Not Found</h2>} />
    </Routes>
  );
}
```

</details>

---

## Best Practices

| Practice                                       | Notes                                      |
| ---------------------------------------------- | ------------------------------------------ |
| Use `<Routes>` instead of `<Switch>` in v6     | Updated API simplifies route handling      |
| Prefer `Link` over anchor tags                 | Avoid full page reloads                    |
| Keep routes flat if possible                   | Simplifies URL structure and routing logic |
| Use nested routes for related content          | Cleaner layout and code reuse              |
| Handle 404 routes at the end                   | Catch unmatched routes                     |
| Memoize components rendered in routes if heavy | Prevent unnecessary re-renders             |

---

## Notes & Caveats

* React Router v6 introduced `element` prop instead of `component`.
* All route components must be wrapped in `<BrowserRouter>` or `<HashRouter>`.
* Route matching is now exact by default in v6.
* Use `Outlet` for nested routes instead of rendering children manually.
