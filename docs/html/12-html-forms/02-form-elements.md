---
sidebar_position: 2
---

# 12.2 Form Elements

## Definitions

- **Input (`<input>`)**: The most used form element, which can be displayed in many ways depending on its `type` attribute.
- **Label (`<label>`)**: Defines a label for several form elements, improving usability and accessibility.
- **Select (`<select>`)**: Defines a drop-down list.
- **Option (`<option>`)**: Defines an option that can be selected within a drop-down list.
- **Textarea (`<textarea>`)**: Defines a multi-line text input control.
- **Fieldset (`<fieldset>`)**: Used to group related data in a form.
- **Legend (`<legend>`)**: Defines a caption for a `<fieldset>` element.

## Beginner Level Introduction

A form is made up of different elements that allow the user to input data. 

The most common element is `<input>`, but there are many others for specific situations, like selecting an item from a list or writing a long message.

### The `<input>` Element

The `<input>` element is the workhorse of HTML forms. It is an empty element (no closing tag). By simply changing its `type` attribute, it can become a text field, a checkbox, a radio button, or a color picker.

```html
<input type="text">
```

### The `<label>` Element

The `<label>` element is crucial. It provides a text description for an input element. 

Why use `<label>` instead of a `<p>` or `<span>`?
1. **Accessibility**: Screen readers will read out the label when the user focuses on the input element.
2. **Usability**: When a user clicks on the text within the `<label>`, it automatically focuses the corresponding input (or checks the checkbox/radio button). This makes forms much easier to use on mobile devices.

To connect a label to an input, the `for` attribute of the `<label>` must exactly match the `id` attribute of the `<input>`.

```html
<label for="username">Username:</label>
<input type="text" id="username" name="username">
```

## Deep Dive

### The `<select>` and `<option>` Elements

When you want the user to choose one option from a predefined list, use a drop-down menu. 
The `<select>` element creates the drop-down, and nested `<option>` elements define the choices.

```html
<select name="cars" id="cars">
  <option value="volvo">Volvo</option>
  <option value="saab">Saab</option>
</select>
```
When the form is submitted, the server receives the `name` of the select and the `value` of the chosen option (e.g., `cars=volvo`).

### The `<textarea>` Element

For long-form text (like a comment or a message), the `<input type="text">` is too small. Use `<textarea>` instead. 

Unlike `<input>`, `<textarea>` has a closing tag. The default text goes *between* the tags, not in a `value` attribute. You can define the size using `rows` and `cols` attributes.

```html
<textarea name="message" rows="10" cols="30">
The cat was playing in the garden.
</textarea>
```

### Grouping with `<fieldset>` and `<legend>`

If your form is very long, you should group related fields together (e.g., "Personal Info", "Shipping Address", "Billing Address").

The `<fieldset>` element draws a box around the related elements. The `<legend>` element defines the title for that box.

## Examples

<details>
<summary><strong>Example: Select Dropdown</strong></summary>

```html
<label for="cars">Choose a car:</label>

<!-- 
  Special Attributes on <select>:
  - name: What the server receives as the key.
  - id: Matches the 'for' attribute in the label.
  - multiple: A boolean attribute allowing the user to select more than one option (using Ctrl/Cmd click).
-->
<select name="cars" id="cars">
  
  <!-- 
    Special Attributes on <option>:
    - value: The actual data sent to the server. (The text between the tags is just what the user sees).
    - selected: A boolean attribute that makes this option the default choice.
  -->
  <option value="volvo">Volvo</option>
  <option value="saab">Saab</option>
  <option value="fiat" selected>Fiat</option>
  <option value="audi">Audi</option>
  
</select>
```

</details>

<details>
<summary><strong>Example: Grouping with Fieldset</strong></summary>

```html
<form action="/submit">
  
  <!-- The fieldset draws a semantic box around these inputs -->
  <fieldset>
    
    <!-- The legend acts as the title of the box -->
    <legend>Personalia:</legend>
    
    <label for="fname">First name:</label><br>
    <input type="text" id="fname" name="fname" value="John"><br>
    
    <label for="lname">Last name:</label><br>
    <input type="text" id="lname" name="lname" value="Doe"><br><br>
    
    <input type="submit" value="Submit">
    
  </fieldset>
  
</form>
```

</details>
