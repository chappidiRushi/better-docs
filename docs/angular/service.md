---
id: angular-services-cheatsheet
title: Services
sidebar_position: 15
---
# Services

> Angular services manage shared logic and data, injectable via DI in components, other services, or standalone providers.

---

## Overview

* **Services** are classes marked `@Injectable()` that hold business logic, state, or API calls.
* Use DI to inject them anywhere.
* Can be **singleton**, **scoped**, or **provided per module/component**.
* Ideal for reusing logic across the app.

---

## Creating a Service

<details>
<summary>Example</summary>

```ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root' // singleton across app
})
export class UserService {
  private users: string[] = [];

  addUser(name: string) {
    this.users.push(name);
  }

  getUsers(): string[] {
    return this.users;
  }
}
```

</details>

---

## Injecting a Service

<details>
<summary>Example</summary>

```ts
import { Component, inject } from '@angular/core';
import { UserService } from './user.service';

@Component({
  selector: 'app-user-list',
  template: '<p *ngFor="let user of users">{{ user }}</p>',
})
export class UserListComponent {
  private userService = inject(UserService);
  users = this.userService.getUsers();
}
```

</details>

---

## Scoped Providers

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-profile',
  template: '...',
  providers: [UserService] // new instance per component
})
export class ProfileComponent {
  constructor(private userService: UserService) {}
}
```

</details>

---

## Injecting Other Services

<details>
<summary>Example</summary>

```ts
@Injectable({ providedIn: 'root' })
export class AuthService {
  constructor(private userService: UserService) {}

  login(name: string) {
    this.userService.addUser(name);
  }
}
```

</details>

---

## Functional Providers (Standalone)

<details>
<summary>Example</summary>

```ts
import { inject, Injectable } from '@angular/core';
import { HttpClient, provideHttpClient } from '@angular/common/http';

export function fetchUsers() {
  const http = inject(HttpClient);
  return http.get('/api/users');
}

bootstrapApplication(AppComponent, {
  providers: [provideHttpClient(), { provide: 'FETCH_USERS', useFactory: fetchUsers }]
});
```

</details>

---

## Optional Dependencies

<details>
<summary>Example</summary>

```ts
import { Injectable, Optional } from '@angular/core';

@Injectable({ providedIn: 'root' })
export class LoggerService {
  log(msg: string) { console.log(msg); }
}

@Injectable({ providedIn: 'root' })
export class DataService {
  constructor(@Optional() private logger?: LoggerService) {}

  fetchData() {
    this.logger?.log('Fetching data');
    return [];
  }
}
```

</details>

---

## Hierarchical Injectors

<details>
<summary>Example</summary>

```ts
@NgModule({
  providers: [UserService] // instance scoped to module
})
export class UserModule {}

@Component({
  selector: 'app-child',
  providers: [UserService] // new instance just for this component
})
export class ChildComponent {}
```

</details>

---

## ProvidedIn Options

| Option                | Scope                                           |
| --------------------- | ----------------------------------------------- |
| `'root'`              | singleton, available app-wide                   |
| `'platform'`          | singleton across multiple apps in same platform |
| `'any'`               | new instance per lazy-loaded module             |
| Component `providers` | scoped to component instance                    |
