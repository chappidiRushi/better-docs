# Passing Data Between Components

> Signal‑based APIs — no decorators

---

## input()

```ts
input<T>(options?)           // receives data from parent as a signal
```

<details>
<summary>Example</summary>

```ts
import { Component, input } from '@angular/core';

@Component({
  selector: 'app-child-input',
  standalone: true,
  template: `
    <p>{{ name() }}</p>
    <p>{{ age() }}</p>
  `
})
export class ChildInputComponent {

  // basic input
  name = input<string>();
  // input<string>({ required: true })
  // input<string | null>()


  // input with transformer
  age = input<number, string>({
    transform: (value) => Number(value) // string → number
  });
  // transform: (value: unknown) => T
  // input<boolean, string>({ transform: v => v === 'true' })
}
```

</details>

---

## output()

```ts
output<T>()                  // emits values from child to parent
```

<details>
<summary>Example</summary>

```ts
import { Component, output } from '@angular/core';

@Component({
  selector: 'app-child-output',
  standalone: true,
  template: `
    <button (click)="emit()">Emit</button>
  `
})
export class ChildOutputComponent {

  clicked = output<string>();
  // output<number>()
  // output<boolean>()
  // output<string | number>()

  emit() {
    this.clicked.emit('clicked');
  }
}
```

</details>

---

## model()

```ts
model<T>(options?)           // two-way bound input + output signal
```

<details>
<summary>Example</summary>

```ts
import { Component, model } from '@angular/core';

@Component({
  selector: 'app-child-model',
  standalone: true,
  template: `
    <input type="number" [(ngModel)]="count" />
  `
})
export class ChildModelComponent {

  count = model<number>(0);
  // model<number>()
  // model<number>({ required: true })
  // model<number>({ alias: 'value' })
}
```

</details>

---

## Parent Usage

```ts
@Component({
  selector: 'app-parent',
  standalone: true,
  imports: [
    ChildInputComponent,
    ChildOutputComponent,
    ChildModelComponent
  ],
  template: `
    <app-child-input [name]="username" />

    <app-child-output (clicked)="onClicked($event)" />

    <app-child-model [(count)]="counter" />
  `
})
export class ParentComponent {
  username = 'Angular';
  counter = 0;

  onClicked(value: string) {}
}
```
