---
sidebar_position: 3
---

# 18.3 Global Attributes

## Definitions

- **Global Attribute**: An attribute that can be used on any standard HTML element. 

## Reference List

While some of these attributes have no effect on certain elements, they are legally permitted to be applied to any HTML tag.

### Core Global Attributes
- `id`: Specifies a unique id for an element. Essential for CSS targeting, JavaScript manipulation, and anchor linking.
- `class`: Specifies one or more class names for an element. Used heavily for CSS styling.
- `style`: Specifies an inline CSS style for an element. (Generally discouraged in production in favor of external stylesheets).
- `title`: Specifies extra information about an element (displayed as a tooltip when hovering over the element).
- `dir`: Specifies the text direction for the content in an element (`ltr`, `rtl`, or `auto`).
- `lang`: Specifies the language of the element's content.

### Interaction & State
- `hidden`: A boolean attribute. When present, it specifies that the element is not yet, or is no longer, relevant (the browser will not render it).
- `tabindex`: Specifies the tabbing order of an element (when the user navigates using the "Tab" key).
- `draggable`: Specifies whether an element is draggable or not.
- `contenteditable`: Specifies whether the content of an element is editable by the user directly in the browser.
- `spellcheck`: Specifies whether the element is to have its spelling and grammar checked or not.

### Data Attributes
- `data-*`: Used to store custom data private to the page or application. This is incredibly useful for passing data from HTML to JavaScript without using invisible input fields.

## Examples

<details>
<summary><strong>Example: Using Custom Data Attributes</strong></summary>

Data attributes are prefixed with `data-`. You can name them whatever you want after the hyphen.

```html
<!-- HTML -->
<ul>
  <li data-animal-type="bird" data-id="123">Owl</li>
  <li data-animal-type="fish" data-id="456">Salmon</li>
  <li data-animal-type="spider" data-id="789">Tarantula</li>
</ul>

<script>
  // JavaScript can easily read these attributes using the dataset property
  const items = document.querySelectorAll('li');
  
  items.forEach(item => {
    // Accesses data-animal-type
    console.log(item.dataset.animalType); 
    // Accesses data-id
    console.log(item.dataset.id); 
  });
</script>
```

</details>

<details>
<summary><strong>Example: Making Text Editable</strong></summary>

```html
<!-- 
  Special Attribute: contenteditable
  - The user can click on this paragraph and type new text directly in the browser! 
-->
<p contenteditable="true">This is an editable paragraph. Try changing this text.</p>
```

</details>
