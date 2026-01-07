# Dependency Injection

---

## ⭐ Overview

Angular dependency injection (DI) **creates and delivers service instances to classes that need them**, using a tree of injectors for scoping and reuse.

Key ideas:

* **Service** = reusable logic class
* **Provider** = recipe telling Angular how to create a dependency
* **Injector** = runtime container that resolves dependencies
* **Token** = lookup key for a dependency (usually the class)
* **Scope** = where an instance lives (root / component / module boundary / custom)

---

## 🧩 Creating & Using Services

A **service** is a class marked with `@Injectable()` so Angular can inject it.

```ts
import { Injectable } from '@angular/core';

@Injectable({ providedIn: 'root' }) // tree‑shakable, app‑wide singleton
export class UserService {
  user = { name: 'Alex' };
}
```

### Inject into a component (modern style)

```ts
import { Component, inject } from '@angular/core';
import { UserService } from './user.service';

@Component({
  selector: 'app-profile',
  standalone: true,
  template: `Hello {{ userService.user.name }}`
})
export class ProfileComponent {
  userService = inject(UserService); // field‑level injection
}
```

You can still inject via constructor, but `inject()` is preferred in standalone apps.

---

## 🏗 Defining Dependency Providers

A **provider defines how Angular creates a value for a token**.

### 1️⃣ Class as token (default)

```ts
@Injectable({ providedIn: 'root' })
export class ApiService {}
```

### 2️⃣ Local providers (override scope)

```ts
@Component({
  selector: 'app-local',
  standalone: true,
  providers: [ApiService],
  template: ''
})
export class LocalComponent {}
```

This creates **a new instance for this component subtree**.

### 3️⃣ Use `useClass`, `useExisting`, `useValue`, `useFactory`

```ts
providers: [
  { provide: Logger, useClass: ConsoleLogger },           // map class → impl
  { provide: BaseLogger, useExisting: Logger },           // alias existing
  { provide: APP_VERSION, useValue: '1.2.3' },            // constant
  { provide: Token, useFactory: () => new Thing() }       // factory
]
```

### 4️⃣ Multi‑providers

```ts
{ provide: HOOKS, multi: true, useValue: 'hook-a' }
{ provide: HOOKS, multi: true, useValue: 'hook-b' }
// injection returns: ['hook-a','hook-b']
```

---

## 🌐 Injection Context

`inject()` **only works inside an Angular injection context** (component creation, provider factories, effects, etc.).

When outside, use `runInInjectionContext()`:

```ts
import { inject, runInInjectionContext, EnvironmentInjector } from '@angular/core';

export function doWork(injector: EnvironmentInjector) {
  return runInInjectionContext(injector, () => {
    const api = inject(ApiService);
    return api;
  });
}
```

Common places that **are already in context**:

* `constructor(...)` injection
* inside component/directive/providers
* inside factory providers

Common places **NOT in context**:

* plain exported functions
* setTimeout / event handlers
* external library callbacks

---

## 🌲 Hierarchical Injectors

Angular builds a **tree of injectors** matching the component tree.

Resolution order:

1. **Component `providers` / `viewProviders`**
2. **Parent components up the tree**
3. **Environment (root) injector**

So you can:

### Scope a service to a feature/component

```ts
@Component({
  selector: 'app-scope-demo',
  standalone: true,
  providers: [CartService],
  template: `<child />`
})
export class ScopeDemo {}
```

Every instance of `ScopeDemo` gets **its own CartService**.

### `providers` vs `viewProviders`

* **providers** → shared with projected content
* **viewProviders** → visible only inside the component view

```ts
@Component({
  viewProviders: [PrivateService]
})
export class SecretComponent {}
```

---

## 🧠 Optimizing Injection Tokens

A **token** is the key used to look up a dependency.

### Prefer typed `InjectionToken`

```ts
import { InjectionToken } from '@angular/core';

export const API_URL = new InjectionToken<string>('api-url');
```

Provide & inject:

```ts
providers: [{ provide: API_URL, useValue: 'https://api.example.com' }];

const url = inject(API_URL);
```

### Tree‑shakable tokens with factory

```ts
export const ENV = new InjectionToken('env', {
  providedIn: 'root',
  factory: () => ({ prod: true })
});
```

No provider array needed → **smaller bundles**.

### Use multi‑tokens for extensibility

Good for **plugin hooks and interceptors**.

```ts
export const HOOKS = new InjectionToken<string[]>('hooks', { multi: true });
```

### Avoid string tokens

They are brittle and not type‑safe.

---

## 📌 Tips & Best Practices

* Use `providedIn: 'root'` for most services (tree‑shakable singletons)
* Use component `providers` for **scoped state**
* Prefer `inject()` in standalone apps
* Use **typed `InjectionToken`** for primitives & interfaces
* Be aware of **hierarchical resolution** to avoid duplicate instances
* Use **multi‑providers** for extensibility

---
