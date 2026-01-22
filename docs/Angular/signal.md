---
id: signals
title: Signals
sidebar_position: 21
---

# Signals

---

## Overview

Signals are **synchronous, reactive values** that represent state. Angular tracks signal reads and automatically re-runs dependent logic when signals change.

---

## signal()

Create a writable reactive value.

<details>
<summary>Example</summary>

```ts
import { signal } from '@angular/core';

count = signal(0);           // number
name = signal<string>('');   // string

count();        // read
count.set(1);   // write
count.update(v => v + 1);
```

</details>

---

## computed()

Derive state from other signals.

<details>
<summary>Example</summary>

```ts
import { signal, computed } from '@angular/core';

price = signal(100);
tax = signal(0.1);

total = computed(() => price() * (1 + tax()));

// recalculates when price or tax changes
```

</details>

---

## linkedSignal()

Create a writable signal that stays in sync with one or more source signals.

<details>
<summary>Example</summary>

```ts
import { signal, linkedSignal } from '@angular/core';

firstName = signal('John');
lastName = signal('Doe');

fullName = linkedSignal(() => `${firstName()} ${lastName()}`);

fullName();            // 'John Doe'
fullName.set('Jane');  // updates linked value only
```

</details>

---

## effect()

Run side effects when signal dependencies change.

<details>
<summary>Example</summary>

```ts
import { signal, effect } from '@angular/core';

count = signal(0);

effect(() => {
  console.log('count changed:', count());
});
```

</details>

---

## Writable vs Readonly Signals

Control whether consumers can mutate state.

<details>
<summary>Example</summary>

```ts
import { signal } from '@angular/core';

private _count = signal(0);
count = this._count.asReadonly();

this._count.set(1);
// this.count.set(...) ❌ not allowed
```

</details>

---

## Signal Equality & Updates

Signals only notify dependents when values change.

<details>
<summary>Example</summary>

```ts
count.set(1);
count.set(1); // no re-run

count.update(v => v); // no re-run
```

</details>

---

## untracked()

Read a signal value without registering a reactive dependency.

<details>
<summary>Example</summary>

```ts
import { signal, effect, untracked } from '@angular/core';

count = signal(0);

// effect depends only on `count`, not on `other`
effect(() => {
  console.log(count());
  untracked(() => console.log(other()));
});

other = signal(100);
```

</details>

---

## isSignal()

Check whether a value is a signal at runtime.

<details>
<summary>Example</summary>

```ts
import { signal, isSignal } from '@angular/core';

value = signal(1);

isSignal(value);   // true
isSignal(123);     // false
```

</details>

---

## isWritableSignal()

Determine whether a signal can be mutated.

<details>
<summary>Example</summary>

```ts
import { signal, isWritableSignal } from '@angular/core';

count = signal(0);
readonlyCount = count.asReadonly();

isWritableSignal(count);          // true
isWritableSignal(readonlyCount);  // false
```

</details>

---

## Signals and Change Detection

Signals automatically trigger change detection without manual subscriptions.

<details>
<summary>Example</summary>

```ts
@Component({
  template: `{{ count() }}`
})
export class Demo {
  count = signal(0);
}
```

</details>

---

## Signals with OnPush Change Detection

Signals automatically mark OnPush components for check when they change.

<details>
<summary>Example</summary>

```ts
import { Component, signal, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-onpush',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `{{ count() }}`
})
export class OnPushComponent {
  count = signal(0);

  increment() {
    this.count.update(v => v + 1); // triggers view update
  }
}
```

</details>

---
