---
id: template-driven-forms
title: Template-Driven Forms
sidebar_position: 24
---

# Template‑Driven Forms

> Mid‑level friendly — **focuses only on what each feature does**.
>
> Uses standalone components + `FormsModule`.

---

## ⭐ Overview

Template‑driven forms **let you declare form controls directly in the template** using `ngModel`, while Angular builds and manages the form model for you.

They are best for **simple forms with minimal logic in TypeScript**.

---

## 🧩 Setup (Standalone Components)

Import **FormsModule** where the form lives.

```ts
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';

@Component({
  selector: 'app-form',
  standalone: true,
  imports: [FormsModule],
  template: `...`
})
export class FormComponent {}
```

<details>
<summary>Example</summary>

```ts
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [FormsModule],
  template: `
    <form #myForm="ngForm">

      <input type="text" (ngSubmit)="onSubmit()" name="username" [(ngModel)]="username" required #usernameCtrl="ngModel"/>

      <div *ngIf="usernameCtrl.invalid && usernameCtrl.touched">
        <small *ngIf="usernameCtrl.errors?.['required']">Username is required
        </small>
      </div>
      <button [disabled]="myForm.invalid">Submit</button>
    </form>
  `
})
export class AppComponent {
  username = '';
  onSubmit(){
    console.log("form submitted")
  }
}

```

</details>

---

## Complete Example

```ts
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [FormsModule],
  template: `
    <!-- ngForm creates the top-level Angular form -->
    <form #userForm="ngForm" (ngSubmit)="onSubmit(userForm)">

      <!-- ================= BASIC INFO GROUP ================= -->
      <!-- ngModelGroup is REQUIRED to create nesting -->
      <fieldset ngModelGroup="basicInfo" #basicInfoGroup="ngModelGroup">
        <legend>Basic Info</legend>

        <!-- NAME FIELD -->
        <label>Name</label><br />
        <input
          type="text"

          name="name"
          <!-- name attribute is REQUIRED for ngModel to register -->

          [(ngModel)]="model.basicInfo.name"
          <!-- ngModel connects this input to Angular forms -->

          required
          <!-- required enables built-in validation -->

          minlength="3"
          <!-- minlength adds validation error -->

          #nameCtrl="ngModel"
          <!-- template reference to access validity & errors -->
        />

        <!-- Error messages must use ngModel reference -->
        <div *ngIf="nameCtrl.invalid && nameCtrl.touched">
          <small *ngIf="nameCtrl.errors?.['required']">
            Name is required
          </small>
          <small *ngIf="nameCtrl.errors?.['minlength']">
            Minimum 3 characters
          </small>
        </div>

        <br /><br />

        <!-- EMAIL FIELD -->
        <label>Email</label><br />
        <input
          type="email"
          name="email"
          [(ngModel)]="model.basicInfo.email"
          required
          email
          <!-- email activates Angular email validator -->
          #emailCtrl="ngModel"
        />

        <div *ngIf="emailCtrl.invalid && emailCtrl.touched">
          <small>Email must be valid</small>
        </div>

      </fieldset>

      <br />

      <!-- ================= ADDRESS GROUP ================= -->
      <!-- Without ngModelGroup, nesting will NOT happen -->
      <fieldset ngModelGroup="address" #addressGroup="ngModelGroup">
        <legend>Address</legend>

        <!-- STREET -->
        <label>Street</label><br />
        <input
          type="text"
          name="street"
          <!-- name must be unique INSIDE this group -->

          [(ngModel)]="model.address.street"
          required
          #streetCtrl="ngModel"
        />

        <div *ngIf="streetCtrl.invalid && streetCtrl.touched">
          <small>Street is required</small>
        </div>

        <br /><br />

        <!-- CITY -->
        <label>City</label><br />
        <input
          type="text"
          name="city"
          [(ngModel)]="model.address.city"
          required
          #cityCtrl="ngModel"
        />

        <div *ngIf="cityCtrl.invalid && cityCtrl.touched">
          <small>City is required</small>
        </div>

        <br /><br />

        <!-- COUNTRY SELECT -->
        <label>Country</label><br />
        <select
          name="country"
          <!-- name is REQUIRED or control is ignored -->

          [(ngModel)]="model.address.country"
          required
          #countryCtrl="ngModel"
        >
          <!-- Disabled option prevents valid selection -->
          <option value="" disabled>Select country</option>
          <option *ngFor="let c of countries" [value]="c">
            {{ c }}
          </option>
        </select>

        <div *ngIf="countryCtrl.invalid && countryCtrl.touched">
          <small>Country is required</small>
        </div>

        <!-- Group-level state -->
        <p>
          Address Group Valid: {{ addressGroup.valid }} <br />
          Address Group Touched: {{ addressGroup.touched }}
        </p>
      </fieldset>

      <br />

      <!-- ================= TERMS CHECKBOX ================= -->
      <label>
        <input
          type="checkbox"
          name="terms"

          [(ngModel)]="model.terms"
          <!-- required only passes when checkbox is true -->

          required
          #termsCtrl="ngModel"
        />
        Accept Terms
      </label>

      <div *ngIf="termsCtrl.invalid && termsCtrl.touched">
        <small>You must accept the terms</small>
      </div>

      <br /><br />

      <!-- ================= FORM STATE ================= -->
      <p>
        Form Valid: {{ userForm.valid }} <br />
        Form Touched: {{ userForm.touched }}
      </p>

      <!-- Submit disabled until entire form is valid -->
      <button type="submit" [disabled]="userForm.invalid">
        Submit
      </button>

    </form>
  `
})
export class AppComponent {

  // Model structure MUST match ngModelGroup hierarchy
  model = {
    basicInfo: {
      name: '',
      email: ''
    },
    address: {
      street: '',
      city: '',
      country: ''
    },
    terms: false
  };

  countries = ['USA', 'India', 'Germany'];

  onSubmit(form: any) {
    // form.value contains nested structure
    console.log('FORM VALUE', form.value);
  }
}
```

