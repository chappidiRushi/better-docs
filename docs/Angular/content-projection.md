# Content Projection

> Projecting content from a parent into a child component

---

## Content Projection API

```html
<ng-content></ng-content>                <!-- default slot -->
<ng-content select="selector"></ng-content> <!-- named slot -->
<ng-content select="[header], h1, .title, #title "></ng-content> <!-- multiple selectors -->

```

---

## Default Content Projection

Simple projection where all parent content is rendered at a single `<ng-content>` location.

<details>
<summary>Example</summary>

```ts
// CHILD COMPONENT
import { Component } from '@angular/core';

@Component({
  selector: 'app-card',
  standalone: true,
  template: `
    <div class="card">
      <ng-content></ng-content>
    </div>
  `
})
export class CardComponent {}


// PARENT USAGE
@Component({
  selector: 'app-parent',
  standalone: true,
  imports: [CardComponent],
  template: `
    <app-card>
      <h2>Title</h2>
      <p>Description</p>
    </app-card>
  `
})
export class ParentComponent {}
```

</details>

---

## Named Slots (Multiple Projection)

Projection split into multiple slots using selectors, allowing structured layouts.

<details>
<summary>Example</summary>

```ts
// CHILD COMPONENT
@Component({
  selector: 'app-layout',
  standalone: true,
  template: `
    <header>
      <ng-content select="[header]"></ng-content>
    </header>

    <main>
      <ng-content></ng-content>
    </main>

    <footer>
      <ng-content select="[footer]"></ng-content>
    </footer>
  `
})
export class LayoutComponent {}


// PARENT USAGE
@Component({
  selector: 'app-parent',
  standalone: true,
  imports: [LayoutComponent],
  template: `
    <app-layout>
      <h1 header>Header</h1>

      <p>Main content</p>

      <small footer>Footer</small>
    </app-layout>
  `
})
export class ParentComponent {}
```

</details>

---

## Element Selector Projection

Projects content based on HTML element names (e.g., `h1`, `p`).

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-panel',
  standalone: true,
  template: `
    <ng-content select="h3"></ng-content>
    <ng-content select="p"></ng-content>
  `
})
export class PanelComponent {}


@Component({
  selector: 'app-parent',
  standalone: true,
  imports: [PanelComponent],
  template: `
    <app-panel>
      <h3>Title</h3>
      <p>Description</p>
    </app-panel>
  `
})
export class ParentComponent {}
```

</details>

---

## Attribute Selector Projection

Projects content using custom attributes, the most common pattern for named slots.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-tabs',
  standalone: true,
  template: `
    <div class="tab-title">
      <ng-content select="[tab-title]"></ng-content>
    </div>
    <div class="tab-body">
      <ng-content select="[tab-body]"></ng-content>
    </div>
  `
})
export class TabsComponent {}


@Component({
  selector: 'app-parent',
  standalone: true,
  imports: [TabsComponent],
  template: `
    <app-tabs>
      <span tab-title>Profile</span>
      <div tab-body>Content</div>
    </app-tabs>
  `
})
export class ParentComponent {}
```

</details>

---

## Fallback Content

Default content rendered when the parent provides no projected nodes.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-empty',
  standalone: true,
  template: `
    <ng-content>
      <p>No content provided</p>
    </ng-content>
  `
})
export class EmptyComponent {}


@Component({
  selector: 'app-parent',
  standalone: true,
  imports: [EmptyComponent],
  template: `
    <app-empty></app-empty>
  `
})
export class ParentComponent {}
```

</details>

---

## Projection Order Rules

Angular matches projected nodes top-down based on the order of `<ng-content>` declarations.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-order',
  standalone: true,
  template: `
    <ng-content select="[first]"></ng-content>
    <ng-content select="[second]"></ng-content>
    <ng-content></ng-content>
  `
})
export class OrderComponent {}


@Component({
  selector: 'app-parent',
  standalone: true,
  imports: [OrderComponent],
  template: `
    <app-order>
      <p second>Second</p>
      <p>Default</p>
      <p first>First</p>
    </app-order>
  `
})
export class ParentComponent {}
```

</details>

---

## Accessing Projected Content — Queries

Programmatic access to projected content using signal-based content queries.

```ts
contentChild(selector)
contentChildren(selector)
```

---

## Access Single Projected Element

Access a single projected node matched by selector, directive, or template reference.

<details>
<summary>Example</summary>

```ts
import { Component, contentChild, ElementRef } from '@angular/core';

@Component({
  selector: 'app-single',
  standalone: true,
  template: `
    <ng-content select="[title]"></ng-content>
  `
})
export class SingleComponent {

  titleEl = contentChild<ElementRef>('titleRef');
  // contentChild(TemplateRef)
  // contentChild(MyDirective)
}


@Component({
  selector: 'app-parent',
  standalone: true,
  imports: [SingleComponent],
  template: `
    <app-single>
      <h1 title #titleRef>Projected Title</h1>
    </app-single>
  `
})
export class ParentComponent {}
```

</details>

---

## Access Multiple Projected Elements

Access all projected nodes that match a selector or directive as a reactive collection.

<details>
<summary>Example</summary>

```ts
import {
  Component,
  contentChildren,
  ElementRef,
  AfterContentInit,
} from '@angular/core';

/* ================= CHILD ================= */

@Component({
  selector: 'app-multi',
  standalone: true,
  template: `
    <ng-content></ng-content>
  `,
})
export class MultiComponent implements AfterContentInit {

  // ✅ contentChildren MUST query template refs (not tag names)
  items = contentChildren<ElementRef>('item');

  ngAfterContentInit() {
    console.log(
      'Projected <p> elements:',
      this.items().map(i => i.nativeElement.textContent)
    );
  }
}

/* ================= PARENT ================= */

@Component({
  selector: 'app-parent',
  standalone: true,
  imports: [MultiComponent],
  template: `
    <app-multi>
      <p #item>One</p>
      <p #item>Two</p>
    </app-multi>
  `,
})
export class ParentComponent {}

```

</details>

---

## Access Projected Template

Access and render a projected `ng-template` manually.

<details>
<summary>Example</summary>

```ts
import { Component, contentChild, TemplateRef } from '@angular/core';

@Component({
  selector: 'app-template-slot',
  standalone: true,
  template: `
    <ng-container *ngTemplateOutlet="tpl()"></ng-container>
  `
})
export class TemplateSlotComponent {

  tpl = contentChild<TemplateRef<any>>('tpl');
}


@Component({
  selector: 'app-parent',
  standalone: true,
  imports: [TemplateSlotComponent],
  template: `
    <app-template-slot>
      <ng-template #tpl>
        <p>Projected Template</p>
      </ng-template>
    </app-template-slot>
  `
})
export class ParentComponent {}
```

</details>
