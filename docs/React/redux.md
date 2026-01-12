---
id: redux
title: Redux
sidebar_position: 12
-------------------

# Redux

> Comprehensive guide to Redux for state management in React applications

---

## Core Concepts

| Concept  | Description                                                  |
| -------- | ------------------------------------------------------------ |
| Store    | Single source of truth holding the state of the app          |
| Action   | Plain object describing what happened (type + payload)       |
| Reducer  | Pure function that takes state and action, returns new state |
| Dispatch | Sends an action to the store to update state                 |
| Selector | Function to read specific part of state from the store       |

---

## Installation

```bash
npm install redux react-redux
```

---

## Creating a Store

<details>
<summary>Example: Store</summary>

```js
import { createStore } from 'redux';

const initialState = { count: 0 };

function counterReducer(state = initialState, action) {
  switch(action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + 1 };
    case 'DECREMENT':
      return { ...state, count: state.count - 1 };
    default:
      return state;
  }
}

const store = createStore(counterReducer);

store.dispatch({ type: 'INCREMENT' });
console.log(store.getState()); // { count: 1 }
```

</details>

---

## React-Redux Integration

| Hook / HOC    | Purpose                                                               |
| ------------- | --------------------------------------------------------------------- |
| `<Provider>`  | Wraps the app and provides store to components                        |
| `useSelector` | Access state from the Redux store in function components              |
| `useDispatch` | Get dispatch function to send actions                                 |
| `connect`     | HOC to map state and dispatch to props (class or function components) |

<details>
<summary>Example: React-Redux Hooks</summary>

```js
import React from 'react';
import { Provider, useSelector, useDispatch } from 'react-redux';
import { createStore } from 'redux';

const initialState = { count: 0 };
function reducer(state = initialState, action) {
  switch(action.type) {
    case 'INCREMENT': return { count: state.count + 1 };
    default: return state;
  }
}

const store = createStore(reducer);

function Counter() {
  const count = useSelector(state => state.count);
  const dispatch = useDispatch();

  return <button onClick={() => dispatch({ type: 'INCREMENT' })}>Count: {count}</button>;
}

function App() {
  return (
    <Provider store={store}>
      <Counter />
    </Provider>
  );
}
```

</details>

---

## Middleware (Redux Thunk Example)

Middleware adds custom logic during dispatch, often used for async operations.

<details>
<summary>Example: Async Action with Thunk</summary>

```js
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';

const initialState = { data: null };
function reducer(state = initialState, action) {
  switch(action.type) {
    case 'SET_DATA': return { ...state, data: action.payload };
    default: return state;
  }
}

const store = createStore(reducer, applyMiddleware(thunk));

function fetchData() {
  return async dispatch => {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos/1');
    const data = await response.json();
    dispatch({ type: 'SET_DATA', payload: data });
  };
}

store.dispatch(fetchData());
```

</details>

---

## Best Practices

| Recommendation           | Notes                                                        |
| ------------------------ | ------------------------------------------------------------ |
| Keep state normalized    | Avoid nested objects for easier updates and selectors        |
| Use selectors            | Memoized selectors (reselect) prevent unnecessary re-renders |
| Use middleware for async | Redux Thunk or Redux Saga for side effects                   |
| Split reducers           | Combine reducers for modularity and maintainability          |
| Immutable updates        | Never mutate state directly; always return new objects       |

---

## Notes & Caveats

* Redux is most useful for medium to large apps with complex state.
* Hooks (`useSelector`, `useDispatch`) are recommended over `connect` for new code.
* Keep actions and reducers simple and predictable.
* Avoid storing non-serializable values in the store (e.g., functions, class instances).
