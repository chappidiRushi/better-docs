---
id: rxjs
title: RxJS
sidebar_position: 19
---

# RxJS

> **RxJS** is Angular's reactive programming library for handling async streams such as HTTP, events, forms, routing, and WebSockets.

---

## Overview

* **Observable** → stream of async values
* **Observer** → consumes values
* **Subscription** → manages lifecycle
* **Operators** → transform streams
* **Subjects** → multicast streams

Angular uses RxJS in:

* HttpClient
* Router events
* Forms
* Component lifecycles

---

## Creating Observables

<details>
<summary>Example</summary>

```ts
import { of, from, interval, timer, Observable } from 'rxjs';

const a$ = of(1, 2, 3);                     // emits values then completes
const b$ = from([10, 20, 30]);              // convert iterable/promise
const c$ = interval(1000);                  // emits 0,1,2,... every second
const d$ = timer(3000);                     // emits once after 3s

const e$ = new Observable(observer => {
  observer.next('hi');
  observer.complete();
});
```

</details>

---

## Subscribing & Unsubscribing

<details>
<summary>Example</summary>

```ts
const sub = interval(1000).subscribe({
  next: v => console.log(v),
  error: err => console.error(err),
  complete: () => console.log('done')
});

sub.unsubscribe(); // stop
```

</details>

---

## Auto‑unsubscribe in Angular (signal destroyRef)

<details>
<summary>Example</summary>

```ts
import { Component, effect, inject, signalDestroyRef } from '@angular/core';
import { interval, takeUntilDestroyed } from '@angular/core/rxjs-interop';

@Component({ standalone: true, template: '' })
export class Demo {
  destroyRef = signalDestroyRef();

  ngOnInit() {
    interval(1000)
      .pipe(takeUntilDestroyed(this.destroyRef))
      .subscribe(v => console.log(v));
  }
}
```

</details>

---

## Common Pipeable Operators

### Transformation

<details>
<summary>Example</summary>

```ts
import { map, scan, pluck } from 'rxjs';

source$.pipe(
  map(v => v * 2),                      // transform value
  scan((acc, v) => acc + v, 0)         // running total
);
```

</details>

---

### Filtering

<details>
<summary>Example</summary>

```ts
import { filter, take, takeUntil, debounceTime, distinctUntilChanged } from 'rxjs';

source$.pipe(
  filter(v => v > 10),
  take(3),                             // complete after 3
  debounceTime(300),                   // wait for pause
  distinctUntilChanged()               // ignore same value
);
```

</details>

---

### Combination

<details>
<summary>Example</summary>

```ts
import { combineLatest, withLatestFrom, merge, concat, forkJoin } from 'rxjs';

combineLatest([a$, b$]);               // latest values from both
withLatestFrom(b$);                     // source + last from b
merge(a$, b$);                          // interleave
concat(a$, b$);                         // sequential
forkJoin([a$, b$]);                     // wait all complete → array
```

</details>

---

### Flattening (Higher‑Order Mapping)

Pick based on behavior:

| Operator     | Behavior                           |
| ------------ | ---------------------------------- |
| `switchMap`  | cancel previous, keep latest       |
| `mergeMap`   | run all concurrently               |
| `concatMap`  | queue sequentially                 |
| `exhaustMap` | ignore new until current completes |

<details>
<summary>Example</summary>

```ts
import { switchMap, mergeMap, concatMap, exhaustMap } from 'rxjs';

clicks$.pipe(
  switchMap(() => http.get('/api'))
);
```

</details>

---

## Error Handling

<details>
<summary>Example</summary>

```ts
import { catchError, retry, retryWhen, delay } from 'rxjs';

http.get('/api').pipe(
  retry(2),                             // retry twice
  catchError(err => of([]))             // fallback value
);
```

</details>

---

## Subjects & Multicasting

### Subject Types

* `Subject` → multicast stream
* `BehaviorSubject` → stores last value
* `ReplaySubject` → replays N values
* `AsyncSubject` → emits last on complete

<details>
<summary>Example</summary>

```ts
import { Subject, BehaviorSubject } from 'rxjs';

const s$ = new Subject<number>();
const b$ = new BehaviorSubject(0);      // must init value

s$.next(1);
b$.value;                               // last value
```

</details>

---

## HttpClient + RxJS

<details>
<summary>Example</summary>

```ts
http.get<User[]>('/api/users').pipe(
  map(users => users.filter(u => u.active)),
  catchError(() => of([]))
);
```

</details>

---

## Router Events as Observables

<details>
<summary>Example</summary>

```ts
import { Router, NavigationEnd } from '@angular/router';
import { filter } from 'rxjs';

router.events.pipe(
  filter(e => e instanceof NavigationEnd)
);
```

</details>

---

## Converting Between Promises & Observables

<details>
<summary>Example</summary>

