---
id: component-queries
title: Referencing Component Children with Queries
sidebar_position: 12
---

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
import { Component, Directive, ElementRef, signal, TemplateRef, viewChild } from '@angular/core';

@Directive({
  selector: '[highlight]',
  standalone: true
})
export class HighlightDirective {
  color = 'yellow';
}

@Component({
  selector: 'child-counter',
  standalone: true,
  template: `Count: {{count()}}`
})
export class CounterComponent {
  count = signal(0);
  inc() { this.count.update(v => v + 1); }
}

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [CounterComponent, HighlightDirective],
  template: `
    <!-- Signal ViewChild: template ref -->
    <input #username type="text" placeholder="Type here..." />

    <!-- Signal ViewChild: child component -->
    <child-counter></child-counter>

    <!-- Signal ViewChild: directive -->
    <p highlight>Highlighted text</p>

    <!-- Signal ViewChild: Template -->
    <ng-template #tpl>
      <p>This is inside ng-template 😎</p>
    </ng-template>

    <button (click)="run()">Run Demo</button>
  `
})
export class App {

  // 1️⃣ ElementRef via template reference
  username = viewChild<ElementRef<HTMLInputElement>>('username');

  // 2️⃣ Child component
  counter = viewChild.required(CounterComponent);

  // 3️⃣ Directive
  highlight = viewChild(HighlightDirective);

  // 4️⃣ TemplateRef
  tpl = viewChild<TemplateRef<any>>('tpl');

  run() {
    console.log('Input value =', this.username()?.nativeElement.value);

    this.counter().inc();
    console.log('Counter =', this.counter().count());

    console.log('Highlight color =', this.highlight()?.color);

    console.log('TemplateRef exists =', !!this.tpl());
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
import { Component, Directive, ElementRef, QueryList, TemplateRef, ViewContainerRef, inject, viewChildren, signal } from '@angular/core';

@Directive({
  selector: '[highlight]',
  standalone: true
})
export class HighlightDirective {
  color = 'yellow';
}

@Component({
  selector: 'tag-pill',
  standalone: true,
  template: `<span class="pill">{{label}}</span>`,
  styles: [`.pill{padding:4px 10px;border-radius:20px;background:#eee;margin-right:6px;display:inline-block}`]
})
export class TagPillComponent {
  label = 'Tag';
}

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [TagPillComponent, HighlightDirective],
  template: `
    <!-- 1️⃣ ViewChildren: template refs -->
    <p #item>Apple</p>
    <p #item>Banana</p>
    <p #item>Cherry</p>

    <!-- 2️⃣ ViewChildren: components -->
    <tag-pill></tag-pill>
    <tag-pill></tag-pill>

    <!-- 3️⃣ ViewChildren: directives -->
    <p highlight>First</p>
    <p highlight>Second</p>

    <button (click)="run()">Run Demo</button>
  `
})
export class AppComponent {

  // 1️⃣ All ElementRefs via template reference
  items = viewChildren<ElementRef<HTMLParagraphElement>>('item');

  // 2️⃣ All child components
  tagPills = viewChildren(TagPillComponent);

  // 3️⃣ All directive instances
  highlights = viewChildren(HighlightDirective);

  run() {
    console.log('--- ElementRefs ---');
    this.items().forEach(el => console.log(el.nativeElement.textContent?.trim()));

    console.log('--- Components ---');
    this.tagPills().forEach(cmp => console.log(cmp.label));

    console.log('--- Directives ---');
    this.highlights().forEach(dir => console.log(dir.color));
  }
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
import { Component, Directive, ElementRef, contentChild, contentChildren, viewChild } from '@angular/core';

/* ---------- A directive we will project ---------- */
@Directive({
  selector: '[highlight]',
  standalone: true
})
export class HighlightDirective {
  color = 'orange';
}

/* ---------- Child component that ACCEPTS projected content ---------- */
@Component({
  selector: 'card-box',
  standalone: true,
  template: `
    <div class="box">
      <h3><ng-content select="[title]"></ng-content></h3>
      <section><ng-content></ng-content></section>
    </div>
  `,
  styles: [`.box{padding:12px;border-radius:10px;border:1px solid #ccc;margin-bottom:10px}`]
})
export class CardBoxComponent {

  // 1️⃣ Single projected element via template ref
  titleEl = contentChild<ElementRef<HTMLElement>>('title');

  // 2️⃣ First directive found in projected content
  highlight = contentChild(HighlightDirective);

  // 3️⃣ All directives found in projected content
  highlights = contentChildren(HighlightDirective);

  log() {
    console.log('Title text =', this.titleEl()?.nativeElement.textContent?.trim());

    console.log('First highlight color =', this.highlight()?.color);

    console.log('All highlights count =', this.highlights().length);
  }
}

/* ---------- PARENT projecting content ---------- */
@Component({
  selector: 'app-root',
  standalone: true,
  imports: [CardBoxComponent, HighlightDirective],
  template: `
    <card-box>
      <span #title title>Projected Card Title</span>

      <p>This paragraph is projected content.</p>

      <p highlight>Projected with directive 1</p>
      <p highlight>Projected with directive 2</p>
    </card-box>

    <button (click)="run()">Run Demo</button>
  `
})
export class App {

  // Grab the child component
  box = viewChild(CardBoxComponent);

  run() {
    console.log("run triggered");

    this.box()?.log();
  }
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
import { Component, Directive, ElementRef, TemplateRef, ViewChild } from '@angular/core';

@Directive({
  selector: '[highlight]',
  standalone: true
})
export class HighlightDirective {
  color = 'yellow';
}

@Component({
  selector: 'child-counter',
  standalone: true,
  template: `Count: {{count}}`
})
export class CounterComponent {
  count = 0;
  inc() { this.count++; }
}

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [CounterComponent, HighlightDirective],
  template: `
    <!-- ViewChild via template ref -->
    <input #username type="text" placeholder="Type here..." />

    <!-- ViewChild child component -->
    <child-counter></child-counter>

    <!-- ViewChild directive -->
    <p highlight>Highlighted text</p>

    <!-- ViewChild template -->
    <ng-template #tpl>
      <p>This is inside ng-template</p>
    </ng-template>

    <button (click)="run()">Run Demo</button>
  `
})
export class App {

  // 1️⃣ Template reference variable
  @ViewChild('username')
  username!: ElementRef<HTMLInputElement>;

  // 2️⃣ Child component
  @ViewChild(CounterComponent)
  counter!: CounterComponent;

  // 3️⃣ Directive instance
  @ViewChild(HighlightDirective)
  highlight!: HighlightDirective;

  // 4️⃣ TemplateRef
  @ViewChild('tpl')
  tpl!: TemplateRef<any>;

  run() {
    console.log('Input value =', this.username.nativeElement.value);

    this.counter.inc();
    console.log('Counter =', this.counter.count);

    console.log('Highlight color =', this.highlight.color);

    console.log('TemplateRef exists =', !!this.tpl);
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
