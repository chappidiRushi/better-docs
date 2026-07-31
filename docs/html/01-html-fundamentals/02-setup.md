---
sidebar_position: 2
---

# 1.2 HTML Setup

## Definitions

- **Code Editor**: A text editor specifically designed for writing and editing source code. It offers features like syntax highlighting, auto-completion, and indentation.
- **IDE (Integrated Development Environment)**: A more comprehensive software suite that combines a code editor with built-in tools for debugging, compiling, and version control.
- **Developer Tools (DevTools)**: A set of web developer tools built directly into modern web browsers, used for inspecting HTML/CSS, debugging JavaScript, and analyzing performance.
- **Localhost**: A hostname that refers to the current device used to access it. It is used to access the network services that are running on the host via the loopback network interface.
- **Live Server/Live Preview**: A tool that launches a local development server with a live reload feature. When you save your code, the browser automatically refreshes to reflect the changes.

## Beginner Level Introduction

### Installing Code Editors

To write HTML, you only technically need a basic text editor like Notepad (Windows) or TextEdit (Mac). However, professional web developers use specialized **Code Editors**. These editors make writing code much faster and easier by color-coding tags, automatically formatting lines, and catching simple errors before you even run the code.

### Recommended Editors

1. **Visual Studio Code (VS Code)**: The most popular editor today. It's free, highly customizable, and has a massive library of extensions.
2. **Sublime Text**: Known for being incredibly fast and lightweight.
3. **WebStorm**: A powerful, paid IDE by JetBrains, favored by many enterprise developers.
4. **Notepad++**: A classic, lightweight text editor for Windows.

### Creating an HTML File

1. Open your code editor.
2. Create a new file.
3. Save the file with a `.html` extension (for example, `index.html`).
4. Type your HTML boilerplate code into the file and save it again.

### Running HTML Locally

To view the HTML file you just created:
1. Locate the file on your computer's file system (e.g., in File Explorer or Finder).
2. Double-click the file, or right-click and select "Open with..." and choose a web browser like Google Chrome or Mozilla Firefox.
3. The browser will render the HTML and display the page. The URL in the address bar will start with `file:///` followed by the path on your computer.

## Deep Dive

### Browser Developer Tools

Modern browsers (Chrome, Firefox, Safari, Edge) come equipped with powerful Developer Tools (DevTools).

**How to access DevTools (Chrome):**
- Right-click anywhere on a webpage and select **"Inspect"**.
- Or press `F12` or `Ctrl+Shift+I` (Windows/Linux) or `Cmd+Option+I` (Mac).

**Key DevTools Features for HTML:**
- **Elements Panel**: Allows you to inspect and modify the DOM tree in real-time. You can change HTML structure, add classes, and immediately see the results on the screen.
- **Styles Pane**: Lets you view and edit the CSS applied to the currently selected HTML element.
- **Console**: Used for running JavaScript and viewing error messages, which can sometimes hint at HTML loading issues.

DevTools changes are *temporary*. When you refresh the page, it reverts to the original source code. It is purely for testing and debugging.

### Live Preview Setup

Manually refreshing the browser every time you save a change to your `.html` file is tedious. Developers solve this using a Live Server.

**Setting up Live Server in VS Code:**
1. Open VS Code and go to the Extensions view (`Ctrl+Shift+X` / `Cmd+Shift+X`).
2. Search for **"Live Server"** (by Ritwick Dey).
3. Click "Install".
4. Open your `.html` file, right-click anywhere in the editor, and select **"Open with Live Server"**.
5. Your default browser will open automatically at a local URL (like `http://127.0.0.1:5500/index.html`).
6. Now, every time you press `Ctrl+S` (Save) in VS Code, the browser will instantly reload.

### Online HTML Editors

If you want to test HTML quickly without installing any software, you can use cloud-based editors. These platforms provide immediate, side-by-side previews of your code.

- **CodePen** (codepen.io): Excellent for building UI components and sharing snippets of HTML, CSS, and JS.
- **JSFiddle** (jsfiddle.net): Similar to CodePen, great for testing small bits of code.
- **Replit** (replit.com): A more complete online IDE capable of hosting full projects, including backend code.

## Examples

<details>
<summary><strong>Example: Creating a local workspace</strong></summary>

While not raw HTML code, organizing your files properly is part of HTML setup.

1. Create a folder on your desktop called `my-website`.
2. Open that folder in your code editor.
3. Create a file named `index.html`. (The name `index` is important; web servers automatically look for a file named `index.html` to serve as the default page for a directory).

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Local Workspace Test</title>
</head>
<body>
    <h1>My Local Environment is Working!</h1>
    <p>I can edit this file and see changes when I refresh.</p>
</body>
</html>
```

</details>

<details>
<summary><strong>Example: Using DevTools to test changes</strong></summary>

Imagine you have this HTML file open in your browser:

```html
<!DOCTYPE html>
<html lang="en">
<body>
    <!-- 
      Special Attribute: 'id'
      - Provides a unique identifier for this element.
      - Useful for targeting with CSS or DevTools.
    -->
    <h1 id="main-heading">Hello World</h1>
</body>
</html>
```

To test changes using DevTools without altering the source file:
1. Open DevTools (`Inspect`).
2. In the Elements panel, find `<h1 id="main-heading">Hello World</h1>`.
3. Double click the text "Hello World" inside the Elements panel.
4. Type "Hello Universe" and press Enter.
5. The webpage will immediately update to say "Hello Universe", but your actual HTML file on your hard drive remains unchanged.

</details>
