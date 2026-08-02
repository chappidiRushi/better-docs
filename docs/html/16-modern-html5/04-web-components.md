---
sidebar_position: 4
---

# 16.4 Web Components

## Definitions

- **Web Components**: A suite of different technologies allowing you to create reusable custom elements with their functionality encapsulated away from the rest of your code.
- **Custom Elements**: A set of JavaScript APIs that allow you to define custom elements and their behavior.
- **Shadow DOM**: A set of JavaScript APIs for attaching an encapsulated "shadow" DOM tree to an element.
- **`<template>` Element**: An HTML mechanism for holding client-side content that is not to be rendered when a page is loaded but may subsequently be instantiated during runtime using JavaScript.
- **`<slot>` Element**: A placeholder inside a web component that you can fill with your own markup, which lets you create separate DOM trees and present them together.

## Beginner Level Introduction

Historically, if you wanted a complex, reusable user interface component (like a custom date picker, a tabbed interface, or an accordion), you had to rely heavily on large JavaScript frameworks like React, Vue, or Angular. 

Web Components are an official W3C web standard that brings this component-based architecture natively to the browser. 

With Web Components, you can create a custom HTML tag (e.g., `<my-custom-dialog>`), style it, give it interactive behavior using JavaScript, and then reuse that tag anywhere in your HTML document without worrying about its styles clashing with the rest of the page.

## Deep Dive

### The Three Pillars of Web Components

1. **Custom Elements API**
   This is the JavaScript API used to define the new HTML tag. By creating a class that extends `HTMLElement` and registering it with `customElements.define()`, you teach the browser how to behave when it encounters your new tag.
   *(Note: Custom element names must contain a hyphen to avoid conflicts with native HTML elements.)*

2. **Shadow DOM**
   When you write CSS for a normal webpage, it cascades everywhere. If you style `h1 { color: red; }`, all `h1` elements turn red. 
   The **Shadow DOM** solves this by encapsulating your component's HTML and CSS. Styles defined *inside* a Shadow DOM do not leak out, and styles defined on the main page do not leak in. It acts as an impenetrable shield.

3. **HTML Templates (`<template>` and `<slot>`)**
   The `<template>` tag holds HTML markup that isn't rendered immediately. You can clone this template using JavaScript and attach it to your custom element's Shadow DOM. 
   The `<slot>` tag acts as a designated placeholder. It allows the developer using your component to pass their own HTML content into your encapsulated component.

## Examples

<details>
<summary><strong>Example: Creating a Custom Profile Card Component</strong></summary>

This example demonstrates how to build a fully self-contained `<user-profile>` web component.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Web Components Example</title>
</head>
<body>

  <!-- 
    Using our new custom element! 
    Notice how we pass an image and a name into the component using the `slot` attribute. 
  -->
  <user-profile>
    <span slot="user-name">Jane Doe</span>
    <img slot="user-image" src="https://via.placeholder.com/150" alt="Profile Picture">
  </user-profile>
  
  <user-profile>
    <span slot="user-name">John Smith</span>
    <!-- Notice we are not providing an image here, so the component's default fallback will show. -->
  </user-profile>

  <!-- Step 1: Define the Template -->
  <template id="profile-template">
    <!-- These styles are encapsulated! They will NOT affect the rest of the page. -->
    <style>
      .card {
        border: 1px solid #ccc;
        border-radius: 8px;
        padding: 16px;
        width: 250px;
        text-align: center;
        box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        font-family: sans-serif;
        margin-bottom: 20px;
      }
      .avatar-slot-wrapper {
        border-radius: 50%;
        overflow: hidden;
        width: 100px;
        height: 100px;
        margin: 0 auto 16px;
        background-color: #eee;
      }
      ::slotted(img) {
        width: 100%;
        height: 100%;
        object-fit: cover;
      }
      h2 { margin: 0 0 8px; font-size: 1.5rem; color: #333; }
    </style>
    
    <div class="card">
      <div class="avatar-slot-wrapper">
        <!-- The 'name' attribute determines which content goes where -->
        <slot name="user-image">
          <!-- This text only shows if no 'user-image' slot is provided -->
          No Image
        </slot>
      </div>
      <h2>
        <slot name="user-name">Anonymous User</slot>
      </h2>
      <button class="follow-btn">Follow</button>
    </div>
  </template>

  <!-- Step 2: Define the Custom Element Logic -->
  <script>
    // Create a class extending HTMLElement
    class UserProfile extends HTMLElement {
      constructor() {
        super(); // Always call super() first in the constructor

        // Attach a shadow root to this element
        // 'open' means we can access it via JavaScript later if needed
        this.attachShadow({ mode: 'open' });

        // Grab the template content
        const template = document.getElementById('profile-template');
        const templateContent = template.content.cloneNode(true);

        // Add interactive behavior entirely scoped to this component
        const btn = templateContent.querySelector('.follow-btn');
        btn.addEventListener('click', () => {
          btn.innerText = btn.innerText === 'Follow' ? 'Following' : 'Follow';
          btn.style.backgroundColor = btn.innerText === 'Following' ? '#4CAF50' : '';
          btn.style.color = btn.innerText === 'Following' ? 'white' : '';
        });

        // Append the cloned template to the shadow root
        this.shadowRoot.appendChild(templateContent);
      }
    }

    // Step 3: Register the custom element
    // The name must contain a hyphen!
    customElements.define('user-profile', UserProfile);
  </script>

</body>
</html>
```

</details>
