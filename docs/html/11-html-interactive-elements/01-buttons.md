---
sidebar_position: 1
---

# 11.1 Buttons

## Definitions

- **Button (`<button>`)**: An interactive HTML element used to trigger actions on a webpage, such as submitting a form or launching a JavaScript function.
- **Button Type**: An attribute that defines the default behavior of the button (`submit`, `reset`, or `button`).
- **Disabled State**: A boolean state where the button is visible but unclickable and un-focusable by the user.

## Beginner Level Introduction

### The Button Element

The `<button>` tag defines a clickable button.

Inside a `<button>` element, you can put text, but you can also put other HTML tags, like `<i>` (for icons), `<b>`, `<strong>`, `<br>`, `<img>`, etc. This makes it much more flexible than using an `<input type="button">`.

```html
<button>Click Me!</button>
```

By default, buttons have a gray background, a border, and they slightly change appearance when you click them (depending on the operating system and browser).

## Deep Dive

### Button Types

The `type` attribute is crucial. It tells the browser what the button is supposed to do. If you do not specify a type, the default type in most browsers is `submit` if the button is inside a `<form>`, which can lead to accidental page reloads.

1. `type="submit"`: The button submits the form data (Default behavior inside a form).
2. `type="reset"`: The button resets all the form controls to their initial values.
3. `type="button"`: The button has no default behavior. It does nothing unless you write JavaScript to listen for its click event.

**Best Practice**: *Always* specify the `type` attribute for a `<button>` element.

### Styling Buttons

Because the default browser styles for buttons are often ugly, developers almost always override them with CSS.

Common CSS properties applied to buttons:
- `background-color`
- `color` (for text)
- `border` (often set to `none`)
- `padding`
- `border-radius` (for rounded corners)
- `cursor: pointer` (to change the mouse to a hand icon when hovering)

### Disabled Buttons

You can disable a button using the boolean `disabled` attribute. A disabled button is unusable and un-clickable. 

This is frequently used in web development alongside JavaScript. For example, you might disable a "Submit" button until the user has filled out all required fields in a form.

### Accessibility

When using icons instead of text for a button (e.g., a magnifying glass for search), a screen reader user won't know what the button does.

Always provide an accessible name for icon-only buttons using the `aria-label` attribute.

```html
<button aria-label="Search">🔍</button>
```

## Examples

<details>
<summary><strong>Example: The Three Button Types</strong></summary>

```html
<!-- A simple form to demonstrate button types -->
<form action="/submit_endpoint" method="post">
  
  <label for="fname">First name:</label>
  <input type="text" id="fname" name="fname">

  <!-- 
    Special Attribute: type="button"
    - Does NOT submit the form. Safe for pure JavaScript actions.
  -->
  <button type="button" onclick="alert('Hello!')">Just say Hello</button>

  <!-- 
    Special Attribute: type="reset"
    - Clears the "First name" input back to empty.
  -->
  <button type="reset">Reset Form</button>

  <!-- 
    Special Attribute: type="submit"
    - Sends the data to the server and reloads the page.
  -->
  <button type="submit">Submit Data</button>
  
</form>
```

</details>

<details>
<summary><strong>Example: Styling and Disabled State</strong></summary>

```html
<style>
  .btn-custom {
    background-color: #4CAF50; /* Green */
    border: none;
    color: white;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
    border-radius: 8px;
    transition: background-color 0.3s;
  }

  /* Change color on hover */
  .btn-custom:hover {
    background-color: #45a049;
  }

  /* Specific styling for disabled state */
  .btn-custom:disabled {
    background-color: #cccccc;
    color: #666666;
    cursor: not-allowed;
  }
</style>

<!-- A normal, active button -->
<button type="button" class="btn-custom">Click Me</button>

<!-- 
  Special Attribute: disabled
  - This button cannot be clicked and uses the CSS :disabled pseudo-class.
-->
<button type="button" class="btn-custom" disabled>I am Disabled</button>
```

</details>
