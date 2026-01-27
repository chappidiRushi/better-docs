---
id: http-client
title: HttpClient
sidebar_position: 18
---

# HttpClient

## Overview

* **HttpClient** is Angular’s typed, Observable‑based API for HTTP/REST.
* Tree‑shakable setup via **`provideHttpClient()`**; opt‑in features like **`withFetch()`** and **`withInterceptors()`**.
* Works seamlessly with standalone components, functional services, and DI via **`inject()`**.

---

## App‑Level Setup (Standalone)

<details>
<summary>Example</summary>
```ts
// app.config.ts
import { ApplicationConfig, inject } from '@angular/core';
import {
  provideHttpClient,
  withInterceptors,
  withFetch,
} from '@angular/common/http';

import { authInterceptor } from './auth.interceptor';

export const appConfig: ApplicationConfig = {
providers: [
provideHttpClient(
withFetch(),              // optional: native Fetch backend
withInterceptors([authInterceptor])
)
]
};

````
</details>

Inject anywhere:
<details>
<summary>Example</summary>
```ts
import { HttpClient } from '@angular/common/http';
import { inject } from '@angular/core';

const http = inject(HttpClient);
````

</details>

> ✔️ Prefer **`inject()`** over constructor injection in functional code.

---

## Making Requests (Typed)

<details>
<summary>Example</summary>
```ts
http.get<T>(url, options?)
http.post<T>(url, body?, options?)
http.put<T>(url, body?, options?)
http.patch<T>(url, body?, options?)
http.delete<T>(url, options?)
http.request<T>(method, url, options?)
```
</details>

<details>
<summary>Example</summary>
```ts
interface Todo { id: number; title: string; completed: boolean; }
http.get<Todo[]>('/api/todos').subscribe();
```
</details>

### Common Options

* `headers?: HttpHeaders | {...}`
* `params?: HttpParams | {...}`
* `observe?: 'body' | 'response' | 'events'`
* `responseType?: 'json' | 'text' | 'blob' | 'arraybuffer'`
* `withCredentials?: boolean`
* `context?: HttpContext`

**Query Params (object literal OK):**

<details>
<summary>Example</summary>
```ts
http.get('/api/items', { params: { page: 1, q: 'search' } });
```
</details>

---

## Responses & Errors

* Success emits **once** then completes.
* Errors emit **`HttpErrorResponse`** (`status`, `message`, `error`, etc.).

<details>
<summary>Example</summary>
```ts
import { HttpErrorResponse } from '@angular/common/http';
import { catchError, throwError } from 'rxjs';

http.get('/api/data').pipe(
catchError((err: HttpErrorResponse) => {
console.error(err.status, err.error);
return throwError(() => err);
})
).subscribe();

````
</details>

---

## Functional Interceptors (Recommended)
<details>
<summary>Example</summary>
```ts
import { HttpInterceptorFn } from '@angular/common/http';

export const authInterceptor: HttpInterceptorFn = (req, next) => {
  const cloned = req.clone({
    setHeaders: { Authorization: 'Bearer token' }
  });
  return next(cloned);
};
````

</details>

Register with:

<details>
<summary>Example</summary>
```ts
provideHttpClient(withInterceptors([authInterceptor]));
```
</details>

---

## Upload Progress & Events

<details>
<summary>Example</summary>
```ts
import { HttpEventType } from '@angular/common/http';

http.post('/upload', formData, {
reportProgress: true,
observe: 'events'
}).subscribe(event => {
if (event.type === HttpEventType.UploadProgress) {
const pct = Math.round(100 * (event.loaded ?? 0) / (event.total ?? 1));
}
});

````
</details>

---

## Files & Binary Data
<details>
<summary>Example</summary>
```ts
http.get('/file.pdf', { responseType: 'blob' })
  .subscribe(blob => { /* save or preview */ });
````

</details>

---

## Cancellation

* **Unsubscribe** from the returned Observable, or
* Use RxJS like **`takeUntil()`** / **`takeUntilDestroyed()`** (Angular).

<details>
<summary>Example</summary>
```ts
const destroy$ = new Subject<void>();
http.get('/api').pipe(takeUntil(destroy$)).subscribe();
destroy$.next();
destroy$.complete();
```
</details>

---

## HttpContext & Tokens (Per‑Request Flags)

<details>
<summary>Example</summary>
```ts
import { HttpContext, HttpContextToken } from '@angular/common/http';

export const SKIP_AUTH = new HttpContextToken(() => false);

http.get('/public', {
context: new HttpContext().set(SKIP_AUTH, true)
});

````
</details>

---

## Organizing APIs (Services)
<details>
<summary>Example</summary>
```ts
import { Injectable, inject } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Injectable({ providedIn: 'root' })
export class UsersApi {
  private http = inject(HttpClient);
  private base = '/api/users';

  list() { return this.http.get<User[]>(this.base); }
  create(dto: Partial<User>) { return this.http.post<User>(this.base, dto); }
}
````

</details>

---

## Retry & Backoff (RxJS)

<details>
<summary>Example</summary>
```ts
import { retry } from 'rxjs';

http.get('/api').pipe(retry(2)).subscribe();

````
</details>

*(Use `retryWhen` + `delay` + `scan` for custom backoff.)*

---

## Authentication
- Attach tokens in an **interceptor**.
- Cookie auth → `{ withCredentials: true }`.

---

## Testing (HttpClientTesting)
<details>
<summary>Example</summary>
```ts
import {
  HttpTestingController,
  provideHttpClientTesting
} from '@angular/common/http/testing';
import { TestBed } from '@angular/core/testing';
import { provideHttpClient } from '@angular/common/http';

beforeEach(() => {
  TestBed.configureTestingModule({
    providers: [
      provideHttpClient(),
      provideHttpClientTesting()
    ]
  });
});

it('mocks a request', () => {
  const http = TestBed.inject(HttpClient);
  const httpMock = TestBed.inject(HttpTestingController);

  http.get('/api/item').subscribe();

  httpMock.expectOne('/api/item').flush({ id: 1 });
  httpMock.verify();
});
````

</details>

---

## Fetch Backend (Optional)

* `withFetch()` switches to the native Fetch API (smaller; some differences like no download progress events).

---
