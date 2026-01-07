# Service Workers & PWA

> **Angular Service Workers** turn your Angular app into a Progressive Web App (PWA) with offline support, caching, background updates, and push notifications.

---

## Overview

* **Service Workers** run in the browser background separate from the main thread.
* Angular ships a **built‑in service worker** (`@angular/service-worker`).
* Enabled automatically when **building for production**.
* Provides **offline caching, faster loads, update control, and push**.

---

## Enable PWA Support (Angular 21)

<details>
<summary>Example</summary>

```bash
ng add @angular/pwa --project=my-app --skipInstall=false --skipConfirm=false
```

</details>

Adds:

* `ngsw-config.json` → SW config
* `manifest.webmanifest` → PWA metadata
* Icons
* Registers SW in `main.ts`

---

## Service Worker Registration

<details>
<summary>Example</summary>

```ts
import { bootstrapApplication } from '@angular/platform-browser';
import { provideServiceWorker } from '@angular/service-worker';
import { AppComponent } from './app/app.component';
import { isDevMode } from '@angular/core';

bootstrapApplication(AppComponent, {
  providers: [
    provideServiceWorker('ngsw-worker.js', {
      enabled: !isDevMode(),        // only enable in production
      registrationStrategy: 'registerWhenStable:30000' // lazy register
    })
  ]
});
```

</details>

`registrationStrategy` options:

* `registerImmediately`
* `registerWhenStable[:timeout]`
* `registerWithDelay:<ms>`

---

## `ngsw-config.json` — Caching Strategy

Defines what & how Angular SW caches.

<details>
<summary>Example</summary>

```json
{
  "$schema": "https://angular.dev/assets/schemas/ngsw-config.schema.json",
  "index": "/index.html",
  "assetGroups": [
    {
      "name": "app",
      "installMode": "prefetch",          // prefetch | lazy
      "updateMode": "prefetch",           // prefetch | lazy
      "resources": {
        "files": [
          "/favicon.ico",
          "/index.html",
          "/*.css",
          "/*.js"
        ]
      }
    },
    {
      "name": "assets",
      "installMode": "lazy",
      "updateMode": "prefetch",
      "resources": {
        "files": [
          "/assets/**",
          "/*.(png|jpg|svg|webp)"
        ]
      }
    }
  ],
  "dataGroups": [
    {
      "name": "api-cache",
      "urls": [
        "/api/**"
      ],
      "cacheConfig": {
        "strategy": "performance",       // performance | freshness
        "maxSize": 100,
        "maxAge": "1d",
        "timeout": "10s"
      }
    }
  ],
  "navigationUrls": [
    { "positive": true, "regex": ".*" },
    { "positive": false, "regex": "/api/.*" }
  ]
}
```

</details>

Key values:

* **assetGroups** → static files
* **dataGroups** → API/network caching
* **strategy**

  * `performance` (cache‑first)
  * `freshness` (network‑first)

---

## App Manifest (`manifest.webmanifest`)

Describes installable app metadata.

<details>
<summary>Example</summary>

```json
{
  "name": "My Angular PWA",
  "short_name": "MyPWA",
  "start_url": "/",
  "display": "standalone",       // fullscreen | minimal-ui | browser
  "background_color": "#ffffff",
  "theme_color": "#1976d2",
  "icons": [
    {
      "src": "assets/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "assets/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

</details>

---

## Detect & Apply Updates (`SwUpdate`)

<details>
<summary>Example</summary>

```ts
import { Component, inject } from '@angular/core';
import { SwUpdate } from '@angular/service-worker';

@Component({
  standalone: true,
  selector: 'app-update-handler',
  template: ''
})
export class UpdateHandlerComponent {
  swUpdate = inject(SwUpdate);

  constructor() {
    this.swUpdate.versionUpdates.subscribe(event => {
      if (event.type === 'VERSION_READY') {
        if (confirm('New version available. Reload?')) {
          location.reload();
        }
      }
    });
  }
}
```

</details>

Other APIs:

* `swUpdate.checkForUpdate()`
* `swUpdate.activateUpdate()`

---

## Push Notifications (`SwPush`)

<details>
<summary>Example</summary>

```ts
import { Component, inject } from '@angular/core';
import { SwPush } from '@angular/service-worker';

@Component({ standalone: true, template: '' })
export class PushComponent {
  swPush = inject(SwPush);
  VAPID_PUBLIC_KEY = 'YOUR_PUBLIC_VAPID_KEY';

  subscribe() {
    this.swPush.requestSubscription({
      serverPublicKey: this.VAPID_PUBLIC_KEY
    }).then(sub => {
      // send sub to backend
    }).catch(err => console.error(err));
  }
}
```

</details>

> Requires backend push service (Web Push protocol).

---

## Build & Test PWA

<details>
<summary>Example</summary>

```bash
ng build --configuration production
npx http-server -p 8080 ./dist/my-app
```

</details>

Service worker **only runs over HTTPS or localhost**.

---

## Common Strategies

* **Static assets → `assetGroups` + `prefetch`**
* **API requests → `dataGroups`**

  * `performance` for rarely changing data
  * `freshness` for real‑time data
* **Lazy load large assets**
* **Trigger user‑friendly update prompt**

---

## Debugging Service Workers

* Chrome DevTools → Application → Service Workers
* Check `ngsw.json` under Network tab
* Clear storage to reset cache

---

## Best Practices

* Enable SW only in production
* Avoid caching auth tokens or private data
* Set sensible `maxAge` + `maxSize`
* Provide manual refresh prompt
* Version APIs to avoid stale data

---

## Notes vs Web Workers

* **Service Workers** → offline, caching, push, proxy network
* **Web Workers** → CPU‑heavy background computation

They solve different problems.

---
