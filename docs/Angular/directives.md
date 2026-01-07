# Angular Directives

---

## Attribute Directives

Attribute directives change the **appearance or behavior of an existing element**.

### `ngClass`

Sets or toggles CSS classes dynamically based on a bound expression.

<details>
<summary>Example</summary>

```ts
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-ngclass-demo',
  standalone: true,
  imports: [CommonModule],
  template: `
    <p [ngClass]="'static-class'">String value</p> <!-- string -->

    <p [ngClass]="['a','b']">Array value</p> <!-- string[] -->

    <p [ngClass]="{ active: isActive, disabled: isDisabled }">Object value</p> <!-- { [class: string]: boolean } -->

    <button (click)="isActive = !isActive">Toggle</button>
  `
})
export class NgClassDemoComponent {
  isActive = true;  // boolean
  isDisabled = false; // boolean
}
```

</details>

---

### `ngStyle`

Applies inline CSS styles dynamically to an element.

<details>
<summary>Example</summary>

```ts
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-ngstyle-demo',
  standalone: true,
  imports: [CommonModule],
  template: `
    <p [ngStyle]="{ color: color, fontSize: size + 'px' }">Styled text</p> <!-- { [prop: string]: string|number|null } -->

    <button (click)="size = size + 2">Grow</button>
  `
})
export class NgStyleDemoComponent {
  color = 'red';   // string
  size = 14;       // number
}
```

</details>

---

### `ngModel`

Creates two‑way binding between a form control and a component property.

<details>
<summary>Example</summary>

```ts
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';

@Component({
  selector: 'app-ngmodel-demo',
  standalone: true,
  imports: [FormsModule],
  template: `
    <input [(ngModel)]="name"> <!-- string -->
    <p>Hello {{ name }}</p>

    <input [(ngModel)]="age" type="number"> <!-- number -->
    <p>Age: {{ age }}</p>
  `
})
export class NgModelDemoComponent {
  name = 'Alex'; // string
  age = 25;      // number
}
```

</details>

---

## Structural Directives (Angular 19+ Control Flow)

Structural directives **add, remove, or reorder elements in the DOM tree**.

In Angular 19+, the **recommended syntax** is:

* `@if` instead of `*ngIf`
* `@for` instead of `*ngFor`
* `@switch` instead of `*ngSwitch`

The old `*` syntax still works, but these are preferred.

---

### `@if`

Conditionally includes a template block when the expression is truthy.

<details>
<summary>Example</summary>

```ts
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-if-demo',
  standalone: true,
  imports: [CommonModule],
  template: `
    <button (click)="show = !show">Toggle</button>

    @if (show) {
      <p>Visible Content</p> <!-- boolean -->
    }

    @if (user; as u) {
      <p>Hello {{ u.name }}</p> <!-- truthy aliasing -->
    } @else {
      <p>Guest</p>
    }
  `
})
export class IfDemoComponent {
  show = true; // boolean
  user: { name: string } | null = { name: 'Alex' }; // object | null
}
```

</details>

---

### `@for`

Repeats a template once per item in a collection.

<details>
<summary>Example</summary>

```ts
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-for-demo',
  standalone: true,
  imports: [CommonModule],
  template: `
    <ul>
      @for (item of items; track item; let i = $index; let isFirst = $first; let isLast = $last; let isEven = $even; let isOdd = $odd) {
        <li>
          {{ i }} — {{ item }}
          @if (isFirst) { <span>(first)</span> }
          @if (isLast) { <span>(last)</span> }
          @if (isEven) { <span>(even)</span> }
        </li>
      }
    </ul>
  `
})
export class ForDemoComponent {
  items = ['A','B','C']; // string[]
}
```

</details>

---

### `@switch`, `@case`, `@default`

Displays the first matching case template for the switch value.

<details>
<summary>Example</summary>

```ts
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-switch-demo',
  standalone: true,
  imports: [CommonModule],
  template: `
    @switch (role) {
      @case ('admin') { <p>Admin</p> }
      @case ('user')  { <p>User</p> }
      @default        { <p>Guest</p> }
    }
  `
})
export class SwitchDemoComponent {
  role: 'admin' | 'user' | 'guest' = 'user';
}
```

</details>

---

## Legacy Structural Directives (Still Supported)

These still work but are **not the recommended syntax in Angular 19+**:

* `*ngIf`
* `*ngFor`
* `*ngSwitch` / `*ngSwitchCase` / `*ngSwitchDefault`

Use them only when needed for backwards compatibility.

---

## Custom Attribute Directive

A custom attribute directive attaches reusable behavior to a host element.

<details>
<summary>Example</summary>

```ts
import { Directive, ElementRef, HostListener, Component } from '@angular/core';

@Directive({
  selector: '[appHighlight]',
  standalone: true
})
export class HighlightDirective {
  constructor(private el: ElementRef) {}

  @HostListener('mouseenter')
  onEnter() {
    this.el.nativeElement.style.background = 'yellow';
  }

  @HostListener('mouseleave')
  onLeave() {
    this.el.nativeElement.style.background = '';
  }
}

@Component({
  selector: 'app-custom-dir-demo',
  standalone: true,
  imports: [HighlightDirective],
  template: `
    <p appHighlight>Hover me</p>
  `
})
export class CustomDirDemoComponent {}
```

</details>

---
