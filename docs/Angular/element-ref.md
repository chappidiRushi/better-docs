# Using DOM APIs with `inject(ElementRef)`

> How to safely access and interact with the DOM using Angular’s modern dependency injection API

---

## What is `ElementRef`

`ElementRef` is a wrapper around a native DOM element.

It provides direct access to the element rendered by Angular for a component or directive.

Use it when you need to:

* read element dimensions
* focus or scroll elements
* integrate with third‑party DOM libraries

---

## Injecting `ElementRef`

In Angular 19+, `ElementRef` is accessed using the `inject()` function instead of constructor injection.

This gives access to the **host element** of the component or directive.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-box',
  standalone: true
})
export class BoxComponent {
  el = inject(ElementRef); // ElementRef<HTMLElement>
}
```

</details>

---

## Accessing the Native Element

The actual DOM node is available via the `nativeElement` property.

This is the element matched by the component’s selector.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-focus',
  standalone: true
})
export class FocusComponent {
  el = inject(ElementRef);

  focus() {
    this.el.nativeElement.focus(); // HTMLElement
  }
}
```

</details>

---

## Reading Layout Information

`ElementRef` can be used to read layout and geometry data from the DOM.

Common use cases include measuring size or position.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-measure',
  standalone: true
})
export class MeasureComponent {
  el = inject(ElementRef);

  logSize() {
    const rect = this.el.nativeElement.getBoundingClientRect();
    console.log(rect.width, rect.height); // number
  }
}
```

</details>

---

## Modifying DOM Properties

You can directly modify native properties on the element.

This includes styles, attributes, and focus state.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-highlight',
  standalone: true
})
export class HighlightComponent {
  el = inject(ElementRef);

  highlight() {
    this.el.nativeElement.style.backgroundColor = 'yellow'; // string
    this.el.nativeElement.setAttribute('tabindex', '0');    // string
  }
}
```

</details>

---

## When `ElementRef` Is Available

The host element exists **as soon as the component is created**.

This means `inject(ElementRef)` is safe to use:

* in class fields
* in the constructor
* in lifecycle hooks

DOM **children** are only guaranteed after view initialization.

---

## Using `ElementRef` with View Queries

`ElementRef` is often combined with `viewChild()` to access **child elements**, not just the host.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-input',
  standalone: true,
  template: `<input #ctrl />`
})
export class InputComponent {
  input = viewChild<ElementRef>('ctrl'); // Signal<ElementRef | undefined>

  focus() {
    this.input()?.nativeElement.focus();
  }
}
```

</details>

---

## Risks and Best Practices

Direct DOM access:

* bypasses Angular’s rendering abstraction
* may break server-side rendering
* may not work with Web Workers

Use `ElementRef`:

* sparingly
* only for read or small, isolated writes
* prefer Angular bindings when possible

---

## Deprecated Pattern: Constructor Injection

> ⚠️ Legacy style. Avoid in Angular 19+.

Before `inject()`, `ElementRef` was commonly injected via the constructor.

<details>
<summary>Example</summary>

```ts
@Component({ selector: 'app-legacy' })
export class LegacyComponent {
  constructor(private el: ElementRef) {}
}
```

</details>

---

## Mental Model to Remember

* `ElementRef` → direct access to **one DOM element**
* `inject(ElementRef)` → host element
* `viewChild(ElementRef)` → child element
* Prefer bindings → fallback to DOM APIs