## directives Table

| Directive        | Purpose          |
| ---------------- | ---------------- |
| `ngForm`         | Root form        |
| `ngModel`        | Control binding  |
| `ngModelGroup`   | Nesting          |
| `ngModelOptions` | Control behavior |
| `ngSubmit`       | Submit handling  |
| `required`       | Validation       |
| `minlength`      | Validation       |
| `maxlength`      | Validation       |
| `pattern`        | Validation       |
| `email`          | Validation       |

## 📝 `ngModel` — Bind Form Controls

`ngModel` **creates and tracks a form control**, syncing value between view and model.

* `[(ngModel)]` → two‑way binding
* `name` attribute → registers control with the form
* `ngModel` without `name` → standalone control

<details>
<summary>Example</summary>

```html
<input
  [(ngModel)]="user.name"   <!-- string | number | boolean | object -->
  name="name"               <!-- required unless standalone: true -->
>

<input
  [ngModel]="age"           <!-- one‑way from ts → view -->
  (ngModelChange)="age=$event" <!-- manual two‑way -->
  name="age"
>

<input
  ngModel                    <!-- no binding, creates control -->
  name="note"
>

<input
  ngModel
  [ngModelOptions]="{standalone:true}"  <!-- not part of form model -->
>
```

</details>

---

## 🧠 Accessing the Form (`NgForm`)

Angular **creates an `NgForm` instance for any `<form>` with controls**.

Use a template reference to access it:

<details>
<summary>Example</summary>

```html
<form #f="ngForm" (ngSubmit)="save(f)">
  <input name="email" [(ngModel)]="email" required>
  <button>Save</button>
</form>

<p>Valid: {{ f.valid }}</p>
<p>Touched: {{ f.touched }}</p>
<p>Value: {{ f.value | json }}</p>
```

```ts
save(form: NgForm) {
  console.log(form.value);
}
```

</details>

---

## ✔️ Validation (Built‑in)

Built‑in validators **add validation rules and control states**.

* `required`
* `minlength="n"`
* `maxlength="n"`
* `min="n"`
* `max="n"`
* `pattern="regex"`
* `email`

<details>
<summary>Example</summary>

```html
<input name="email" [(ngModel)]="email" email required>

<input name="pwd" [(ngModel)]="pwd" minlength="6" maxlength="20" required>

<input name="age" [(ngModel)]="age" type="number" min="18" max="65">
```

</details>

---

## 🧩 Custom Validators (Directive)

Custom validators **run custom logic and mark a control invalid when needed**.

<details>
<summary>Example</summary>

```ts
import { Directive } from '@angular/core';
import { NG_VALIDATORS, Validator, AbstractControl } from '@angular/forms';

@Directive({
  selector: '[appForbidden]';
  providers: [{ provide: NG_VALIDATORS, useExisting: ForbiddenDirective, multi: true }]
})
export class ForbiddenDirective implements Validator {
  validate(control: AbstractControl) {
    return control.value === 'bad' ? { forbidden: true } : null; // error object | null
  }
}
```

```html
<input name="code" ngModel appForbidden>
```

</details>

---

## 🎯 Form Control State Classes

Angular **applies CSS classes based on state**:

* `ng-pristine` / `ng-dirty`
* `ng-untouched` / `ng-touched`
* `ng-valid` / `ng-invalid`
* `ng-pending`

<details>
<summary>Example</summary>

