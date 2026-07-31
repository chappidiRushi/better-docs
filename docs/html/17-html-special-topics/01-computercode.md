---
sidebar_position: 1
---

# 17.1 Computer Code

## Definitions

- **Monospace Font**: A font whose letters and characters each occupy the same amount of horizontal space (e.g., Courier). Ideal for displaying code.
- **Preformatted Text (`<pre>`)**: Text in an HTML document that preserves both spaces and line breaks.
- **Code Element (`<code>`)**: Used to define a fragment of computer code.

## Beginner Level Introduction

If you are writing a technical blog or a documentation site (like this one!), you need to display computer code to your readers. 

You cannot simply paste code into a standard paragraph (`<p>`) tag for two reasons:
1. Standard fonts (like Arial or Times New Roman) have variable-width characters, which makes code very hard to read.
2. Browsers automatically ignore extra spaces and line breaks in HTML. If you write Python code, which relies on indentation, the browser will flatten it all into a single, unreadable line.

## Deep Dive

### The `<code>` Element

The `<code>` element is used to define a piece of computer code. By default, browsers display it in a monospace font.

It is an **inline element**. This means it is perfect for highlighting a single variable name or function in the middle of a sentence.

```html
<p>To print a message in JavaScript, use the <code>console.log()</code> function.</p>
```

### The `<pre>` Element

The `<pre>` element defines preformatted text. 

It is a **block-level element**. Text inside a `<pre>` element is displayed in a fixed-width font, and it preserves both spaces and line breaks exactly as they are written in the HTML file.

### Combining `<pre>` and `<code>`

For multi-line blocks of code (like a full function or a script), best practices dictate that you should wrap a `<code>` element *inside* a `<pre>` element.

The `<pre>` tag preserves the line breaks and indentation, while the `<code>` tag provides the semantic meaning that the content is computer code.

### Other Code-Related Elements

- `<kbd>`: Defines keyboard input (e.g., instructing a user to press <kbd>Ctrl</kbd> + <kbd>C</kbd>).
- `<samp>`: Defines sample output from a computer program.
- `<var>`: Defines a variable in programming or in a mathematical expression.

## Examples

<details>
<summary><strong>Example: Inline vs Block Code</strong></summary>

```html
<!-- Inline Code -->
<p>To declare a variable in JavaScript, you can use <code>let</code> or <code>const</code>.</p>

<!-- Block Code -->
<!-- Notice how the indentation inside the <pre> tag is preserved -->
<pre>
<code>
function greet(name) {
  if (name) {
    console.log("Hello, " + name);
  } else {
    console.log("Hello, stranger");
  }
}
</code>
</pre>
```

</details>

<details>
<summary><strong>Example: Keyboard Input and Variables</strong></summary>

```html
<p>
  To save the file, press <kbd>Cmd</kbd> + <kbd>S</kbd> on Mac, 
  or <kbd>Ctrl</kbd> + <kbd>S</kbd> on Windows.
</p>

<p>
  The area of a triangle is: 1/2 * <var>b</var> * <var>h</var>, where 
  <var>b</var> is the base, and <var>h</var> is the vertical height.
</p>

<p>
  If the command fails, the console will output: <samp>Error: File not found.</samp>
</p>
```

</details>
