---
sidebar_position: 6
---

# 12.6 Constraint Validation API

## Definitions

- **Constraint Validation**: The process the browser goes through to ensure the data entered into a form matches the rules specified by the input's attributes (like `required`, `minlength`, `pattern`).
- **ValidityState**: A JavaScript object that represents the validity states that an element can be in, with respect to constraint validation.
- **`novalidate`**: A boolean attribute on a `<form>` that disables the browser's default validation UI (like the popup bubbles) so you can handle it entirely with custom JavaScript.

## Beginner Level Introduction

We already learned that you can add attributes like `required` or `type="email"` to an input. If a user types something wrong, the browser automatically stops them from submitting the form and shows a default error bubble (e.g., "Please fill out this field").

However, the browser's default error bubbles often look ugly, and you cannot easily change their design with CSS. Furthermore, the default error messages are generic. You might want to say "Your username must contain a special character" instead of "Please match the requested format."

To build beautiful, custom error messages, you must use the **Constraint Validation API** in JavaScript.

## Deep Dive

### The ValidityState Object

Every form input element in JavaScript has a `validity` property. This property contains a `ValidityState` object with boolean values describing exactly *why* an input is invalid.

Key properties of `validity`:
- `valueMissing`: `true` if the element has a `required` attribute, but no value is entered.
- `typeMismatch`: `true` if the value is not in the required syntax (e.g., invalid email or URL).
- `patternMismatch`: `true` if the value does not match the specified `pattern` attribute (Regular Expression).
- `tooShort` / `tooLong`: `true` if the value violates `minlength` or `maxlength`.
- `rangeUnderflow` / `rangeOverflow`: `true` if a number violates `min` or `max`.
- **`valid`**: `true` if the element meets *all* its validation constraints.

### Customizing the Error Message

You can change the text of the browser's default error bubble using the `setCustomValidity()` method.

```javascript
const input = document.getElementById("username");

if (input.value === "admin") {
  // Sets a custom error, making the input invalid
  input.setCustomValidity("You cannot use 'admin' as a username.");
} else {
  // Passing an empty string CLEARS the error, making the input valid again
  input.setCustomValidity(""); 
}
```

### Disabling Default UI (`novalidate`)

If you want to design your own error messages using HTML and CSS (instead of the browser's popups), you must add the `novalidate` attribute to the `<form>` element. This tells the browser: "I will handle the validation UI myself."

Even with `novalidate` enabled, the browser *still evaluates* the constraints in the background, allowing you to use the `:valid` and `:invalid` CSS pseudo-classes.

## Examples

<details>
<summary><strong>Example: Custom Validation Logic and UI</strong></summary>

In this example, we disable the default browser popups and show our own custom HTML error message when the user tries to submit an invalid form.

```html
<!-- 
  Special Attribute: novalidate
  - Disables the browser's default popup bubbles, but keeps the underlying validation logic active.
-->
<form id="myForm" novalidate>
  <label for="email">Email Address:</label>
  <input type="email" id="email" required>
  
  <!-- Our custom error container, hidden by default -->
  <span id="emailError" class="error-msg" style="color: red; display: none;"></span>

  <button type="submit">Submit</button>
</form>

<script>
  const form = document.getElementById('myForm');
  const emailInput = document.getElementById('email');
  const emailError = document.getElementById('emailError');

  form.addEventListener('submit', function(event) {
    // If the input is NOT valid according to HTML constraints
    if (!emailInput.validity.valid) {
      // 1. Prevent the form from submitting to the server
      event.preventDefault();
      
      // 2. Determine exactly WHAT went wrong using ValidityState
      if (emailInput.validity.valueMissing) {
        emailError.textContent = "You forgot to enter your email!";
      } else if (emailInput.validity.typeMismatch) {
        emailError.textContent = "That doesn't look like a real email address.";
      }
      
      // 3. Show our custom error message
      emailError.style.display = "block";
    }
  });

  // Clear the error as soon as the user starts typing again
  emailInput.addEventListener('input', function() {
    if (emailInput.validity.valid) {
      emailError.textContent = "";
      emailError.style.display = "none";
    }
  });
</script>
```

</details>

<details>
<summary><strong>Example: CSS Validation Styling</strong></summary>

You can style inputs dynamically based on their validation state without writing any JavaScript!

```html
<style>
  /* Applied when the input satisfies all constraints */
  input:valid {
    border: 2px solid green;
  }

  /* Applied when the input violates a constraint */
  /* We also use :not(:placeholder-shown) so the box isn't red before they even start typing */
  input:invalid:not(:placeholder-shown) {
    border: 2px solid red;
    background-color: #ffe6e6;
  }
</style>

<input type="email" required placeholder="Enter email">
```

</details>
