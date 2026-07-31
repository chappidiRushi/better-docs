---
sidebar_position: 3
---

# 12.3 Input Types

## Definitions

- **Input Type (`type`)**: An attribute of the `<input>` element that dictates how the input behaves and how it is rendered by the browser.
- **Radio Button**: An input type that allows the user to select exactly one option from a predefined set.
- **Checkbox**: An input type that allows the user to select zero or more options from a predefined set.

## Beginner Level Introduction

The `<input>` element can be displayed in several ways, depending on the `type` attribute. If you don't specify a type, it defaults to `text`.

### Basic Input Types

- `type="text"`: Defines a single-line text input field.
- `type="password"`: Defines a password field. The characters are masked (shown as asterisks or dots) so bystanders cannot read it.
- `type="email"`: Used for input fields that should contain an e-mail address. On mobile devices, this triggers a special keyboard with the "@" symbol prominently displayed.
- `type="number"`: Used for input fields that should contain a numeric value. You can set restrictions on the accepted numbers (min, max, step).

```html
<label for="age">Age:</label>
<input type="number" id="age" name="age">
```

## Deep Dive

### Selection Input Types

**Checkboxes (`type="checkbox"`)**
Checkboxes let a user select ZERO or MORE options from a limited number of choices.
To pre-select a checkbox when the page loads, add the boolean `checked` attribute.

**Radio Buttons (`type="radio"`)**
Radio buttons let a user select ONLY ONE of a limited number of choices.
**Crucial Rule**: For radio buttons to work as an exclusive group (where clicking one unclicks the others), they must all share the exact same `name` attribute.

### Date and Time Input Types

HTML5 introduced several native date and time pickers. The exact UI depends on the user's browser and operating system, but they provide a standard way to collect temporal data without needing heavy JavaScript libraries.

- `type="date"`: Selects a date (year, month, day).
- `type="time"`: Selects a time (no time zone).
- `type="datetime-local"`: Selects a date and a time.
- `type="month"`: Selects a month and year.

### Other Useful Input Types

- `type="file"`: Defines a file-select field and a "Browse" button for file uploads. (Requires `enctype="multipart/form-data"` on the `<form>`).
- `type="range"`: Defines a control for entering a number whose exact value is not important (like a volume slider).
- `type="color"`: Defines a color picker. The value sent to the server is a HEX code (e.g., `#ff0000`).
- `type="search"`: Used for search fields. Behaves like a regular text field, but some browsers add a small "x" to quickly clear the search term.

## Examples

<details>
<summary><strong>Example: Radio Buttons (Exclusive Selection)</strong></summary>

```html
<p>Please select your favorite web language:</p>

<!-- 
  Notice how all three inputs have name="fav_language".
  This groups them together. 
-->
<input type="radio" id="html" name="fav_language" value="HTML">
<label for="html">HTML</label><br>

<input type="radio" id="css" name="fav_language" value="CSS">
<label for="css">CSS</label><br>

<input type="radio" id="javascript" name="fav_language" value="JavaScript">
<label for="javascript">JavaScript</label>
```

</details>

<details>
<summary><strong>Example: File Upload</strong></summary>

```html
<!-- 
  If you are uploading files, the method MUST be POST, 
  and the enctype MUST be "multipart/form-data".
-->
<form action="/upload" method="POST" enctype="multipart/form-data">
  
  <label for="myfile">Select a file:</label>
  
  <!-- 
    Special Attribute: accept
    - Limits the file types the user can select in the dialog box.
  -->
  <input type="file" id="myfile" name="myfile" accept=".jpg, .jpeg, .png">
  
  <br><br>
  <input type="submit">
  
</form>
```

</details>

<details>
<summary><strong>Example: Range Slider</strong></summary>

```html
<label for="vol">Volume (between 0 and 50):</label>

<!-- 
  Special Attributes:
  - min: Minimum value.
  - max: Maximum value.
  - step: Legal number intervals (e.g., step="10" means 0, 10, 20...).
-->
<input type="range" id="vol" name="vol" min="0" max="50" step="5">
```

</details>