```ts
import { firstValueFrom, lastValueFrom, from } from 'rxjs';

const obs$ = from(fetch('/api')); // promise → observable

await firstValueFrom(obs$);       // observable → promise
```

</details>

---

## Scheduling & Performance

Schedulers control **when** work runs:

* `asyncScheduler` → async tasks
* `queueScheduler` → sync ordered
* `animationFrameScheduler` → UI‑safe timing

---

## Memory & Cleanup Strategy

* Prefer **`takeUntilDestroyed()`** in components
* Avoid manual `unsubscribe()` everywhere
* Never subscribe inside subscribe
* Use flattening operators instead

---

## Debugging Tips

* Add `.pipe(tap(console.log))`
* Check uncompleted subscriptions
* Use DevTools RxJS debugging extensions

---

## When to Use What

* **switchMap** → user typing / route changes
* **mergeMap** → parallel API calls
* **concatMap** → ordered requests
* **exhaustMap** → login buttons

---

This cheatsheet covers **RxJS usage in Angular 21+, operators, subjects, cleanup strategies, and best practices** — all with collapsible examples to fit your Docusaurus‑friendly format.

## 🚀 Cool Things You Can Do with RxJS

Here are some **powerful and fun real‑world patterns** you can easily implement using RxJS in Angular 21.

---

### 🔍 Debounced Live Search (avoid hammering the server)

<details>
<summary>Example</summary>

```ts
search$ = new Subject<string>();

results$ = this.search$.pipe(
  debounceTime(300),
  distinctUntilChanged(),
  switchMap(term => this.http.get(`/api?q=${term}`))
);
```

</details>

---

### ⚡ Retry API Calls with Exponential Backoff

<details>
<summary>Example</summary>

```ts
httpCall$ = this.http.get('/api').pipe(
  retryWhen(errors =>
    errors.pipe(
      scan((count) => count + 1, 0),
      tap(count => {
        if (count === 5) throw errors;
      }),
      delayWhen(count => timer(count * 1000))
    )
  )
);
```

</details>

---

### 🧠 Smart Caching — Share Latest Value Across Subscribers

<details>
<summary>Example</summary>

```ts
users$ = this.http.get('/users').pipe(
  shareReplay({ bufferSize: 1, refCount: true })
);
```

</details>

---

### 🧵 Combine Multiple Streams (UI + API + Auth)

<details>
<summary>Example</summary>

```ts
vm$ = combineLatest([
  this.user$,
  this.settings$,
  this.authService.isLoggedIn$
]).pipe(
  map(([user, settings, loggedIn]) => ({ user, settings, loggedIn }))
);
```

</details>

---

### ⏱️ Auto‑Save Form Values with Throttle + Distinct

<details>
<summary>Example</summary>

```ts
this.form.valueChanges.pipe(
  throttleTime(1000),
  distinctUntilChanged(),
  switchMap(v => this.save(v))
).subscribe();
```

</details>

---

### 🛰️ WebSocket Streams as Observables

<details>
<summary>Example</summary>

```ts
const socket$ = webSocket('wss://example.com');

socket$.subscribe(msg => console.log(msg));
```

</details>

---

### 🔁 Polling APIs the Right Way

<details>
<summary>Example</summary>

```ts
poll$ = timer(0, 5000).pipe(
  switchMap(() => this.http.get('/status'))
);
```

</details>

---

### 🎚️ Build Reactive ViewModels (no manual state syncing)

<details>
<summary>Example</summary>

```ts
vm$ = combineLatest([
  this.route.paramMap.pipe(map(p => p.get('id'))),
  this.cartService.items$
]).pipe(
  switchMap(([id, cart]) =>
    this.productService.getProduct(id).pipe(
      map(product => ({ product, cart }))
    )
  )
);
```

</details>

---

### 🎯 Cancel In‑Flight HTTP Requests

<details>
<summary>Example</summary>

```ts
this.search.valueChanges.pipe(
  switchMap(term => this.http.get(`/api?q=${term}`))
);
```

</details>

---

### 🧪 Test Streams Like a Pro (Marble Testing)

You can write tests that express time visually using marble strings — super readable and deterministic.

---

## 🥷 Pro‑Level Patterns

* **Facade pattern with vm$** — keep components dumb and UI‑focused
* **Global store with RxJS (without NgRx)**
* **Event buses using Subjects**
* **Reactive forms + operators**
* **Backpressure strategies (debounce, throttle, audit)**
* **Side‑effect isolation using `tap`**
* **Memory‑safe streams with `takeUntil`**

---

## ✅ Best Practices Refresher

* Prefer **`switchMap` for search‑like actions**
* Prefer **`mergeMap` for parallel work**
* Prefer **`concatMap` for ordered work**
* Use **`exhaustMap` for login buttons** (ignore spam clicks)
* Always clean up long‑lived streams
* Cache where it makes sense

---
