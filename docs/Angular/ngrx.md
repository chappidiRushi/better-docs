---
id: ngrx
title: NgRx (State Management)
sidebar_position: 23
---

# NgRx (State Management)

> **NgRx** is a reactive state management library for Angular apps based on Redux principles — a **single source of truth, immutable state, unidirectional data flow, and RxJS streams**.

This guide assumes a **mid‑level Angular dev** and explains **what each piece does — concisely — with foldable examples**.

---

## 🧠 Core Concepts

* **Store** — global reactive state container
* **Actions** — plain objects that describe *what happened*
* **Reducers** — pure functions that *update state* based on actions
* **Selectors** — queries that read state reactively
* **Effects** — side‑effects handler (e.g., HTTP)
* **Entity** — helpers for normalized collections
* **ComponentStore** — local feature‑level store
* **Router Store** — sync router state with Store

---

## 🚀 Setup (Angular 21 + NgRx latest)

Install packages:

```bash
ng add @ngrx/store@latest
ng add @ngrx/effects@latest
ng add @ngrx/store-devtools@latest
ng add @ngrx/entity@latest
ng add @ngrx/router-store@latest
```

Register store in `main.ts` or bootstrap file:

<details>
<summary>Example</summary>

```ts
import { bootstrapApplication } from '@angular/platform-browser';
import { AppComponent } from './app/app.component';
import { provideStore } from '@ngrx/store';
import { provideEffects } from '@ngrx/effects';
import { provideStoreDevtools } from '@ngrx/store-devtools';

bootstrapApplication(AppComponent, {
  providers: [
    provideStore(),              // root store (empty for now)
    provideEffects(),            // root effects
    provideStoreDevtools()       // Redux DevTools
  ]
});
```

</details>

---

## 🧩 Defining Feature State

> A **feature** isolates a slice of global state.

<details>
<summary>Example</summary>

```ts
// counter.reducer.ts
import { createReducer, on } from '@ngrx/store';
import { createAction, props } from '@ngrx/store';

// Actions — describe events
export const increment = createAction('[Counter] Increment');
export const decrement = createAction('[Counter] Decrement');
export const set = createAction('[Counter] Set', props<{ value: number }>());

// State model
export interface CounterState {
  value: number;
}

const initialState: CounterState = { value: 0 };

// Reducer — pure state update logic
export const counterReducer = createReducer(
  initialState,
  on(increment, state => ({ value: state.value + 1 })),
  on(decrement, state => ({ value: state.value - 1 })),
  on(set, (state, { value }) => ({ value }))
);
```

</details>

Register the feature:

<details>
<summary>Example</summary>

```ts
import { provideState } from '@ngrx/store';
import { counterReducer } from './counter.reducer';

providers: [
  provideState('counter', counterReducer)
]
```

</details>

---

## 🎯 Actions (What Happened)

> **Actions are plain objects** with a `type` and optional payload.

<details>
<summary>Example</summary>

```ts
export const loadUsers = createAction('[Users] Load Users');
export const loadUsersSuccess = createAction(
  '[Users] Load Users Success',
  props<{ users: User[] }>()
);
export const loadUsersFailure = createAction(
  '[Users] Load Users Failure',
  props<{ error: unknown }>()
);
```

</details>

---

## 🧮 Reducers (How State Changes)

> **Pure functions** → no side effects, no async.

<details>
<summary>Example</summary>

```ts
export const usersReducer = createReducer(
  { users: [], loading: false },
  on(loadUsers, s => ({ ...s, loading: true })),
  on(loadUsersSuccess, (s, { users }) => ({ ...s, users, loading: false })),
  on(loadUsersFailure, s => ({ ...s, loading: false }))
);
```

</details>

---

## 🔍 Selectors (Read Data Reactively)

> Query state in a **memoized**, performant way.

<details>
<summary>Example</summary>

```ts
import { createSelector, createFeatureSelector } from '@ngrx/store';

const selectUsersFeature = createFeatureSelector<any>('users');

export const selectUsers = createSelector(
  selectUsersFeature,
  s => s.users
);

export const selectUsersLoading = createSelector(
  selectUsersFeature,
  s => s.loading
);
```

</details>

Use in a component:

<details>
<summary>Example</summary>

