---

id: redux
title: Redux
sidebar_position: 12
--------------------

# Redux

> A **simple, modern reference** for using Redux with React via **Redux Toolkit**.

Redux is a **state management library** for JavaScript applications. It is mainly used to manage **global state** — state that many components need to access and update.

Use Redux when:

* Many components need the same data (auth, theme, user info)
* State updates are complex or follow strict rules
* You want predictable, debuggable state changes

---

## Core Concepts

| Concept  | Description                                      |
| -------- | ------------------------------------------------ |
| Store    | Holds the entire application state               |
| Action   | An object describing what happened               |
| Reducer  | A function that updates state based on an action |
| Dispatch | Sends actions to the store                       |
| Selector | Reads specific data from the store               |

---

## Modern Redux (Redux Toolkit)

Redux Toolkit (RTK) is the **official and recommended** way to use Redux.

Install:

```bash
npm install @reduxjs/toolkit react-redux
```

---

## Example: Counter App (Step by Step)

### 1. Create a Redux Slice

A **slice** defines:

* Initial state
* Reducer logic
* Auto‑generated actions

<details>
<summary>counterSlice.js</summary>

```js
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: state => {
      state.value += 1;
    },
    decrement: state => {
      state.value -= 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    },
  },
});

export const { increment, decrement, incrementByAmount } = counterSlice.actions;
export default counterSlice.reducer;
```

</details>

---

### 2. Set Up the Redux Store

The store combines all slices and makes state available globally.

<details>
<summary>store.js</summary>

```js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../features/counter/counterSlice';

export const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});
```

</details>

---

### 3. Provide the Store to React

Wrap your app with `<Provider>` so components can access the store.

<details>
<summary>index.js</summary>

```js
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

</details>

---

### 4. Use Redux in Components

Use hooks to read state and dispatch actions.

<details>
<summary>App.js</summary>

```js
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

</details>

---

## Async Logic with createAsyncThunk

Redux Toolkit includes built‑in support for async logic.

<details>
<summary>Example: Async User Fetch</summary>

```js
import { configureStore, createSlice, createAsyncThunk } from '@reduxjs/toolkit';

const fetchUserAPI = () =>
  new Promise(resolve =>
    setTimeout(() => resolve({ id: 1, name: 'John Doe' }), 1000)
  );

export const fetchUser = createAsyncThunk('user/fetchUser', async () => {
  return await fetchUserAPI();
});

const userSlice = createSlice({
  name: 'user',
  initialState: { user: null, status: 'idle' },
  extraReducers: builder => {
    builder
      .addCase(fetchUser.pending, state => {
        state.status = 'loading';
      })
      .addCase(fetchUser.fulfilled, (state, action) => {
        state.status = 'succeeded';
        state.user = action.payload;
      })
      .addCase(fetchUser.rejected, state => {
        state.status = 'failed';
      });
  },
});

export const store = configureStore({
  reducer: {
    user: userSlice.reducer,
  },
});
```

</details>

---

## Summary

| Part        | Role                       |
| ----------- | -------------------------- |
| Slice       | Defines state and reducers |
| Store       | Holds global state         |
| Provider    | Connects Redux to React    |
| useSelector | Reads state                |
| useDispatch | Updates state              |

---

## Developer Experience (DX)

Redux is known for its **excellent developer experience**, especially when paired with Redux Toolkit.

### Redux DevTools

Redux DevTools allow you to:

* Inspect current state
* See every dispatched action
* Time‑travel (undo / redo state changes)
* Debug complex state transitions

Redux Toolkit enables DevTools automatically in development.

---

### Predictability & Debugging

Redux enforces:

* Unidirectional data flow
* Explicit actions
* Pure reducers

This makes bugs easier to trace and fixes more confident.

---

## Selectors & Performance

Selectors encapsulate how state is read.

```js
const selectCount = state => state.counter.value;
```

Benefits:

* Centralized state access
* Easier refactoring
* Better performance

For expensive derived data, use **memoized selectors** (`reselect`).

---

## Async Patterns & Side Effects

Preferred async approaches:

| Tool             | Use Case                                  |
| ---------------- | ----------------------------------------- |
| createAsyncThunk | Simple async requests                     |
| RTK Query        | Server state (fetching, caching, polling) |

Avoid manually handling loading/error state when RTK Query fits.

---

## Advantages of Redux

* Centralized state
* Predictable updates
* Excellent DevTools (time‑travel debugging)
* Testable business logic
* Scales well for large applications

---

## When NOT to Use Redux

Avoid Redux when:

* State is local to a single component
* App is small or simple
* Context or component state is sufficient

---

## Key Takeaways

* Redux Toolkit is the **standard way** to use Redux
* Use slices to reduce boilerplate
* Keep Redux for **shared, global, complex state**
* Prefer simplicity over over‑engineering
