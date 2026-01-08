# Routing

---

## ⭐ Overview

Angular routing **maps URLs → components**, handles navigation, lazy loading, route data, guards, and parameters using a single‐page app router.

---

## 🛣️ Defining Routes

Routes describe which component loads for a path.

```ts
import { Routes } from '@angular/router';

export const routes: Routes = [
  { path: '', component: HomeComponent },                // default route
  { path: 'about', component: AboutComponent },          // static path
  { path: 'users/:id', component: UserComponent },       // route param
  { path: '**', component: NotFoundComponent }           // wildcard
];
```

`pathMatch: 'full'` forces full URL match (often for redirects).

```ts
{ path: '', redirectTo: '/home', pathMatch: 'full' }
```

---

## ⚙️ Bootstrapping the Router

Provide the router at app startup.

```ts
import { bootstrapApplication } from '@angular/platform-browser';
import { provideRouter } from '@angular/router';

bootstrapApplication(AppComponent, {
  providers: [provideRouter(routes)]
});
```

Optional router features:

```ts
provideRouter(routes, withInMemoryScrolling(), withComponentInputBinding())
```

---

## 🧭 Navigating Between Routes

Template navigation:

```html
<a routerLink="/about">About</a>
<a [routerLink]="['/users', userId]">User</a>
```

Programmatic navigation:

```ts
import { Router } from '@angular/router';

constructor(private router: Router) {}

this.router.navigate(['/users', 123]);

this.router.navigate(['/search'], {
  queryParams: { q: 'angular', page: 1 }
});
```

---

## 🔐 Route Guards (Functional)

Guards **control whether navigation is allowed**.

```ts
import { inject } from '@angular/core';
import { CanActivateFn } from '@angular/router';

export const authGuard: CanActivateFn = () => {
  const auth = inject(AuthService);
  return auth.isLoggedIn(); // boolean | UrlTree | Observable | Promise
};
```

Use in route definition:

```ts
{ path: 'dashboard', canActivate: [authGuard], component: DashboardComponent }
```

Other guards:

* `canDeactivate` — block leaving
* `canMatch` — block matching a route entirely
* `resolve` — load data before route activates

---

## 📥 Route Resolvers

Resolvers **fetch data before loading a route**.

```ts
import { ResolveFn } from '@angular/router';

export const userResolver: ResolveFn<User> = (route) => {
  const id = route.paramMap.get('id');
  return inject(UserService).getUser(id!);
};
```

Bind to route:

```ts
{ path: 'users/:id', component: UserComponent, resolve: { user: userResolver } }
```

Consume in component:

```ts
const route = inject(ActivatedRoute);
route.data.subscribe(d => console.log(d.user));
```

---

## 💤 Lazy Loading (Standalone)

Lazy loading **defers feature code until needed**.

### Load a standalone component

```ts
{ path: 'settings', loadComponent: () => import('./settings.component').then(m => m.SettingsComponent) }
```

### Load child routes

```ts
{ path: 'admin', loadChildren: () => import('./admin.routes').then(m => m.ADMIN_ROUTES) }
```

Child route file:

```ts
export const ADMIN_ROUTES: Routes = [
  { path: '', component: AdminHomeComponent },
  { path: 'users', component: AdminUsersComponent }
];
```

---

## 🧩 Router Outlets & Multiple Layouts

`<router-outlet>` **renders the active route component**.

You can create **auxiliary outlets** for side‑by‑side views:

```html
<router-outlet name="sidebar"></router-outlet>
```

Route example:

```ts
{ path: 'help', outlet: 'sidebar', component: HelpPanelComponent }
```

---

## 🧷 Route Parameters & Query Params

### Route params

```ts
// /users/123
const route = inject(ActivatedRoute);

route.paramMap.subscribe(p => {
  const id = p.get('id');
});
```

### Query params

```ts
// /search?q=angular&page=1
route.queryParamMap.subscribe(q => {
  const qStr = q.get('q');
});
```

Preserve params during navigation:

```ts
this.router.navigate(['./'], { relativeTo: route, queryParamsHandling: 'preserve' });
```

---

## 🧠 Route Data & Component Inputs

Static `data` **passes metadata to components**.

```ts
{ path: 'about', component: AboutComponent, data: { title: 'About Us' } }
```

Read it:

```ts
inject(ActivatedRoute).data.subscribe(d => console.log(d.title));
```

With `withComponentInputBinding()`, route params & data can bind directly to `@Input()`.

---

## 🔁 Redirects & Wildcards

Redirects change the URL.

```ts
{ path: '', redirectTo: 'home', pathMatch: 'full' }
{ path: '**', component: NotFoundComponent }
```

Use `pathMatch: 'full'` for exact root redirects.

---

## 🚀 Navigation Extras

Control navigation behavior:

```ts
this.router.navigate(['/cart'], {
  replaceUrl: true,      // replace history entry
  skipLocationChange: true, // does not update URL bar
  state: { from: 'checkout' } // history.state
});
```

Read router state:

```ts
history.state
```

---

## 🧭 Multiple Routes & Child Route Hierarchies

Multiple routes let you **structure features under a parent path** and/or show **more than one routed view at the same time**.

### Child routes (nested routing)

Child routes share a **layout component** that hosts a `<router-outlet>`.

```ts
export const ACCOUNT_ROUTES: Routes = [
  {
    path: 'account',
    component: AccountShellComponent,   // layout/shell
    children: [
      { path: '', component: OverviewComponent },      // /account
      { path: 'orders', component: OrdersComponent },  // /account/orders
      { path: 'settings', component: SettingsComponent }
    ]
  }
];
```

`AccountShellComponent` template:

```html
<h1>Account</h1>
<nav>
  <a routerLink="/account">Overview</a>
  <a routerLink="/account/orders">Orders</a>
  <a routerLink="/account/settings">Settings</a>
</nav>
<router-outlet></router-outlet>
```

This keeps feature navigation scoped under the parent path.

---

### Multiple active routes (auxiliary / named outlets)

Use **named outlets** to render multiple routed components at once.

Outlet in template:

```html
<router-outlet></router-outlet>              <!-- primary -->
<router-outlet name="sidebar"></router-outlet> <!-- secondary -->
```

Define routes:

```ts
export const routes: Routes = [
  { path: 'home', component: HomeComponent },
  { path: 'help', outlet: 'sidebar', component: HelpPanelComponent },
];
```

Navigate to multiple outlets at once:

```ts
this.router.navigate([
  { outlets: { primary: ['home'], sidebar: ['help'] } }
]);
```

Clear an auxiliary outlet:

```ts
this.router.navigate([{ outlets: { sidebar: null } }]);
```

Use auxiliary routes when you want **independent UI panels controlled by the router**.

---

## 📌 Best Practices

* Prefer **standalone + provideRouter()**
* Use **lazy loading** for feature areas
* Keep route config **flat & readable**
* Use **functional guards & resolvers**
* Centralize **app shell layout** around `<router-outlet>`
* Handle **404s with wildcard route**

---
