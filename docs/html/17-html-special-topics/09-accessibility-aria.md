---
sidebar_position: 9
---

# 17.9 Advanced Accessibility (WAI-ARIA)

## Definitions

- **WAI-ARIA (Web Accessibility Initiative - Accessible Rich Internet Applications)**: A specification that provides an ontology of roles, states, and properties that define accessible user interface elements.
- **Screen Reader**: Assistive technology that reads the text displayed on the screen aloud, or translates it into Braille, primarily used by visually impaired users.
- **Role**: Defines the type of user interface element (e.g., `role="alert"`, `role="tab"`).
- **Focus Management**: The process of programmatically moving the browser's focus to specific interactive elements to assist keyboard-only navigation.

## Beginner Level Introduction

We already learned that using Semantic HTML (like `<nav>`, `<main>`, and `<button>`) is the easiest and best way to make a website accessible. Browsers automatically know how to present these elements to screen readers.

However, sometimes you build custom UI components using generic `<div>` or `<span>` elements because native HTML doesn't have a tag for what you need (e.g., a complex custom dropdown, an autocomplete widget, or a drag-and-drop interface). 

Because a `<div>` has no semantic meaning, a screen reader will completely ignore it, leaving visually impaired users unable to use your widget. 
**ARIA** bridges this gap. It allows you to add special attributes to your HTML to manually describe what a `<div>` is doing.

> **The First Rule of ARIA**: No ARIA is better than bad ARIA. Always prefer native HTML elements (like `<button>`) over a custom `<div>` with `role="button"`.

## Deep Dive

### ARIA Roles

The `role` attribute tells assistive technologies what an element *is*. 

If you must use a `<div>` as a clickable button, you must add `role="button"`.
```html
<!-- BAD: Screen readers don't know this is clickable -->
<div class="my-btn" onclick="submit()">Save</div>

<!-- BETTER: Now screen readers announce it as a button -->
<div class="my-btn" role="button" tabindex="0" onclick="submit()">Save</div>
```
Common Roles:
- `role="alert"`: A message with important, and usually time-sensitive, information. Screen readers will interrupt the user to read this immediately.
- `role="dialog"`: Defines a dialog box or modal window.
- `role="navigation"`: Equivalent to the `<nav>` tag.

### ARIA States and Properties

ARIA attributes that start with `aria-` describe the *current state* of an element or provide additional context.

1. **`aria-label`**: Provides a string that labels the current element. Use this when the element has no visible text (like an icon-only button).
2. **`aria-labelledby`**: Points to the `id` of another element that serves as the label.
3. **`aria-describedby`**: Points to the `id` of another element that provides a longer description or hint (often used for form inputs).
4. **`aria-hidden="true"`**: Completely hides an element (and its children) from screen readers, even if it is visibly displayed on screen. Useful for decorative icons.
5. **`aria-expanded`**: Indicates whether a collapsible element (like a dropdown menu or accordion) is currently open (`true`) or closed (`false`). This must be updated via JavaScript.

### Focus Management (`tabindex`)

Users who cannot use a mouse navigate the web using the `Tab` key to jump between interactive elements (links, buttons, inputs).

- **`tabindex="0"`**: Adds an element to the normal tab flow. If you make a custom `<div>` button, you MUST add `tabindex="0"` so keyboard users can reach it.
- **`tabindex="-1"`**: Removes an element from the normal tab flow, BUT allows it to be focused programmatically using JavaScript (`element.focus()`). This is critical for modal windows (when a modal opens, you should move focus to the modal container).
- **`tabindex=">0"` (e.g., `tabindex="1"`)**: **Avoid doing this.** It forces a specific tab order that almost always creates a confusing, illogical experience for users.

## Examples

<details>
<summary><strong>Example: An Accessible Icon Button</strong></summary>

```html
<!-- 
  A screen reader seeing a generic magnifying glass icon SVG will read nothing, 
  leaving the user confused about what the button does. 
  
  Special Attribute: aria-label
  - We provide the text "Search" exclusively for screen readers.
-->
<button aria-label="Search" class="icon-btn">
  <svg width="24" height="24" viewBox="0 0 24 24">
    <!-- SVG path data for a magnifying glass -->
  </svg>
</button>
```

</details>

<details>
<summary><strong>Example: Linking Form Hints with ARIA</strong></summary>

```html
<label for="new-password">Choose a Password:</label>
<!-- 
  Special Attribute: aria-describedby
  - It links the input directly to the paragraph below it.
  - When the user focuses the input, the screen reader will say:
    "Choose a Password, password edit. Password must be at least 8 characters."
-->
<input type="password" id="new-password" aria-describedby="password-hint">

<p id="password-hint" class="text-small text-muted">
  Password must be at least 8 characters long and contain a number.
</p>
```

</details>

<details>
<summary><strong>Example: Live Regions (Dynamic Updates)</strong></summary>

When content on a page changes dynamically via JavaScript (e.g., a "Saved successfully" toast notification appears), screen readers won't notice it unless you tell them to.

```html
<!-- 
  Special Attribute: aria-live
  - "polite": The screen reader will wait until the user stops typing/interacting, then read the new content.
  - "assertive": The screen reader will interrupt the user immediately to read the new content (use sparingly for critical errors).
-->
<div id="toast-container" aria-live="polite">
  <!-- JavaScript injects messages here: <span>Settings saved.</span> -->
</div>
```

</details>
