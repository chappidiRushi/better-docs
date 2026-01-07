# Templates

> How Angular templates declare views and bind data

---

## What is a Template

A template defines **how a component renders its view**.

It describes:

* HTML structure
* data bindings
* control flow

Angular compiles templates into efficient rendering instructions.

---

## Inline vs External Templates

A template can be defined:

* inline using `template`
* in a separate file using `templateUrl`

Both produce the same result at runtime.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-inline',
  standalone: true,
  template: `<h1>Hello</h1>`        // inline template
  // templateUrl: './a.html'        // alternative: external file
})
export class InlineComponent {}
```

```ts
@Component({
  selector: 'app-external',
  standalone: true,
  templateUrl: './external.component.html' // external template
  // template: `<h1>Hello</h1>`             // alternative: inline
})
export class ExternalComponent {}
```

</details>

---

## Interpolation

Interpolation renders **component values as text** inside the template.

It evaluates expressions against the component instance.

<details>
<summary>Example</summary>

```ts
@Component({
  standalone: true,
  template: `
    Hello {{ name }}
    {{ count + 1 }}        <!-- expression -->
    {{ getTitle() }}       <!-- method call -->
  `
})
export class InterpolationComponent {
  name = 'Angular';       // string
  count = 0;              // number

  getTitle() {
    return 'Framework';   // string
  }
}
```

</details>

---

## Property Binding

Property binding sets **DOM properties** using component values.

It flows data **from component to view**.

<details>
<summary>Example</summary>

```ts
@Component({
  standalone: true,
  template: `
    <img [src]="url">            <!-- property binding -->
    <img bind-src="url">         <!-- alternative syntax -->

    <div [hidden]="isHidden"></div>
  `
})
export class PropertyBindingComponent {
  url = 'logo.png';       // string
  isHidden = false;       // boolean
}
```

</details>

---

## Attribute Binding

Attribute binding sets **HTML attributes** instead of DOM properties.

Used for ARIA, SVG, and non-property attributes.

<details>
<summary>Example</summary>

```ts
@Component({
  standalone: true,
  template: `
    <div [attr.aria-label]="label"></div>
    <!-- alternative: remove attribute when null -->
  `
})
export class AttributeBindingComponent {
  label = 'menu';         // string | null
}
```

</details>

---

## Class Binding

Class binding adds or removes CSS classes dynamically.

<details>
<summary>Example</summary>

```ts
@Component({
  standalone: true,
  template: `
    <div [class.active]="isActive"></div>
    <div [class]="className"></div>        <!-- string -->
    <div [class]="classMap"></div>         <!-- Record<string, boolean> -->
  `
})
export class ClassBindingComponent {
  isActive = true;                            // boolean
  className = 'box active';                   // string
  classMap = { active: true, disabled: false }; // object
}
```

</details>

---

## Style Binding

Style binding sets inline styles on elements.

<details>
<summary>Example</summary>

```ts
@Component({
  standalone: true,
  template: `
    <div [style.width.px]="width"></div>     <!-- number -->
    <div [style.width]="widthStr"></div>     <!-- string -->
    <div [style]="styleMap"></div>           <!-- object -->
  `
})
export class StyleBindingComponent {
  width = 200;                                // number
  widthStr = '50%';                           // string
  styleMap = { width: '100px', height: '50px' }; // object
}
```

</details>

---

## Event Binding

Event binding listens to DOM events and executes component logic.

Data flows **from view to component**.

<details>
<summary>Example</summary>

```ts
@Component({
  standalone: true,
  template: `
    <button (click)="increment()"></button>
    <button (click)="increment($event)"></button> <!-- event object -->
  `
})
export class EventBindingComponent {
  increment(event?: MouseEvent) {}
}
```

</details>

---

## Two-Way Binding

Two-way binding synchronizes **view and component state**.

It combines property and event binding.

<details>
<summary>Example</summary>

```ts
@Component({
  standalone: true,
  imports: [FormsModule],
  template: `
    <input [(ngModel)]="value">          <!-- shorthand -->
    <input [ngModel]="value" (ngModelChange)="value = $event"> <!-- expanded -->
  `
})
export class TwoWayBindingComponent {
  value = '';             // string
}
```

</details>

---

## Control Flow (`@if`, `@for`, `@switch`)

Templates control rendering using built-in control flow blocks.

These replace structural directives.

<details>
<summary>Example</summary>

```ts
@Component({
  standalone: true,
  template: `
    @if (isLoggedIn) {
      <p>Welcome</p>
    } @else {
      <p>Login</p>
    }

    @for (item of items; track item) {
      <div>{{ item }}</div>
    }

    @switch (role) {
      @case ('admin') { <p>Admin</p> }
      @default { <p>User</p> }
    }
  `
})
export class ControlFlowComponent {
  isLoggedIn = true;       // boolean
  items = ['a', 'b'];     // string[]
  role = 'admin';         // string
}
```

</details>

---

## Template Variables

Template variables reference elements or directives in the template.

They are scoped to the template block.

<details>
<summary>Example</summary>

```ts
@Component({
  standalone: true,
  template: `
    <input #ctrl>
    <button (click)="log(ctrl.value)"></button>

    <!-- alternative: export directive -->
    <form #f="ngForm"></form>
  `
})
export class TemplateVarComponent {
  log(value: string) {}
}
```

</details>

---

## `ng-template`

`<ng-template>` defines a **deferred or reusable template block**.

It does not render by itself.

<details>
<summary>Example</summary>

```ts
@Component({
  standalone: true,
  template: `
    <ng-template #empty>
      No data
    </ng-template>

    <!-- alternative usage via outlet -->
    <ng-container *ngTemplateOutlet="empty"></ng-container>
  `
})
export class NgTemplateComponent {}
```

</details>

---

## `@let` — Local Variables in Templates

`@let` creates a local variable inside your template without needing an `*ngIf`, `*ngFor`, or method call.

It helps avoid recomputation and keeps expressions readable.

```
@let variable = expression;
```

---

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-let-demo',
  template: `
    @let taxRate = 0.1;                 // number
    @let total = price + (price * taxRate); // number | computed value

    <p>Base Price: {{ price }}</p>
    <p>Tax: {{ price * taxRate }}</p>
    <p>Total: {{ total }}</p>

    @let formatted = total | currency:'USD':'symbol'; // string from pipe
    <p>Formatted Total: {{ formatted }}</p>
  `
})
export class LetDemoComponent {
  price = 200; // number
}
```

