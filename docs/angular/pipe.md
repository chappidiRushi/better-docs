---
id: pipes
title: Pipes
sidebar_position: 7
---

# Pipes

> Transform data in templates

---

## What is a Pipe

Transforms input to output in templates.

---

## Using Built-in Pipes

### Date Pipe

* Formats dates.
* `{{ value | date:format:timezone:locale }}`
* Options: `'short' | 'medium' | 'long' | 'fullDate' | 'shortTime'` + custom pattern, timezone, locale

<details>
<summary>Example</summary>
```ts
<p>{{ today | date }}</p>                            // default format
<p>{{ today | date:'short' }}</p>                     // predefined format
<p>{{ today | date:'fullDate' }}</p>
<p>{{ today | date:'yyyy/MM/dd' }}</p>                 // custom pattern
<p>{{ today | date:'fullDate':'UTC' }}</p>           // timezone
<p>{{ today | date:'fullDate':'UTC':'en-GB' }}</p>  // locale
```
</details>

### Currency Pipe

* Formats numbers as currency.
* `{{ value | currency:currencyCode:symbolDisplay:digitInfo:locale }}`
* Options: currency code, symbol/code, digits, locale

<details>
<summary>Example</summary>
```ts
<p>{{ amount | currency }}</p>                        // default USD
<p>{{ amount | currency:'USD' }}</p>
<p>{{ amount | currency:'EUR':'symbol' }}</p>
<p>{{ amount | currency:'JPY':'code':'1.0-0' }}</p>
<p>{{ amount | currency:'USD':'symbol':'1.2-2':'en-US' }}</p>
```
</details>

### Decimal Pipe

* Formats numbers.
* `{{ value | number:digitInfo:locale }}`
* Options: digit info, locale

<details>
<summary>Example</summary>
```ts
<p>{{ 3.14159 | number }}</p>                       // default
<p>{{ 3.14159 | number:'1.0-0' }}</p>               // minInt-minFrac-maxFrac
<p>{{ 3.14159 | number:'1.2-3' }}</p>
<p>{{ 3.14159 | number:'3.1-2':'en-US' }}</p>        // locale
```
</details>

### Percent Pipe

* Converts number to percent.
* `{{ value | percent:digitInfo:locale }}`

<details>
<summary>Example</summary>
```ts
<p>{{ 0.123 | percent }}</p>                          // default
<p>{{ 0.123 | percent:'1.0-0' }}</p>
<p>{{ 0.123 | percent:'1.2-2' }}</p>
<p>{{ 0.123 | percent:'1.2-2':'en-GB' }}</p>          // locale
```
</details>

### Uppercase / Lowercase

* Converts text.
* `{{ value | uppercase }}` or `{{ value | lowercase }}`

<details>
<summary>Example</summary>
```ts
<p>{{ 'Angular' | uppercase }}</p>
<p>{{ 'Angular' | lowercase }}</p>
```
</details>

### Json Pipe

* Converts object to JSON string.
* `{{ value | json }}`

<details>
<summary>Example</summary>
```ts
<p>{{ obj | json }}</p>
<p>{{ { a:1, b:2 } | json }}</p>
```
</details>

### Async Pipe

* Subscribes to Observable/Promise.
* `{{ obs$ | async }}`

<details>
<summary>Example</summary>
```ts
@Component({
  standalone: true,
  imports: [AsyncPipe],
  selector: 'app-async',
  template: `
    <h2>Async Pipe Examples</h2>

    <!-- 1️⃣ Observable with async pipe -->
    <p>Observable value: {{ obs$ | async }}</p>

    <!-- 2️⃣ Promise with async pipe -->
    <p>Promise value: {{ promise | async }}</p>
  `
})
export class AsyncComponent {
  // Observable emitting after 2 seconds
  obs$: Observable<string> = of('Hello from Observable!').pipe(delay(2000));

  // Promise resolving after 3 seconds
  promise: Promise<string> = new Promise(resolve => setTimeout(() => resolve('Hello from Promise!'), 3000));
}

```
</details>

---

## Custom Pipes

* Create with `@Pipe` + `PipeTransform`.
* Accept arguments as needed.

<details>
<summary>Example</summary>
```ts
@Pipe({ name: 'exclaim', standalone: true })
export class ExclaimPipe implements PipeTransform {
  transform(value: string, times: number = 1) {
    return value + '!'.repeat(times);
  }
}

@Component({
standalone: true,
imports: [ExclaimPipe],
template: ` <p>{{ 'Hello' | exclaim }}</p>        // default times=1     <p>{{ 'Hello' | exclaim:3 }}</p>      // times=3
  `
})
export class CustomPipeComponent {}

````
</details>

---

## Pure vs Impure Pipes
- Pure → runs only when input changes (default)
- Impure → runs on every change detection

<details>
<summary>Example</summary>
```ts
@Pipe({ name: 'impure', pure: false })
export class ImpurePipe implements PipeTransform {
  transform(value: any) { return value; }
}
````

</details>

---

## Chaining Pipes

* Combine multiple pipes in order.

<details>
<summary>Example</summary>
```ts
<p>{{ text | uppercase | slice:0:4 }}</p>
<p>{{ amount | currency:'USD':'symbol' | uppercase }}</p>
```
</details>

---

## Multiple Ways to Use Pipes

* Interpolation: `{{ value | pipe }}`
* ng-container: `<ng-container>{{ value | pipe }}</ng-container>`
* Chained: `{{ value | pipe1 | pipe2 }}`
* With async: `{{ obs$ | async | pipe }}`

---

## Deprecated / Discouraged

* Calling functions in templates is discouraged.

<details>
<summary>Example</summary>
```ts
<p>{{ format(value) }}</p>
```
</details>

---

## Mental Model

* Pipe = template-level transform
* Use built-in pipes whenever possible
* Custom pipes are reusable
* Chaining allows multiple transformations
* Pipes may accept arguments (options)
