---
id: performance-and-optimization
title: Performance And Optimization
sidebar_position: 7
-------------------

# Performance & Optimization

> Techniques to optimize React components and prevent unnecessary re-renders

---

## React.memo

`React.memo` is a HOC that memoizes a component, preventing re-renders if props haven't changed.

<details>
<summary>Example: React.memo</summary>

```js
import React from 'react';

const Child = React.memo(function Child({ name }) {
  console.log('Child rendered');
  return <p>Hello, {name}</p>;
});

function App() {
  const [count, setCount] = React.useState(0);
  return (
    <>
      <Child name="Alice" />
      <button onClick={() => setCount(c => c + 1)}>Increment: {count}</button>
    </>
  );
}
```

</details>

---

## useCallback

`useCallback` memoizes a function to prevent unnecessary re-creations on re-renders.

<details>
<summary>Example: useCallback</summary>

```js
import { useState, useCallback } from 'react';

function Button({ onClick, label }) {
  console.log('Button rendered');
  return <button onClick={onClick}>{label}</button>;
}

function App() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => setCount(c => c + 1), []);

  return <Button onClick={handleClick} label={`Count: ${count}`} />;
}
```

</details>

---

## useMemo

`useMemo` memoizes a value or calculation to avoid expensive recalculations on every render.

<details>
<summary>Example: useMemo</summary>

```js
import { useState, useMemo } from 'react';

function App({ numbers }) {
  const [multiplier, setMultiplier] = useState(2);

  const doubled = useMemo(() => {
    console.log('Calculating doubled');
    return numbers.map(n => n * multiplier);
  }, [numbers, multiplier]);

  return (
    <>
      <p>{doubled.join(', ')}</p>
      <button onClick={() => setMultiplier(m => m + 1)}>Increase Multiplier</button>
    </>
  );
}
```

</details>

---

## Performance Best Practices

| Technique                                            | Purpose                                                                |
| ---------------------------------------------------- | ---------------------------------------------------------------------- |
| Memoization (`React.memo`, `useMemo`, `useCallback`) | Prevent unnecessary re-renders or recalculations                       |
| Splitting Components                                 | Smaller components re-render independently                             |
| Avoid Inline Functions/Objects                       | Inline values create new references on each render                     |
| Lazy Loading                                         | Load components or modules only when needed (`React.lazy`, `Suspense`) |
| Key Prop for Lists                                   | Proper keys prevent re-rendering entire lists                          |
| Batching State Updates                               | Group multiple state changes to reduce renders                         |
| Avoid Heavy Calculations in Render                   | Move to memoized functions or effects                                  |

---

## Notes & Caveats

* `React.memo` only does shallow comparison of props; for deep objects, consider `useMemo`.
* Overusing memoization can increase complexity; profile before optimization.
* `useCallback` and `useMemo` are only beneficial when their dependencies are stable and recalculation is expensive.
* Lazy load routes and components to reduce init
