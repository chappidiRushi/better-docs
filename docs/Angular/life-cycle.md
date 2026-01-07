# Component Lifecycle

> When and how Angular creates, updates, and destroys a component

---

## Constructor

Runs when the component class is instantiated.

Used only for **dependency injection and simple initialization**. No bindings, inputs, or DOM are available.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-demo',
  standalone: true
})
export class DemoComponent {
  constructor(private service: DataService) {}
}
```

</details>

---

## `ngOnChanges`

Runs when one or more `@Input()` values change.

Called **before `ngOnInit`** and again on every subsequent input update.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-user',
  standalone: true
})
export class UserComponent implements OnChanges {
  @Input() id!: number;            // number

  ngOnChanges(changes: SimpleChanges) {
    console.log(changes['id'].currentValue);
  }
}
```

</details>

---

## `ngOnInit`

Runs once after the first input values are set.

Used for **initialization logic**, data loading, and subscriptions.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-dashboard',
  standalone: true
})
export class DashboardComponent implements OnInit {
  data!: string[];

  ngOnInit() {
    this.data = ['a', 'b', 'c'];    // any[] | Observable | Promise
  }
}
```

</details>

---

## `ngDoCheck`

Runs on every change detection cycle.

Used for **custom change detection logic** when default Angular checks are insufficient.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-check',
  standalone: true
})
export class CheckComponent implements DoCheck {
  ngDoCheck() {
    // manual dirty checking
  }
}
```

</details>

---

## `ngAfterContentInit`

Runs once after projected content (`ng-content`) is initialized.

Used when the component depends on **content-projected children**.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-wrapper',
  standalone: true,
  template: `<ng-content />`
})
export class WrapperComponent implements AfterContentInit {
  ngAfterContentInit() {}
}
```

</details>

---

## `ngAfterContentChecked`

Runs after every check of projected content.

Used for reacting to **content updates**.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-content-check',
  standalone: true
})
export class ContentCheckComponent implements AfterContentChecked {
  ngAfterContentChecked() {}
}
```

</details>

---

## `ngAfterViewInit`

Runs once after the component’s view and child views are initialized.

Safe place to access **`ViewChild` / DOM APIs**.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-modal',
  standalone: true,
  template: `<div #panel></div>`
})
export class ModalComponent implements AfterViewInit {
  @ViewChild('panel') panel!: ElementRef; // ElementRef | TemplateRef

  ngAfterViewInit() {
    this.panel.nativeElement.focus();
  }
}
```

</details>

---

## `ngAfterViewChecked`

Runs after every check of the component’s view.

Used for **post-render logic**, avoid heavy work.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-view-check',
  standalone: true
})
export class ViewCheckComponent implements AfterViewChecked {
  ngAfterViewChecked() {}
}
```

</details>

---

## `ngOnDestroy`

Runs just before the component is destroyed.

Used for **cleanup**: unsubscribe, remove listeners, cancel timers.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-timer',
  standalone: true
})
export class TimerComponent implements OnDestroy {
  intervalId = setInterval(() => {}, 1000);

  ngOnDestroy() {
    clearInterval(this.intervalId);
  }
}
```

</details>

---

## Lifecycle Execution Order

Angular calls lifecycle hooks in this sequence:

1. constructor
2. ngOnChanges
3. ngOnInit
4. ngDoCheck
5. ngAfterContentInit
6. ngAfterContentChecked
7. ngAfterViewInit
8. ngAfterViewChecked
9. ngOnDestroy

---

## Modern Cleanup Pattern (Angular 16+)

Use `DestroyRef` for automatic teardown without implementing `OnDestroy`.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-stream',
  standalone: true
})
export class StreamComponent {
  private destroyRef = inject(DestroyRef);

  constructor() {
    const id = setInterval(() => {}, 1000);

    this.destroyRef.onDestroy(() => {
      clearInterval(id);
    });
  }
}
```

</details>

---

## Tips & Tricks to Remember Lifecycle Names and Order

### 1. Lifecycle Flow Mental Model

Think in **phases**, not individual hooks:

* **Create** → constructor → `OnChanges` → `OnInit`
* **Check** → `DoCheck`
* **Content** → `AfterContentInit` → `AfterContentChecked`
* **View** → `AfterViewInit` → `AfterViewChecked`
* **Destroy** → `OnDestroy`

If you remember the phases, the hook names become predictable.

---

### 2. Name Pattern Rule

Every lifecycle hook follows this structure:

```
ng + <When> + <What>
```

Examples:

* `ngAfterViewInit` → after + view + init
* `ngAfterContentChecked` → after + content + checked

If you know **when** and **what**, you can reconstruct the name.

---

### 3. Content vs View Shortcut

* **Content** → `ng-content` (projected from parent)
* **View** → component’s own template

Rule of thumb:

* Using `ng-content` → Content hooks
* Using `ViewChild` → View hooks

---

### 4. Init vs Checked

* `Init` → runs **once**
* `Checked` → runs **every change detection**

This applies to both Content and View hooks.

---

### 5. One-Line Mnemonic (Order)

```
Create → Change → Init → Check → Content → View → Destroy
```

Expanded:

```
constructor
ngOnChanges
ngOnInit
ngDoCheck
ngAfterContentInit
ngAfterContentChecked
ngAfterViewInit
ngAfterViewChecked
ngOnDestroy
```

---

### 6. Practical Memory Tip

If Angular lets you safely access it:

* **Inputs** → `OnChanges` / `OnInit`
* **Projected content** → `AfterContent*`
* **DOM / ViewChild** → `AfterViewInit`
* **Cleanup** → `OnDestroy`

If access is unsafe → you’re too early in the lifecycle.