```ts
import { Component, inject } from '@angular/core';
import { Store } from '@ngrx/store';
import { selectUsers, selectUsersLoading, loadUsers } from './state';

@Component({ standalone: true, template: `` })
export class UsersComponent {
  private store = inject(Store);

  users$ = this.store.select(selectUsers);
  loading$ = this.store.select(selectUsersLoading);

  ngOnInit() {
    this.store.dispatch(loadUsers());
  }
}
```

</details>

---

## ⚡ Effects (Handle Async & Side‑Effects)

> **Effects listen to actions → trigger async work → dispatch new actions.**

<details>
<summary>Example</summary>

```ts
import { inject, Injectable } from '@angular/core';
import { Actions, createEffect, ofType } from '@ngrx/effects';
import { switchMap, map, catchError, of } from 'rxjs';
import { HttpClient } from '@angular/common/http';

@Injectable()
export class UsersEffects {
  private actions$ = inject(Actions);
  private http = inject(HttpClient);

  loadUsers$ = createEffect(() =>
    this.actions$.pipe(
      ofType(loadUsers),
      switchMap(() =>
        this.http.get<User[]>(`/api/users`).pipe(
          map(users => loadUsersSuccess({ users })),
          catchError(error => of(loadUsersFailure({ error })))
        )
      )
    )
  );
}
```

</details>

Provide effects:

<details>
<summary>Example</summary>

```ts
import { provideEffects } from '@ngrx/effects';

providers: [
  provideEffects(UsersEffects)
];
```

</details>

---

## 📦 NgRx Entity (for Lists & CRUD)

> Provides **adapters** to manage collections efficiently (IDs, sorting, updates).

<details>
<summary>Example</summary>

```ts
import { createEntityAdapter, EntityState } from '@ngrx/entity';

interface User { id: string; name: string; }

export interface UsersState extends EntityState<User> {
  loading: boolean;
}

const adapter = createEntityAdapter<User>();

const initialState = adapter.getInitialState({ loading: false });

export const usersReducer = createReducer(
  initialState,
  on(loadUsersSuccess, (s, { users }) =>
    adapter.setAll(users, { ...s, loading: false })
  )
);

export const {
  selectAll: selectAllUsers,
  selectEntities: selectUserEntities
} = adapter.getSelectors();
```

</details>

---

## 🧭 Router Store

> Sync Angular Router state into the Store for reactive routing.

<details>
<summary>Example</summary>

```ts
import { provideRouterStore } from '@ngrx/router-store';

providers: [
  provideRouterStore()
];
```

</details>

---

## 🧰 Store DevTools

> Inspect state & actions in **Redux DevTools**.

<details>
<summary>Example</summary>

```ts
import { provideStoreDevtools } from '@ngrx/store-devtools';

providers: [
  provideStoreDevtools({ maxAge: 25, logOnly: false })
];
```

</details>

---

## 🧑‍🔬 ComponentStore (Local & Feature Stores)

> A lightweight, component‑scoped reactive store.

<details>
<summary>Example</summary>

```ts
import { ComponentStore } from '@ngrx/component-store';
import { Injectable } from '@angular/core';

interface TodoState { todos: string[]; }

@Injectable()
export class TodoStore extends ComponentStore<TodoState> {
  constructor() { super({ todos: [] }); }

  readonly todos$ = this.select(s => s.todos);

  readonly add = this.updater((s, todo: string) => ({
    todos: [...s.todos, todo]
  }));
}
```

</details>

---

## 🔗 NgRx + Angular Signals (Bridge)

> Select store values *as signals* for template‑first reactivity.

<details>
<summary>Example</summary>

```ts
import { toSignal } from '@angular/core/rxjs-interop';

users = toSignal(this.store.select(selectUsers));
```

</details>

---

## 🧪 Testing NgRx

> Test reducers as pure functions & test effects with marble testing.

<details>
<summary>Example</summary>

```ts
it('should increment', () => {
  const state = counterReducer({ value: 0 }, increment());
  expect(state.value).toBe(1);
});
```

</details>

---

## 📏 Best Practices

* Keep **reducers pure** — no async / side effects
* Use **selectors everywhere** — not `store.source` internals
* Prefer **Entity** for lists
* Load data using **Effects**, *not components*
* Use **ComponentStore** for isolated feature logic
* Lazy‑load feature state with `provideState`
* Avoid giant global stores — organize by feature

---

## 🧠 When to Use What

* **Store + Effects** → global cross‑app state, server sync, caching
* **ComponentStore** → page/feature‑local reactive state
* **Entity** → large lists & CRUD
* **RouterStore** → route‑driven logic
* **Signals + Store** → simpler read layer

---
