---

id: forms-architecture
title: Forms Architecture
sidebar_position: 15
--------------------

# Forms Architecture

> Forms define **user input collection, validation, submission, and integration with browser automation (autofill, password managers)**.

---

## Unified Example (Full Forms Model)

```html
<form
  action="/submit" <!-- action: submission URL (relative | absolute | empty = self) -->
  method="post" <!-- method: get | post | dialog -->
  enctype="multipart/form-data" <!-- enctype: application/x-www-form-urlencoded | multipart/form-data | text/plain -->
  target="_self" <!-- target: browsing context -->
  autocomplete="on" <!-- autocomplete: on | off -->
  novalidate <!-- novalidate: disables native validation UI -->
>

  <!-- Label association -->
  <label for="email">Email</label>
  <input
    id="email"
    name="email" <!-- name: REQUIRED for successful controls -->
    type="email" <!-- type: see full list below -->
    required <!-- required: boolean -->
    autocomplete="email" <!-- autofill hint -->
  >

  <!-- Input types -->
  <input type="text"> <!-- text | search | tel | url | email -->
  <input type="password"> <!-- password -->
  <input type="number" min="0" max="10" step="1"> <!-- number | range -->
  <input type="date"> <!-- date | time | datetime-local | month | week -->
  <input type="checkbox" checked> <!-- checkbox -->
  <input type="radio" name="group"> <!-- radio (grouped by name) -->
  <input type="file" multiple accept="image/*"> <!-- file: accept MIME types/extensions -->
  <input type="color"> <!-- color picker -->
  <input type="hidden" value="token"> <!-- hidden: not rendered -->

  <!-- Textarea -->
  <textarea
    name="message"
    rows="4" cols="40"
    minlength="10" maxlength="200"
    wrap="soft" <!-- wrap: soft | hard -->
  ></textarea>

  <!-- Select menu -->
  <select name="choice" required>
    <optgroup label="Group A"> <!-- optgroup: label required -->
      <option value="1">One</option>
    </optgroup>
    <option value="2" selected>Two</option>
  </select>

  <!-- Datalist -->
  <input list="browsers">
  <datalist id="browsers">
    <option value="Chrome">
    <option value="Firefox">
  </datalist>

  <!-- Output -->
  <output name="result">0</output>

  <!-- Field grouping -->
  <fieldset disabled> <!-- disabled: disables all descendants -->
    <legend>Advanced</legend>
    <input type="text">
  </fieldset>

  <!-- Buttons -->
  <button type="submit">Submit</button> <!-- submit | reset | button -->
  <button type="reset">Reset</button>

  <!-- Image submit button -->
  <input type="image" src="submit.png" alt="Submit"> <!-- submits with click coordinates -->

  <!-- Progress (form-associated, read-only) -->
  <progress value="32" max="100"></progress> <!-- task completion indicator -->

  <!-- Meter (form-associated, scalar measurement) -->
  <meter min="0" max="100" low="30" high="80" optimum="90" value="65"></meter>

</form>
```

---

## Input Types (Complete)

* text, search, tel, url, email
* password
* number, range
* date, time, datetime-local, month, week
* checkbox, radio
* file
* color
* hidden
* submit, reset, button, image

> Notes:
>
> * `image` submits click coordinates
> * `range` has no required UI precision
> * `hidden` participates in submission but not validation

---

## Constraint Validation API

* `required`, `min`, `max`, `step`, `pattern`, `minlength`, `maxlength`
* `checkValidity()`, `reportValidity()`
* `setCustomValidity(message)`
* Validity states exposed via `validity` object

---

## Native Validation UX

* Triggered on submit or `reportValidity()`
* UA-specific UI, cannot be fully styled
* Suppressed via `novalidate`

---

## Autofill & Password Managers

* Driven by `name`, `autocomplete`, and field type
* Incorrect `autocomplete` breaks password managers
* Grouped radio/checkbox names affect autofill behavior

---

## Missing-but-Valid Form-Associated Elements

* `<progress>` — read-only progress indicator, participates in form ownership
* `<meter>` — scalar measurement with known range

> Obsolete / Removed:
>
> * `<keygen>` (removed for security reasons)

---

## Additional Hardcore Topics

* Successful vs unsuccessful controls (disabled, no name, type=submit)
* Form-associated custom elements
* `form` attribute for out-of-tree controls
* Implicit submission via Enter key
* `method="dialog"` for `<dialog>` integration

---

## Mental Model (Interview One-Liner)

> **HTML forms are a declarative state machine for input, validation, submission, and browser automation; JavaScript augments but does not replace the native model.**
