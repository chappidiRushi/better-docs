---

id: forms-user-input

title: Forms & User Input

sidebar_position: 7
-----------------

# Forms & User Input

> User data collection, validation, and submission primitives in HTML

---

## Forms Basics

```txt
<form> groups controls and defines submission behavior
```

```html
<form action="/submit" method="post"></form>
<!-- action: URL -->
<!-- method: get | post | dialog -->
```

<details>
<summary>Examples</summary>

```html
<form action="/login" method="post" novalidate>
  <input name="user">
</form>
<!-- novalidate: disables UA validation -->
```

</details>

---

## Input Types

```txt
<input> defines interactive controls based on type and attributes
```

```html
<input type="text">
```

```txt
Core attributes (apply to most input types):
- type: text | search | url | email | password | number | range | color | date | time | datetime-local | month | week | checkbox | radio | file | hidden | submit | reset | button | image
- name: string (form field name)
- value: string
- id: string
- class: string
- disabled: boolean
- readonly: boolean
- required: boolean
- autofocus: boolean
- autocomplete: on | off | token
- form: form_id

Text / value-related:
- placeholder: string
- minlength: number
- maxlength: number
- pattern: regex
- size: number

Numeric / range:
- min: number | date
- max: number | date
- step: number | any

Checkbox / radio:
- checked: boolean

File:
- accept: mime | .ext
- multiple: boolean

Image button:
- src: URL
- alt: string
- height: number
- width: number

Submission:
- formaction: URL
- formmethod: get | post
- formenctype: application/x-www-form-urlencoded | multipart/form-data | text/plain
- formnovalidate: boolean
- formtarget: _self | _blank | _parent | _top
```

<details>
<summary>Examples</summary>

```html
<input type="text" name="user" placeholder="name" required maxlength="20">

<input type="number" name="age" min="0" max="120" step="1">

<input type="checkbox" name="agree" checked>

<input type="file" name="avatar" accept="image/*" multiple>

<input type="submit" value="Send" formaction="/submit" formmethod="post">
```

```txt
Notes:
- Unsupported types fallback to text
- Attribute support may vary by type
```

</details>

---

## Labels & Placeholders

```txt
<label> provides accessible naming; placeholder is a hint only
```

```html
<label for="id"></label>
<input id="id" placeholder="hint">
```

<details>
<summary>Examples</summary>

```html
<label>
  Email
  <input type="email" required>
</label>
```

```txt
Rules:
- Every control should have a label
- placeholder ≠ label
```

</details>

---

## Required Fields & Validation

```txt
Constraint validation via attributes
```

<details>
<summary>Examples</summary>

```html
<input required>
<input type="email" required>
<input pattern="[A-Za-z]+"> <!-- regex -->
<input minlength="3" maxlength="10"> <!-- numbers -->
```

```txt
APIs:
- checkValidity()
- reportValidity()
```

</details>

---

## Buttons & Form Submission

```txt
<button> triggers actions based on type
```

```html
<button type="submit"></button>
```

<details>
<summary>Examples</summary>

```html
<button type="submit">Send</button>   <!-- submit | reset | button -->
<input type="submit" value="Send">
```

```txt
Notes:
- Default type is submit
```

</details>

---

## Advanced Form Elements

```txt
Specialized controls for structured input
```

---

## `<select>`

```html
<select></select>
```

<details>
<summary>Examples</summary>

```html
<select name="country">
  <option value="us">US</option>
  <option value="in">IN</option>
</select>
<!-- multiple | size -->
```

</details>

---

## `<textarea>`

```html
<textarea></textarea>
```

<details>
<summary>Examples</summary>

```html
<textarea rows="4" cols="40">Text</textarea>
<!-- rows | cols | maxlength -->
```

</details>

---

## `<datalist>`

```txt
Autocomplete options for inputs
```

<details>
<summary>Examples</summary>

```html
<input list="browsers">
<datalist id="browsers">
  <option value="Chrome">
  <option value="Firefox">
</datalist>
```

</details>

---

## `<fieldset>`

```txt
Groups related controls
```

<details>
<summary>Examples</summary>

```html
<fieldset disabled>
  <legend>Profile</legend>
  <input name="name">
</fieldset>
<!-- disabled | form -->
```

</details>

---

## Notes

* Native validation is UA-dependent
* Forms submit name/value pairs
* Accessibility depends on labeling

## Caveats

* Custom JS validation can break UX
* Misusing placeholder harms accessibility
* Some input types vary by platform
