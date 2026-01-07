# Referencing Component Children with Queries

> How a component references elements, directives, and child components using the **modern query APIs**

---

## What Are Queries

Queries let a component **reference its children** created by Angular.

They are used to:

* call child component APIs
* access DOM elements
* coordinate layout or behavior

Queries expose **read-only references** that stay in sync with the rendered view.

---

## Query Types (Mental Model)

Queries are categorized by **where the child comes from**:

* **View queries** → declared inside the component’s own template
* **Content queries** → projected into the component using `ng-content`

If the element is written in the template → **view query**
If the element comes from the parent → **content query**

---

## View Queries

Use `viewChild()` and `viewChildren()` to reference children declared in the component template.

They return **signals**, not decorators.

---

### `viewChild()`

Returns the **first matching child** from the component view.

Resolves automatically after view initialization.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-modal',
  standalone: true,
  template: `<div #panel></div>`
})
export class ModalComponent {
  panel = viewChild<ElementRef>('panel'); // Signal<ElementRef | undefined>

  focus() {
    this.panel()?.nativeElement.focus();
  }
}
```

</details>

---

### `viewChildren()`

Returns **all matching children** from the component view.

Always stays up to date when the view changes.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-list',
  standalone: true,
  template: `<item *ngFor="let i of items" />`
})
export class ListComponent {
  items = viewChildren(ItemComponent); // Signal<readonly ItemComponent[]>
}
```

</details>

---

## Content Queries

Use `contentChild()` and `contentChildren()` to reference **projected content**.

---

### `contentChild()`

Returns the **first projected child** that matches the selector.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-card',
  standalone: true,
  template: `<ng-content />`
})
export class CardComponent {
  title = contentChild(TitleDirective); // Signal<TitleDirective | undefined>
}
```

</details>

---

### `contentChildren()`

Returns **all projected children** matching a selector.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-tabs',
  standalone: true,
  template: `<ng-content />`
})
export class TabsComponent {
  tabs = contentChildren(TabComponent); // Signal<readonly TabComponent[]>
}
```

</details>

---

## Query Selectors

Modern queries accept the same selector types:

* template reference (`'ref'`)
* component class
* directive class
* `ElementRef`
* `TemplateRef`

<details>
<summary>Example</summary>

```ts
viewChild('ref');
viewChild(MyComponent);
viewChild(MyDirective);
viewChild(ElementRef);
viewChild(TemplateRef);
```

</details>

---

## Why Signal-Based Queries

* No lifecycle hooks required
* Always up to date
* Lazy and safe access
* Works naturally with computed and effects

---

## Deprecated: Decorator-Based Queries

> ⚠️ Legacy API. Avoid in new Angular 19+ code.

Before signal-based queries, Angular used decorators and `QueryList`.

They still work but are **not recommended** going forward.

---

### `@ViewChild` / `@ViewChildren`

Access children declared in the component template.

<details>
<summary>Example</summary>

```ts
@Component({ selector: 'app-modal' })
export class ModalComponent implements AfterViewInit {
  @ViewChild('panel') panel!: ElementRef;

  ngAfterViewInit() {
    this.panel.nativeElement.focus();
  }
}
```

</details>

---

### `@ContentChild` / `@ContentChildren`

Access projected content provided via `ng-content`.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-tabs',
  template: `<ng-content />`
})
export class TabsComponent implements AfterContentInit {
  @ContentChildren(TabComponent)
  tabs!: QueryList<TabComponent>;

  ngAfterContentInit() {
    console.log(this.tabs.length);
  }
}
```

</details>

---

## Migration Shortcut

| Legacy             | Angular 19+         |
| ------------------ | ------------------- |
| `@ViewChild`       | `viewChild()`       |
| `@ViewChildren`    | `viewChildren()`    |
| `@ContentChild`    | `contentChild()`    |
| `@ContentChildren` | `contentChildren()` |

---

## Final Mental Model

* **View** → what the component renders
* **Content** → what the parent provides
* **Single** → `*Child()`
* **Multiple** → `*Children()`
* **Modern Angular** → signals, not decorators
