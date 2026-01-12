
---
id: forms
title: Forms
sidebar_position: 17
-------------------
# Forms

> Guide to handling forms in React, including controlled, uncontrolled components, and best practices

---

## Controlled Components

Controlled components have their form data managed by React state.

<details>
<summary>Example: Controlled Input</summary>

```js
import { useState } from 'react';

function ControlledForm() {
  const [name, setName] = useState('');

  const handleChange = e => setName(e.target.value);
  const handleSubmit = e => {
    e.preventDefault();
    alert(`Submitted name: ${name}`);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" value={name} onChange={handleChange} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

</details>

---

## Uncontrolled Components

Uncontrolled components use refs to access form values directly from the DOM.

<details>
<summary>Example: Uncontrolled Input</summary>

```js
import { useRef } from 'react';

function UncontrolledForm() {
  const inputRef = useRef();

  const handleSubmit = e => {
    e.preventDefault();
    alert(`Submitted name: ${inputRef.current.value}`);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" ref={inputRef} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

</details>

---

## Form Handling with Multiple Inputs

<details>
<summary>Example: Multiple Controlled Inputs</summary>

```js
import { useState } from 'react';

function MultiInputForm() {
  const [form, setForm] = useState({ firstName: '', lastName: '' });

  const handleChange = e => {
    const { name, value } = e.target;
    setForm(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = e => {
    e.preventDefault();
    console.log(form);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="firstName" value={form.firstName} onChange={handleChange} placeholder="First Name" />
      <input name="lastName" value={form.lastName} onChange={handleChange} placeholder="Last Name" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

</details>

---

## Controlled vs Uncontrolled

| Type         | Managed By   | Use Case                                         |
| ------------ | ------------ | ------------------------------------------------ |
| Controlled   | React state  | When you need immediate validation or state sync |
| Uncontrolled | DOM via refs | Quick forms or third-party integrations          |

---

## Best Practices

* Prefer controlled components for predictable state and easier validation.
* Use `onChange` handlers for controlled inputs.
* Use `useRef` for uncontrolled components when you need direct DOM access.
* Normalize form state for multiple inputs to simplify updates.
* Validate inputs before submission.
* Consider using libraries like Formik or React Hook Form for complex forms.

---

## Notes & Caveats

* Controlled components may cause re-renders on each keystroke; optimize for performance if needed.
* Uncontrolled components are simpler but less flexible.
* For large forms, consider splitting into smaller components for maintainability.
