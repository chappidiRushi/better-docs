# Programmatic Rendering

> How to create, insert, and destroy views and components imperatively

---

## What is Programmatic Rendering

Programmatic rendering means **creating and controlling views or components using code**, not templates.

Angular exposes APIs to:

* create embedded views from templates
* create component instances dynamically
* attach and detach views manually

---

## Core Building Blocks

Programmatic rendering is built on three core primitives:

* `ViewContainerRef` → insertion point
* `TemplateRef` → blueprint for embedded views
* `ComponentRef` → handle to a created component

---

## Rendering a Template (`TemplateRef`)

`TemplateRef` represents an `<ng-template>`.

It can be instantiated multiple times as embedded views.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-template-render',
  standalone: true,
  template: `
    <ng-template #tpl let-name="name">
      Hello {{ name }}
    </ng-template>
  `
})
export class TemplateRenderComponent {
  vc = inject(ViewContainerRef);
  tpl = viewChild<TemplateRef<any>>('tpl');

  render() {
    this.vc.createEmbeddedView(this.tpl()!, {
      name: 'Angular' // context object
    });
  }
}
```

</details>

---

## Clearing and Managing Views

`ViewContainerRef` manages a **list of views**.

It allows insertion, removal, and destruction.

<details>
<summary>Example</summary>

```ts
clear() {
  this.vc.clear();        // remove all views
}

removeFirst() {
  this.vc.remove(0);     // remove by index
}
```

</details>

---

## Programmatically Creating Components

Components can be instantiated without selectors using `createComponent()`.

Angular resolves dependencies automatically.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-host',
  standalone: true,
  template: ''
})
export class HostComponent {
  vc = inject(ViewContainerRef);

  create() {
    const ref = this.vc.createComponent(DynamicComponent);

    ref.instance.title = 'Hello'; // input
    ref.changeDetectorRef.detectChanges();
  }
}
```

</details>

---

## Accessing the Created Component

`ComponentRef` provides control over the component instance.

It exposes lifecycle, injector, and change detection.

<details>
<summary>Example</summary>

```ts
const ref = this.vc.createComponent(DynamicComponent);

ref.instance.count = 1;              // component instance
ref.location.nativeElement.focus();  // host element
ref.destroy();                        // destroy component
```

</details>

---

## Inserting at Specific Positions

Views and components can be inserted at a specific index.

<details>
<summary>Example</summary>

```ts
this.vc.createComponent(DynamicComponent, {
  index: 0 // number
});
```

</details>

---

## Attaching Views Outside Containers

Views can be created and attached manually to the application.

Used for overlays, modals, and portals.

<details>
<summary>Example</summary>

```ts
const view = this.tpl().createEmbeddedView({});
appRef.attachView(view);
document.body.append(...view.rootNodes);
```

</details>

---

## When Programmatic Rendering Runs

* Components are created immediately
* Change detection runs on insertion
* Destruction happens when removed or manually destroyed

---

## When to Use Programmatic Rendering

Use it for:

* dynamic layouts
* modals and overlays
* plugin-style architectures
* CMS-driven UIs

Avoid it for:

* static layouts
* simple conditional rendering

---

## Deprecated Pattern: `ComponentFactoryResolver`

> ⚠️ Legacy API. Do not use in Angular 19+

<details>
<summary>Example</summary>

```ts
const factory = resolver.resolveComponentFactory(MyComponent);
vc.createComponent(factory);
```

</details>

---

## Mental Model to Remember

* `TemplateRef` → blueprint
* `ViewContainerRef` → insertion point
* `ComponentRef` → live instance
* Templates render **views**
* Components render **behavior + view**
