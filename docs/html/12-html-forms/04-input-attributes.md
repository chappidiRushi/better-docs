---
sidebar_position: 4
---

# 12.4 Input Attributes

## Definitions

- **Input Attribute**: A modifier added to an `<input>` tag that alters its behavior, restricts data entry, or provides guidance to the user.
- **Client-Side Validation**: Checking the data entered by the user in the browser *before* it is sent to the server.
- **Regular Expression (Regex)**: A sequence of characters that specifies a search pattern, often used for complex form validation.

## Beginner Level Introduction

We already know that the `type`, `name`, and `id` attributes are essential for inputs to function. But HTML provides many more attributes to make forms user-friendly and secure.

### The `placeholder` Attribute

The `placeholder` attribute specifies a short hint that describes the expected value of an input field (e.g., a sample value or a short description of the expected format). 

The short hint is displayed in the input field before the user enters a value, and disappears as soon as they start typing.

```html
<input type="text" name="fname" placeholder="e.g. John">
```

### The `value` Attribute

The `value` attribute specifies the initial, default value for an input field. Unlike a placeholder, this text is actual data that will be submitted if the user doesn't delete it.

```html
<input type="text" name="fname" value="John">
```

## Deep Dive

### Readonly and Disabled

- `readonly`: Specifies that an input field is read-only. A user cannot change the text, but they can highlight it and copy it. **The value is sent when submitting the form.**
- `disabled`: Specifies that an input field should be disabled. A user cannot interact with it at all. **The value is NOT sent when submitting the form.**

### HTML5 Validation Attributes

HTML5 introduced attributes that perform basic validation directly in the browser, without needing JavaScript.

- `required`: A boolean attribute. Specifies that an input field must be filled out before submitting the form. If empty, the browser will block submission and show an error popup.
- `minlength` and `maxlength`: Specifies the minimum and maximum number of characters allowed in a text field.
- `min` and `max`: Specifies the minimum and maximum numerical values allowed in a `number`, `range`, or `date` field.

### The `pattern` Attribute

For complex validation, you can use the `pattern` attribute. It specifies a regular expression that the `<input>` element's value is checked against when the form is submitted.

```html
<!-- Only allows exactly 3 uppercase or lowercase letters -->
<input type="text" name="country_code" pattern="[A-Za-z]{3}" title="Three letter country code">
```
*(Note: Always provide a `title` attribute when using `pattern`. The browser will use the `title` text in the error message if the user enters invalid data).*

### The `autocomplete` Attribute

The `autocomplete` attribute specifies whether a form or an input field should have autocomplete turned on or off. 
When turned on, the browser automatically complete values based on values that the user has entered before (like saving addresses or credit cards).

You can use specific values to help the browser understand exactly what the field is for, which is highly recommended for accessibility.
- `autocomplete="email"`
- `autocomplete="new-password"`
- `autocomplete="street-address"`

## Examples

<details>
<summary><strong>Example: Comprehensive Client-Side Validation</strong></summary>

```html
<form action="/register" method="POST">
  
  <label for="username">Username (4-8 chars):</label>
  <!-- 
    Special Attributes:
    - required: Cannot be empty.
    - minlength / maxlength: Enforces length constraints.
  -->
  <input type="text" id="username" name="username" required minlength="4" maxlength="8">
  <br><br>

  <label for="pin">PIN Code (Exactly 4 digits):</label>
  <!-- 
    Special Attribute: pattern
    - \d means "any digit".
    - {4} means "exactly four times".
    - title is used for the error message.
  -->
  <input type="text" id="pin" name="pin" required pattern="\d{4}" title="Please enter exactly 4 numbers.">
  <br><br>

  <label for="email">Email Address:</label>
  <!-- 
    Special Attribute: autocomplete
    - Tells the browser this is an email, prompting auto-fill options.
  -->
  <input type="email" id="email" name="email" required autocomplete="email">
  <br><br>

  <button type="submit">Register</button>
</form>
```

</details>

<details>
<summary><strong>Example: Readonly vs Disabled</strong></summary>

```html
<form action="/checkout">
  
  <label for="itemId">Item ID (Sent to server):</label>
  <!-- 
    Special Attribute: readonly
    - User cannot edit "10495", but it WILL be submitted.
  -->
  <input type="text" id="itemId" name="itemId" value="10495" readonly>
  <br><br>

  <label for="discount">Discount Code (Not eligible):</label>
  <!-- 
    Special Attribute: disabled
    - User cannot interact with it, and it will NOT be submitted.
  -->
  <input type="text" id="discount" name="discount" value="SUMMER20" disabled>
  <br><br>

</form>
```

</details>
