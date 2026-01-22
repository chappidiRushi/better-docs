---
id: component-decorator
title: Component
sidebar_position: 5
---

# @Component

> `@Component()` decorator — complete metadata reference (single view)

---

## @Component

```ts
@Component({
  selector: string | string[],            // how Angular matches the component in templates

  template?: string,                      // inline HTML template
  templateUrl?: string,                   // external HTML file path

  styles?: string[],                      // inline component-scoped CSS
  styleUrls?: string[],                   // external CSS file paths

  changeDetection?: ChangeDetectionStrategy, // change detection mode

  encapsulation?: ViewEncapsulation,      // CSS scoping strategy

  host?: { [key: string]: string },       // bindings on the host element

  exportAs?: string | string[],           // template reference variable name(s)

  preserveWhitespaces?: boolean,          // preserve/remove whitespace in template

  standalone?: boolean                   // marks component as standalone
})
```

---

<details>
<summary>Example</summary>

```ts
import { Component, ChangeDetectionStrategy, ViewEncapsulation } from '@angular/core';

@Component({
  selector: 'app-example',
  // selector: ['app-a', 'app-b'],          // multiple selectors
  // selector: '[appExample]',              // attribute selector

  template: '<h1>Hello</h1>',
  // templateUrl: './example.component.html', // external template

  styles: [
    ':host { display: block; }',
    'h1 { color: red; }'
  ],
  // styleUrls: ['./example.component.css'], // external styles
  changeDetection: ChangeDetectionStrategy.OnPush, // ChangeDetectionStrategy.Default
  encapsulation: ViewEncapsulation.Emulated,  // ViewEncapsulation.None | ViewEncapsulation.ShadowDom
  host: {
    'role': 'region',                      // static attribute
    '[class.active]': 'true',              // property binding
    '(click)': 'onClick()'                 // event binding
  },
  exportAs: 'cmp',                         // exportAs: ['cmp', 'component'],
  preserveWhitespaces: false,             // true
  standalone: true                        // false
})
export class ExampleComponent {}
```

</details>
