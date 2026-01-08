# Angular Directives

---

## Attribute Directives

Attribute directives change the **appearance or behavior of an existing element**.

### `ngClass`

Sets or toggles CSS classes dynamically based on a bound expression.
---

### `ngStyle`

Applies inline CSS styles dynamically to an element.

---

### `ngModel`

Creates two‑way binding between a form control and a component property.

---

## Structural Directives (Angular 19+ Control Flow)

Structural directives **add, remove, or reorder elements in the DOM tree**.

In Angular 19+, the **recommended syntax** is:

* `@if` instead of `*ngIf`
* `@for` instead of `*ngFor`
* `@switch` instead of `*ngSwitch`

The old `*` syntax still works, but these are preferred.

---

## Legacy Structural Directives (Still Supported)

These still work but are **not the recommended syntax in Angular 19+**:

* `*ngIf`
* `*ngFor`
* `*ngSwitch` / `*ngSwitchCase` / `*ngSwitchDefault`

Use them only when needed for backwards compatibility.

---

## Attribute Directives (Custom)

### `appHighlight` (Hover Highlight)

<details>
<summary>Example</summary>

```ts
import { Directive, ElementRef, inject, Component } from '@angular/core';

@Directive({
  selector: '[appHighlight]',
  host: {
    '(mouseenter)': 'onMouseEnter()',
    '(mouseleave)': 'onMouseLeave()',
  },
  standalone: true
})
export class HighlightDirective {
  private el = inject(ElementRef);

  onMouseEnter() { this.highlight('yellow'); }
  onMouseLeave() { this.highlight(''); }

  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}

@Component({
  selector: 'app-highlight-demo',
  standalone: true,
  imports: [HighlightDirective],
  template: `<p appHighlight>Hover over me!</p>`
})
export class HighlightDemoComponent {}
```

</details>

---

### `appTooltip` (Dynamic Tooltip)

<details>
<summary>Example</summary>

```ts
import { Directive, ElementRef, Input, inject, Component } from '@angular/core';

@Directive({
  selector: '[appTooltip]',
  host: {
    '(mouseenter)': 'show()',
    '(mouseleave)': 'hide()',
  },
  standalone: true
})
export class TooltipDirective {
  @Input() appTooltip = '';
  private tooltipEl?: HTMLElement;
  private el = inject(ElementRef);

  show() {
    this.tooltipEl = document.createElement('span');
    this.tooltipEl.textContent = this.appTooltip;
    this.tooltipEl.style.position = 'absolute';
    this.tooltipEl.style.background = '#333';
    this.tooltipEl.style.color = '#fff';
    this.tooltipEl.style.padding = '2px 6px';
    this.tooltipEl.style.borderRadius = '4px';
    this.el.nativeElement.appendChild(this.tooltipEl);
  }

  hide() { this.tooltipEl?.remove(); this.tooltipEl = undefined; }
}

@Component({
  selector: 'app-tooltip-demo',
  standalone: true,
  imports: [TooltipDirective],
  template: `<button appTooltip="Click me!">Hover me</button>`
})
export class TooltipDemoComponent {}
```

</details>

---

## Structural Directives (Custom)

### `@if` (Custom Structural Conditional)

<details>
<summary>Example</summary>

```ts
import { Directive, Input, TemplateRef, ViewContainerRef, inject, Component } from '@angular/core';

@Directive({ selector: '[appIf]', standalone: true })
export class AppIfDirective {
  private hasView = false;
  private templateRef = inject(TemplateRef<any>);
  private vcRef = inject(ViewContainerRef);

  @Input() set appIf(condition: boolean) {
    if (condition && !this.hasView) {
      this.vcRef.createEmbeddedView(this.templateRef);
      this.hasView = true;
    } else if (!condition && this.hasView) {
      this.vcRef.clear();
      this.hasView = false;
    }
  }
}

@Component({
  selector: 'app-if-demo',
  standalone: true,
  imports: [AppIfDirective],
  template: `
    <p *appIf="show">This text is conditionally rendered!</p>
    <button (click)="show = !show">Toggle</button>
  `
})
export class IfDemoComponent {
  show = true;
}
```

</details>

---

### `@for` (Custom Structural Loop)

<details>
<summary>Example</summary>

```ts
import { Directive, Input, TemplateRef, ViewContainerRef, inject, Component } from '@angular/core';

@Directive({ selector: '[appFor]', standalone: true })
export class AppForDirective<T> {
  private templateRef = inject(TemplateRef<any>);
  private vcRef = inject(ViewContainerRef);

  @Input() set appFor(items: T[]) {
    this.vcRef.clear();
    items.forEach((item, i) => {
      this.vcRef.createEmbeddedView(this.templateRef, { $implicit: item, index: i });
    });
  }
}

@Component({
  selector: 'app-for-demo',
  standalone: true,
  imports: [AppForDirective],
  template: `
    <ul>
      <li *appFor="let item of items; let i = index">{{ i }} — {{ item }}</li>
    </ul>
  `
})
export class ForDemoComponent {
  items = ['A', 'B', 'C'];
}
```

</details>

---

## Attribute & Structural Directives — Deprecated

### `appHighlight` Deprecated

<details>
<summary>Example</summary>

```ts
import { Directive, ElementRef, HostListener, Component } from '@angular/core';

@Directive({ selector: '[appHighlight]' })
export class HighlightDirectiveDeprecated {
  constructor(private el: ElementRef) {}

  @HostListener('mouseenter') onEnter() { this.el.nativeElement.style.backgroundColor = 'yellow'; }
  @HostListener('mouseleave') onLeave() { this.el.nativeElement.style.backgroundColor = ''; }
}

@Component({
  selector: 'app-highlight-demo-deprecated',
  standalone: true,
  imports: [HighlightDirectiveDeprecated],
  template: `<p appHighlight>Hover over me!</p>`
})
export class HighlightDemoDeprecatedComponent {}
```

</details>

### `appTooltip` Deprecated

<details>
<summary>Example</summary>

```ts
import { Directive, ElementRef, Input, HostListener, Component } from '@angular/core';

@Directive({ selector: '[appTooltip]' })
export class TooltipDirectiveDeprecated {
  @Input() appTooltip = '';
  private tooltipEl?: HTMLElement;

  constructor(private el: ElementRef) {}

  @HostListener('mouseenter') show() {
    this.tooltipEl = document.createElement('span');
    this.tooltipEl.textContent = this.appTooltip;
    this.tooltipEl.style.position = 'absolute';
    this.tooltipEl.style.background = '#333';
    this.tooltipEl.style.color = '#fff';
    this.tooltipEl.style.padding = '2px 6px';
    this.tooltipEl.style.borderRadius = '4px';
    this.el.nativeElement.appendChild(this.tooltipEl);
  }

  @HostListener('mouseleave') hide() { this.tooltipEl?.remove(); this.tooltipEl = undefined; }
}

@Component({
  selector: 'app-tooltip-demo-deprecated',
  standalone: true,
  imports: [TooltipDirectiveDeprecated],
  template: `<button appTooltip="Click me!">Hover me</button>`
})
export class TooltipDemoDeprecatedComponent {}
```

</details>
