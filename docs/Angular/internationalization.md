# Internationalization (i18n)

> Built‑in Angular i18n using **@angular/localize** with runtime locale switching support

---

## Overview

* **Internationalization (i18n)** prepares your app for multiple languages.
* Angular provides:

  * Template translation via `i18n` attributes
  * Message extraction → translation files (XLIFF)
  * ICU plural & gender rules
  * Locale‑aware pipes (date / currency / number)
* Works in **standalone apps**, supports **multiple locales** & **runtime switching**.

---

## Setup (Standalone)

<details>
<summary>Example</summary>

```ts
// main.ts
import { bootstrapApplication } from '@angular/platform-browser';
import { AppComponent } from './app/app.component';
import { provideI18n, withLocaleId, withTranslations } from '@angular/localize/init';

bootstrapApplication(AppComponent, {
  providers: [
    provideI18n(
      withLocaleId('en'),              // default locale
      withTranslations('en', {}),      // translations loaded at build/runtime
    )
  ]
});
```

</details>

> ✔️ Angular 17+ supports **runtime translation loading** — no rebuild required per locale.

---

## Marking Text for Translation

### Basic Text

<details>
<summary>Example</summary>

```html
<h1 i18n>Welcome</h1>
```

</details>

---

### With Description & Meaning (Helps Translators)

<details>
<summary>Example</summary>

```html
<h1 i18n="@@welcomeHeading|Header shown on home page">Welcome</h1>
```

</details>

Format:

```
meaning|description@@customId
```

---

### Interpolations

<details>
<summary>Example</summary>

```html
<p i18n>Hello {{ name }}!</p>
```

</details>

---

### Attributes

<details>
<summary>Example</summary>

```html
<button i18n-title title="Save the form">Save</button>
```

</details>

---

## Plural & Gender (ICU Syntax)

### Plural

<details>
<summary>Example</summary>

```html
<p i18n>{ itemsCount, plural,
  =0 {No items}
  =1 {One item}
  other {{ itemsCount }} items
}</p>
```

</details>

---

### Select (e.g., Gender)

<details>
<summary>Example</summary>

```html
<p i18n>{ gender, select,
  male {He}
  female {She}
  other {They}
} liked your post.</p>
```

</details>

---

## Extracting Messages

Use Angular CLI:

<details>
<summary>Example</summary>

```bash
ng extract-i18n --format=xlf --output-path=src/i18n
```

</details>

Creates e.g.:

```
src/i18n/messages.xlf
```

Duplicate per locale:

```
messages.fr.xlf
messages.es.xlf
```

Translate the `<target>` tags.

---

## Loading Translations (Runtime)

You can dynamically load JSON/XLIFF.

<details>
<summary>Example</summary>

```ts
import { provideI18n, withLocaleId, withTranslations } from '@angular/localize/init';

export const appConfig = {
  providers: [
    provideI18n(
      withLocaleId('en'),
      withTranslations('en', enTranslations),
      withTranslations('fr', frTranslations),
    )
  ]
};
```

</details>

Then switch locale:

<details>
<summary>Example</summary>

```ts
import { inject } from '@angular/core';
import { I18nService } from '@angular/localize';

const i18n = inject(I18nService);
i18n.setLocale('fr');
```

</details>

---

## Registering Locale Data (Dates, Numbers, Currency)

<details>
<summary>Example</summary>

```ts
import { registerLocaleData } from '@angular/common';
import localeFr from '@angular/common/locales/fr';

registerLocaleData(localeFr);
```

</details>

---

## Locale‑Aware Pipes

### DatePipe

<details>
<summary>Example</summary>

```html
<p>{{ today | date:'longDate' }}</p>
```

</details>

---

### CurrencyPipe

<details>
<summary>Example</summary>

```html
<p>{{ price | currency:'EUR':'symbol' }}</p>
```

</details>

---

### DecimalPipe / PercentPipe

<details>
<summary>Example</summary>

```html
<p>{{ rating | number:'1.0-2' }}</p>
<p>{{ ratio | percent }}</p>
```

</details>

---

## Multiple Locales (Build + Runtime)

### Build‑Time (Legacy — Rebuild per locale)

<details>
<summary>Example</summary>

```jsonc
// angular.json
{
  "projects": {
    "app": {
      "i18n": {
        "sourceLocale": "en-US",
        "locales": {
          "fr": "src/i18n/messages.fr.xlf",
          "es": "src/i18n/messages.es.xlf"
        }
      }
    }
  }
}
```

```
ng build --localize
```

</details>

### Runtime (Recommended)

* One build → load translations dynamically
* Use `provideI18n()` with `withTranslations()`

---

## Translating Code Strings (`$localize`)

<details>
<summary>Example</summary>

```ts
import { $localize } from '@angular/localize';

const message = $localize`Hello, ${name}!`;
```

</details>

---

## Parameterized Messages (Templates)

<details>
<summary>Example</summary>

```html
<p i18n>Logged in as {{ username }}</p>
```

</details>

---

## Tagged Meaning & Context

<details>
<summary>Example</summary>

```html
<button i18n="@@saveButton|Action to save form">Save</button>
```

</details>

---

## Dynamic Locale Switching Strategy

* Store current locale (signal/store/service)
* Load translation file
* Call `setLocale()`
* Register locale data when first used

<details>
<summary>Example</summary>

```ts
import { effect, signal } from '@angular/core';
import { I18nService } from '@angular/localize';
import { registerLocaleData } from '@angular/common';

const locale = signal<'en' | 'fr'>('en');
const i18n = inject(I18nService);

effect(async () => {
  const l = locale();
  if (l === 'fr') {
    const { default: fr } = await import('@angular/common/locales/fr');
    registerLocaleData(fr);
  }
  i18n.setLocale(l);
});
```

</details>

---

## Testing Translations

<details>
<summary>Example</summary>

```ts
import { render } from '@testing-library/angular';

it('renders translated text', async () => {
  const { getByText } = await render(AppComponent, {
    providers: [
      provideI18n(withLocaleId('fr'), withTranslations('fr', frTranslations))
    ]
  });

  getByText('Bienvenue');
});
```

</details>

---


## Common Pitfalls

* ❌ Forgetting `registerLocaleData()` → wrong date/number formats
* ❌ Mixing runtime + build‑time i18n incorrectly
* ❌ Missing `i18n-*` attribute for attributes
* ❌ Not providing unique `@@ids`
