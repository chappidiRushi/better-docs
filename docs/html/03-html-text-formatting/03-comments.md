---
sidebar_position: 3
---

# 3.3 HTML Comments

## Definitions

- **Comment**: A section of code that is ignored by the compiler or interpreter (in this case, the web browser). It is used for leaving notes or temporarily disabling code.
- **Syntax**: The set of rules that defines the combinations of symbols that are considered to be correctly structured document or fragment in that language.
- **Debugging**: The process of identifying and removing errors from computer hardware or software.

## Beginner Level Introduction

HTML comments are not displayed in the browser, but they can help document your HTML source code. They are incredibly useful for leaving notes for yourself or other developers who might work on your code in the future.

### Syntax

An HTML comment begins with `<!--` and ends with `-->`.

```html
<!-- This is a comment -->
<p>This is a paragraph.</p>
<!-- Remember to add an image here later -->
```

Notice that there is an exclamation point (`!`) in the start tag, but not in the end tag.

## Deep Dive

### Multi-line Comments

Comments can span multiple lines. This is particularly useful for explaining complex sections of code or leaving detailed instructions.

```html
<!--
  This is a multi-line comment.
  Everything between the opening and closing tags
  will be completely ignored by the browser.
-->
```

### Debugging Comments (Commenting Out Code)

One of the most common uses for comments during development is to temporarily hide (or "comment out") sections of HTML. If a part of your layout is breaking, you can comment it out to see if the rest of the page fixes itself. This helps isolate the problem.

```html
<p>This will be displayed.</p>
<!-- <p>This paragraph is currently hidden for testing.</p> -->
```

You can even comment out multiple HTML elements at once.

### Conditional Comments (Legacy)

In the past, Internet Explorer (IE) supported "conditional comments." These were special HTML comments that only IE could read, allowing developers to target specific versions of IE with custom CSS or scripts to fix compatibility bugs.

```html
<!--[if IE 9]>
  <p>You are using Internet Explorer 9.</p>
<![endif]-->
```

*Note: Conditional comments are not supported in modern browsers (including modern versions of Edge) and are now considered obsolete.*

## Examples

<details>
<summary><strong>Example: Single and Multi-line Comments</strong></summary>

```html
<!DOCTYPE html>
<html>
<body>

  <!-- Header Section Starts Here -->
  <header>
    <h1>Welcome to my Blog</h1>
  </header>
  <!-- Header Section Ends Here -->

  <main>
    <!-- 
      TODO: 
      - Fetch latest blog posts from database
      - Format date strings
      - Add pagination at the bottom
    -->
    <p>Blog posts will go here.</p>
  </main>

</body>
</html>
```

</details>

<details>
<summary><strong>Example: Debugging by Commenting Out</strong></summary>

```html
<div>
  <h2>My Gallery</h2>
  <img src="pic1.jpg" alt="Picture 1">
  
  <!-- 
    The second image is broken and messing up the layout.
    I will comment it out until I fix the image path.
    
    <img src="pic2_broken_path.jpg" alt="Picture 2"> 
  -->
  
  <img src="pic3.jpg" alt="Picture 3">
</div>
```

</details>
