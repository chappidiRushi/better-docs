# Signal Forms


---

## Overview

Signal Forms use **signals as the single source of truth** for form state, validation, and interaction, replacing traditional reactive forms.

---

## Creating a Form Signal

Define form state using `form()` with strongly typed fields.

<details>
<summary>Example</summary>

```ts
import { Component } from '@angular/core';
import { form } from '@angular/forms';

@Component({
  selector: 'app-form',
  standalone: true,
  template: `{{ userForm.value() | json }}`
})
export class FormComponent {

  userForm = form({
    name: '',              // string
    age: 0,                // number
    active: false           // boolean
    // nested objects supported
  });
}
```

</details>

---

## Binding Form Controls

Bind form fields directly to inputs using `formField()`.

<details>
<summary>Example</summary>

```ts
import { Component } from '@angular/core';
import { form, formField } from '@angular/forms';

@Component({
  selector: 'app-bind',
  standalone: true,
  template: `
    <input [formField]="userForm.controls.name" />
    <input type="number" [formField]="userForm.controls.age" />
  `
})
export class BindComponent {

  userForm = form({
    name: '',
    age: 0
  });
}
```

</details>

---

## Reading Form State

Access form values, validity, and interaction state via signals.

<details>
<summary>Example</summary>

```ts
userForm.value();      // full form value
userForm.valid();      // boolean
userForm.dirty();      // boolean
userForm.touched();    // boolean

userForm.controls.name.value();   // field value
userForm.controls.name.valid();   // field validity
```

</details>

---

## Validation

Attach synchronous validators directly to fields.

<details>
<summary>Example</summary>

```ts
import { Validators } from '@angular/forms';

userForm = form({
  email: ['', [
    Validators.required,
    Validators.email
  ]],
  password: ['', Validators.minLength(8)]
});
```

</details>

---

## Custom Validators

Define reusable validation logic as pure functions.

<details>
<summary>Example</summary>

```ts
const evenValidator = (value: number) =>
  value % 2 === 0 ? null : { notEven: true };

userForm = form({
  count: [0, evenValidator]
});
```

</details>

---

## Updating Form Values

Update form state imperatively or partially.

<details>
<summary>Example</summary>

```ts
userForm.set({ name: 'Alice', age: 30 });
userForm.patch({ age: 31 });

userForm.controls.name.set('Bob');
```

</details>

---

## Resetting Forms

Reset the form to its initial or custom state.

<details>
<summary>Example</summary>

```ts
userForm.reset();
userForm.reset({ name: '', age: 0 });
```

</details>

---

## Nested Forms

Compose forms using nested objects.

<details>
<summary>Example</summary>

```ts
userForm = form({
  profile: {
    firstName: '',
    lastName: ''
  },
  settings: {
    theme: 'dark'
  }
});

userForm.controls.profile.controls.firstName.value();
```

</details>

---

## Form Submission

Handle submission using form validity signals.

<details>
<summary>Example</summary>

```ts
submit() {
  if (this.userForm.valid()) {
    console.log(this.userForm.value());
  }
}
```

</details>

---

## Key Characteristics

* Signal-based (no RxJS)
* Strongly typed
* Zone-less friendly
* Composable and tree-shakable
* Designed for standalone components
