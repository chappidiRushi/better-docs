---
id: error-handling-and-edge-features
title: Error Handling & Edge Features
sidebar_position: 9
-------------------

# Error Handling & Edge Features

> Techniques and features for handling errors and advanced rendering patterns

---

## Error Boundaries

Error boundaries catch JavaScript errors in child components and display a fallback UI.

<details>
<summary>Example: Error Boundary</summary>

```js
import React from 'react';

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    console.error('Error caught:', error, info);
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}

function BuggyComponent() {
  throw new Error('Oops');
}

function App() {
  return (
    <ErrorBoundary>
      <BuggyComponent />
    </ErrorBoundary>
  );
}
```

</details>

---

## Portals

Portals allow rendering children into a DOM node outside the parent component hierarchy.

<details>
<summary>Example: Portal</summary>

```js
import React from 'react';
import ReactDOM from 'react-dom';

function Modal({ children }) {
  return ReactDOM.createPortal(
    <div className="modal">{children}</div>,
    document.getElementById('modal-root')
  );
}

function App() {
  return <Modal>Hello in Portal!</Modal>;
}
```

</details>

---

## Suspense

`Suspense` lets you display a fallback UI while waiting for components or data to load.

<details>
<summary>Example: Suspense</summary>

```js
import React, { Suspense } from 'react';

const LazyComponent = React.lazy(() => import('./LazyComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <LazyComponent />
    </Suspense>
  );
}
```

</details>

---

## Lazy Loading (`React.lazy`)

`React.lazy` allows components to be loaded lazily, reducing initial bundle size.

<details>
<summary>Example: React.lazy</summary>

```js
import React, { Suspense } from 'react';

const LazyComponent = React.lazy(() => import('./LazyComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <LazyComponent />
    </Suspense>
  );
}
```

</details>

---

## Notes & Best Practices

* Error boundaries catch errors in rendering, lifecycle methods, and constructors of child components.
* Portals are useful for modals, tooltips, and overlays that should escape parent styles.
* Always provide a `fallback` for Suspense to improve UX.
* Lazy loading helps reduce initial bundle size; combine with Suspense for smooth loading.
* Error boundaries do not catch errors in event handlers; handle those separately.