</details>

---

## `@defer` — Defer Loading & Rendering

`@defer` delays the loading and rendering of part of the template until a trigger condition happens.

This helps reduce initial bundle size and improves startup performance.

```
@defer (triggerOptions) {
  ... deferred content ...
} @placeholder {
  ... shown before loading ...
} @loading {
  ... shown during loading ...
} @error {
  ... shown if loading fails ...
}
```

### Trigger Options

```
on idle           // when browser is idle
on viewport       // when element scrolls into view
on timer(ms)      // after delay
on interaction    // user interacts with page
when condition    // boolean expression becomes true
prefetch          // fetch early but render later
```

---

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-defer-demo',
  standalone: true,
  imports: [],
  template: `
    <!-- Trigger when element enters viewport -->
    @defer (on viewport; prefetch) {
      <heavy-widget [data]="data"></heavy-widget>
    } @placeholder {
      <p>Widget will load soon...</p>
    } @loading {
      <p>Loading widget...</p>
    } @error {
      <p>Failed to load widget</p>
    }

    <!-- Trigger after user interaction -->
    @defer (on interaction) {
      <lazy-panel></lazy-panel>
    }

    <!-- Trigger after 2 seconds -->
    @defer (on timer(2000)) {
      <delayed-section></delayed-section>
    }

    <!-- Trigger when condition true -->
    @defer (when isReady) {
      <ready-content></ready-content>
    }

    <!-- Trigger when browser idle -->
    @defer (on idle) {
      <background-stuff></background-stuff>
    }
  `
})
export class DeferDemoComponent {
  data = { name: 'demo' }; // object
  isReady = false; // boolean
}
```

</details>

---

## `@let` + `@defer` Together

They can be combined so expensive work happens **only when content is rendered**.

---

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-combo-demo',
  template: `
    @defer (on viewport) {
      @let result = compute();
      <p>Computed: {{ result }}</p>
    } @placeholder {
      <p>Scrolling into view…</p>
    }
  `
})
export class ComboDemoComponent {
  compute() {
    return Math.random(); // number result
  }
}
```

</details>

---

## Deprecated Syntax

> ⚠️ Legacy structural directives

These are replaced by built-in control flow blocks.

<details>
<summary>Example</summary>

```html
<div *ngIf="visible"></div>
<div *ngFor="let item of items"></div>
```

</details>

---

## Mental Model to Remember

* Template = view declaration
* Bindings connect class ↔ DOM
* Same result, multiple syntaxes
* Prefer the most readable form