```html
<input name="email" [(ngModel)]="email" required>

<!-- style based on validity -->
input.ng-invalid.ng-touched {
  border-color: red;
}
```

</details>

---

## 🧷 Grouping Fields (`ngModelGroup`)

`ngModelGroup` **groups related controls inside the form model**.

<details>
<summary>Example</summary>

```html
<form #f="ngForm">
  <div ngModelGroup="address">
    <input name="street" ngModel>
    <input name="city" ngModel>
  </div>
</form>

<!-- f.value = { address: { street: '', city: '' } } -->
```

</details>

---

## 🚀 Submitting Forms

`(ngSubmit)` **fires when the form is validly submitted**.

<details>
<summary>Example</summary>

```html
<form #f="ngForm" (ngSubmit)="submit(f)">
  <input name="email" [(ngModel)]="email" required>
  <button type="submit">Submit</button>
</form>
```

```ts
submit(form: NgForm) {
  console.log(form.valid, form.value);
}
```

</details>

---

## 🛑 Disabling Controls

Use the native `disabled` attribute or bind it.

<details>
<summary>Example</summary>

```html
<input name="email" [(ngModel)]="email" [disabled]="isLocked">
```

</details>

---

## 🔄 Default Values

Default values **come from the bound property**.

<details>
<summary>Example</summary>

```ts
email = 'test@example.com';
```

```html
<input name="email" [(ngModel)]="email">
```

</details>

---

## 📌 Best Practices

* Always set `name` on controls (unless `standalone:true`)
* Use template‑driven forms for **simple UI‑focused forms**
* Prefer reactive forms for **complex validation & state logic**
* Avoid mixing reactive + template‑driven in the same control

---

If you want, I can also add **file inputs, radio groups, checkboxes, select controls, async validators, or CVA for custom controls**.

## Additional Topics & Gotchas

### Radio buttons & checkbox groups

* **Radio buttons** share a `name` to behave as a group.

```html
<label *ngFor="let plan of plans">
  <input type="radio" name="plan" [(ngModel)]="selected" [value]="plan" /> {{ plan }}
</label>
```

* **Checkboxes** bind booleans.

```html
<input type="checkbox" [(ngModel)]="agreed" /> I agree
```

### Select lists (single & multiple)

```html
<select [(ngModel)]="country">
  <option *ngFor="let c of countries" [ngValue]="c">{{ c.name }}</option>
</select>

<select multiple [(ngModel)]="selectedTags">
  <option *ngFor="let t of tags" [ngValue]="t">{{ t }}</option>
</select>
```

Use **`[ngValue]`** for non‑string objects.

### Native vs Angular validation

* Browser validation attributes like `required`, `minlength`, `pattern` integrate with Angular.
* Disable native validation UI to avoid duplicate messages:

```html
<form novalidate>…</form>
```

### Form reset & default values

```ts
@ViewChild('formRef') form!: NgForm;

resetForm() {
  this.form.resetForm({ name: 'Guest' });
}
```

### Disable submit until valid

```html
<button [disabled]="form.invalid">Submit</button>
```

### Using `ngModel` with standalone controls

* Add **`ngModelOptions`** to avoid registration in a parent form when needed:

```html
<input [(ngModel)]="search" ngModelOptions="{standalone: true}" />
```

### Change detection timing

* Template‑driven forms update **after change detection**.
* Avoid reading `form.value` in the same cycle you mutate it.

### Number inputs & type conversion

```html
<input type="number" [(ngModel)]="age" />
```

Values bind as **numbers** when the type is `number`; otherwise strings.

### Custom cross‑field validation (directive)

Add a directive to the **form** to validate across controls.

```ts
@Directive({selector: '[passwordMatch]', providers:[{provide: NG_VALIDATORS, useExisting: forwardRef(() => PasswordMatchDirective), multi:true}]})
export class PasswordMatchDirective implements Validator {
  validate(form: NgForm) {
    const p = form.controls['password']?.value;
    const c = form.controls['confirm']?.value;
    return p && c && p !== c ? { passwordMismatch:true } : null;
  }
}
```

### Async validation (template‑driven)

* Rare, but possible with a **directive providing `NG_ASYNC_VALIDATORS`**.
* Return a **Promise/Observable** resolving to `null | ValidationErrors`.

### Accessibility checklist

* Associate labels via `for` / `id` or wrapping.
* Provide descriptive error messages tied with `aria-describedby`.
* Keep logical tab order.

### Common pitfalls

* ⚠️ Don’t use `[(ngModel)]` on the same element as `formControl`.
* ⚠️ Avoid mixing template‑driven & reactive in the same control tree.
* ⚠️ Remember `name` is required for controls inside a form.

---
