# Reactive Forms

> Mid‑level friendly — **focuses on what each feature does and how to structure forms well**.
>
> Uses **standalone components** + `ReactiveFormsModule`.

---

## ⭐ Overview

Reactive Forms are **model‑driven** — you build the form structure in TypeScript, then bind it to the template.

They are best for:

* complex validation logic
* conditional / dynamic fields
* reusable custom controls
* predictable & testable form state

---

## 🧩 Setup (Standalone Component)

Import **ReactiveFormsModule** where the form lives:

```ts
import { Component } from '@angular/core';
import { ReactiveFormsModule, FormControl } from '@angular/forms';

@Component({
  selector: 'app-login',
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `...`
})
export class LoginComponent {
  email = new FormControl('');
}
```

---

## 🏗️ Building Forms

### **FormControl** — Single Value

```ts
email = new FormControl<string>('');
```

Bind in template:

```html
<input type="email" [formControl]="email" />
```

---

### **FormGroup** — Object Shape

```ts
form = new FormGroup({
  email: new FormControl<string>(''),
  password: new FormControl<string>('')
});
```

Template:

```html
<form [formGroup]="form" (ngSubmit)="submit()">
  <input formControlName="email" />
  <input formControlName="password" type="password" />
  <button type="submit">Login</button>
</form>
```

---

### **FormBuilder** — Less Boilerplate

```ts
import { FormBuilder, Validators } from '@angular/forms';

constructor(private fb: FormBuilder) {}

form = this.fb.nonNullable.group({
  email: ['', [Validators.required, Validators.email]],
  password: ['', [Validators.required, Validators.minLength(6)]],
  remember: false
});
```

✔️ `nonNullable` gives **strongly typed, non‑null controls**.

---

## 🧬 FormArray — Lists of Controls

```ts
phones = new FormArray([
  new FormControl('')
]);

addPhone() {
  this.phones.push(new FormControl(''));
}
```

Template:

```html
<div [formArrayName]="'phones'">
  <div *ngFor="let c of phones.controls; let i = index">
    <input [formControlName]="i" />
  </div>
</div>
```

---

## ✔️ Validation

### Built‑in Validators

```ts
Validators.required
Validators.email
Validators.min(18)
Validators.max(65)
Validators.minLength(6)
Validators.maxLength(20)
Validators.pattern(/^[A-Z].+/)
```

Apply them when creating controls:

```ts
email = new FormControl('', { validators: [Validators.required, Validators.email] });
```

---

### Custom Validator (Sync)

```ts
import { AbstractControl, ValidationErrors } from '@angular/forms';

export function forbidden(value: string) {
  return (c: AbstractControl): ValidationErrors | null =>
    c.value === value ? { forbidden: true } : null;
};
```

Use it:

```ts
code = new FormControl('', { validators: [forbidden('bad')] });
```

---

### Async Validator (e.g., Server Check)

```ts
import { Observable, of, delay, map } from 'rxjs';

function emailTaken(c: AbstractControl): Observable<ValidationErrors | null> {
  return of(c.value === 'test@example.com').pipe(
    delay(500),
    map(taken => taken ? { emailTaken: true } : null)
  );
}

email = new FormControl('', { asyncValidators: [emailTaken] });
```

---

## 🔍 Reading State & Values

```ts
this.form.value;          // object (possibly partial)
this.form.get('email');   // FormControl
this.form.valid;          // boolean
this.form.touched;        // boolean
```

### Listen to changes

```ts
this.form.valueChanges.subscribe(v => console.log(v));
this.form.statusChanges.subscribe(s => console.log(s));
```

---

## 🔄 Updating Values

```ts
this.form.setValue({ email: 'a@b.com', password: '123', remember: true });

this.form.patchValue({ remember: false }); // partial
```

---

## 🛑 Enable / Disable Controls

```ts
this.form.get('email')?.disable();
this.form.get('email')?.enable();
```

Disabled controls are **excluded from `value`**.

---

## 🧠 Update Triggers

Use `updateOn` to delay validation/updates:

```ts
email = new FormControl('', {
  validators: [Validators.required],
  updateOn: 'blur' // 'change' | 'blur' | 'submit'
});
```

Apply at group level to affect children.

---

## 🧷 Cross‑Field Validation (Group Validator)

```ts
function passwordMatch(group: AbstractControl) {
  const p = group.get('password')?.value;
  const c = group.get('confirm')?.value;
  return p && c && p !== c ? { passwordMismatch: true } : null;
}

form = this.fb.group({
  password: [''],
  confirm: ['']
}, { validators: [passwordMatch] });
```

---

## 🚀 Submitting the Form

```ts
submit() {
  if (this.form.invalid) return;
  console.log(this.form.getRawValue()); // includes disabled
}
```

Template:

```html
<form [formGroup]="form" (ngSubmit)="submit()" novalidate>
  <button type="submit" [disabled]="form.invalid">Save</button>
</form>
```

---

## ♻️ Resetting & Defaults

```ts
this.form.reset();
this.form.reset({ remember: true });
```

---

## 🎛️ Dynamic Controls

```ts
addField() {
  this.form.addControl('nickname', new FormControl(''));
}

removeField() {
  this.form.removeControl('nickname');
}
```

---

## 🎯 Showing Errors in Template

```html
<input formControlName="email">
<div *ngIf="form.get('email')?.touched && form.get('email')?.errors">
  <span *ngIf="form.get('email')?.hasError('required')">Email is required</span>
  <span *ngIf="form.get('email')?.hasError('email')">Invalid email</span>
</div>
```

---

## 🔐 Common Pitfalls

* ⚠️ `setValue` requires **all fields** — use `patchValue` otherwise
* ⚠️ Disabled fields are **not in `.value`** — use `getRawValue()`
* ⚠️ Don’t read `.value` before initialization
* ⚠️ Avoid business logic buried in templates

---

## 🧪 Quick Reference

| Feature            | API                                   |
| ------------------ | ------------------------------------- |
| Create control     | `new FormControl()`                   |
| Create group       | `new FormGroup({...})`                |
| Create via builder | `fb.group({...})`                     |
| List               | `new FormArray([...])`                |
| Sync validator     | `Validators.*`                        |
| Async validator    | Function returning Promise/Observable |
| Listen to changes  | `.valueChanges` / `.statusChanges`    |
| Patch value        | `.patchValue()`                       |
| Full set           | `.setValue()`                         |
| Reset              | `.reset()`                            |
| Disable            | `.disable()`                          |

---
