---
sidebar_position: 2
---

# 18.2 HTML Attributes

## Definitions

- **Attribute**: Modifiers of HTML elements that provide additional information or alter their behavior. They are always specified in the start tag and usually come in name/value pairs like: `name="value"`.

## Reference List

This is a list of the most common and important element-specific attributes. For attributes that apply to *all* HTML elements, see the **Global Attributes** section.

### Form Attributes
- `action`: Specifies where to send the form-data when a form is submitted. (Used on `<form>`)
- `method`: Specifies the HTTP method to use for sending form-data. (Used on `<form>`)
- `enctype`: Specifies how the form-data should be encoded when submitting it to the server. (Used on `<form>`)
- `autocomplete`: Specifies whether a form or an input field should have autocomplete on or off. (Used on `<form>`, `<input>`)
- `novalidate`: Specifies that the form should not be validated when submitted. (Used on `<form>`)

### Input Attributes
- `type`: Specifies the type of input element to display. (Used on `<input>`, `<button>`)
- `value`: Specifies the value of an element. (Used on `<input>`, `<option>`, `<button>`, etc.)
- `name`: Specifies the name of an element. Critical for form submission. (Used on `<input>`, `<select>`, `<textarea>`, `<button>`, etc.)
- `placeholder`: Specifies a short hint that describes the expected value of an input field. (Used on `<input>`, `<textarea>`)
- `required`: Specifies that an input field must be filled out before submitting the form. (Used on `<input>`, `<select>`, `<textarea>`)
- `disabled`: Specifies that an input element should be disabled. (Used on `<input>`, `<button>`, `<optgroup>`, `<option>`, `<select>`, `<textarea>`)
- `readonly`: Specifies that an input field is read-only. (Used on `<input>`, `<textarea>`)
- `checked`: Specifies that an `<input>` element should be pre-selected when the page loads (for type="checkbox" or type="radio").
- `multiple`: Specifies that a user can enter more than one value. (Used on `<input>`, `<select>`)
- `pattern`: Specifies a regular expression that an `<input>` element's value is checked against.
- `min` / `max`: Specifies a minimum/maximum value for an `<input>` element.
- `step`: Specifies the legal number intervals for an input field.
- `maxlength` / `minlength`: Specifies the maximum/minimum number of characters allowed in an element.

### Media Attributes
- `src`: Specifies the URL of the media file. (Used on `<img>`, `<audio>`, `<video>`, `<iframe>`, `<script>`, `<embed>`, `<source>`, `<track>`)
- `alt`: Specifies an alternate text for an image, if the image cannot be displayed. (Used on `<img>`, `<area>`, `<input>`)
- `width` / `height`: Specifies the width/height of the element. (Used on `<img>`, `<iframe>`, `<object>`, `<video>`, `<canvas>`)
- `controls`: Specifies that audio/video controls should be displayed. (Used on `<audio>`, `<video>`)
- `autoplay`: Specifies that the audio/video will start playing as soon as it is ready. (Used on `<audio>`, `<video>`)
- `loop`: Specifies that the audio/video will start over again, every time it is finished. (Used on `<audio>`, `<video>`)
- `muted`: Specifies that the audio output of the video should be muted. (Used on `<audio>`, `<video>`)
- `poster`: Specifies an image to be shown while the video is downloading, or until the user hits the play button. (Used on `<video>`)

### Link and Navigation Attributes
- `href`: Specifies the URL of the page the link goes to. (Used on `<a>`, `<link>`, `<area>`, `<base>`)
- `target`: Specifies where to open the linked document. (Used on `<a>`, `<area>`, `<base>`, `<form>`)
- `rel`: Specifies the relationship between the current document and the linked document. (Used on `<a>`, `<area>`, `<form>`, `<link>`)
- `download`: Specifies that the target will be downloaded when a user clicks on the hyperlink. (Used on `<a>`, `<area>`)

### Table Attributes
- `colspan`: Specifies the number of columns a cell should span. (Used on `<td>`, `<th>`)
- `rowspan`: Specifies the number of rows a cell should span. (Used on `<td>`, `<th>`)
- `headers`: Specifies one or more forms the header cell is related to. (Used on `<td>`, `<th>`)

## Examples

<details>
<summary><strong>Example: Syntax Rules for Attributes</strong></summary>

While HTML5 is forgiving, best practices dictate the following rules:

1. Always use lowercase attribute names.
2. Always quote attribute values.
3. Use double quotes (single quotes are acceptable if the value itself contains double quotes).

```html
<!-- BAD -->
<input TYPE=text value=Hello>

<!-- GOOD -->
<input type="text" value="Hello">
```

</details>
