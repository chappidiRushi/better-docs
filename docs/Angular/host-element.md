---
id: host-elements-cheatsheet
title: Host Elements CheatSheet
sidebar_position: 13
---

# Host Elements CheatSheet

> How a component interacts with its own DOM element using modern Angular APIs

---

## Host Element

The **host element** is the DOM element that matches a component’s selector. Angular instantiates the component and renders its view inside this element.

It is the surface where attributes, styles, classes, and events are applied.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-user',
  standalone: true
})
export class UserComponent {}
```

```html
<app-user></app-user> <!-- host element -->
```

</details>

---

## Host Bindings

Use the `host` property on `@Component()` to bind attributes, properties, classes, styles, and events directly to the host element.

This is the **only recommended API** for host interaction in Angular 19+.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-slider',
  standalone: true,
  host: {
    'role': 'slider',                         // static attribute

    '[attr.aria-valuenow]': 'value',          // string | number
    '[class.active]': 'active',               // boolean
    '[style.width.px]': 'width',              // number
    '[style.backgroundColor]': 'bg',          // string

    '(click)': 'toggle()',                    // MouseEvent
    '(keydown.enter)': 'increment()',         // KeyboardEvent
    '(document:keydown.escape)': 'reset()',  // KeyboardEvent
    '(window:resize)': 'onResize($event)'    // UIEvent
  }
})
export class SliderComponent {
  value = 50;
  width = 200;
  active = true;
  bg = 'white';

  toggle() {
    this.active = !this.active;
    this.bg = this.active ? '#d1e7dd' : 'white';
  }

  increment() { this.value++; }

  reset() {
    this.value = 0;
    this.active = false;
    this.bg = 'white';
  }

  onResize(event: UIEvent) {}
}
```

</details>

---

## Binding Precedence

When the same host property is bound in multiple places, Angular resolves conflicts in this order:

1. Dynamic bindings override static bindings
2. Template bindings override static host bindings
3. Component host bindings override template dynamic bindings

<details>
<summary>Example</summary>

```html
<app-profile role="group" [id]="externalId"></app-profile>
```

```ts
@Component({
  selector: 'app-profile',
  standalone: true,
  host: {
    'role': 'presentation',
    '[id]': 'internalId'
  }
})
export class ProfileComponent {
  internalId = 'profile-1';
}
```

</details>

---

## Injecting Static Host Attributes

Static attributes on the host element can be read using `HostAttributeToken` with `inject()`.

Useful for lightweight configuration without inputs.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-button',
  standalone: true
})
export class ButtonComponent {
  variant = inject(new HostAttributeToken('variant')); // string | null
}
```

```html
<app-button variant="primary"></app-button>
```

</details>

---

## Legacy Host APIs (Old Syntax)

> ⚠️ Kept for reference only. Not recommended for new code in Angular 19+.

Before the `host` property became the preferred API, Angular provided decorator-based host bindings.

These APIs still work but are considered **legacy**.

---

### `@HostBinding`

Binds a class, style, property, or attribute on the host element to a component field or getter.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-card'
})
export class CardComponent {
  @HostBinding('class.selected')
  selected = false;                 // boolean

  @HostBinding('attr.role')
  role = 'region';                  // string

  @HostBinding('style.opacity')
  get opacity(): number {            // number | string
    return this.selected ? 1 : 0.5;
  }
}
```

</details>

---

### `@HostListener`

Listens to events emitted by the host element, `document`, or `window`.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-panel'
})
export class PanelComponent {
  @HostListener('click')
  onClick() {}

  @HostListener('document:keydown.escape')
  onEscape() {}

  @HostListener('window:resize', ['$event'])
  onResize(event: UIEvent) {}
}
```

</details>
