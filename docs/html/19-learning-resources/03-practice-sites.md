---
sidebar_position: 3
---

# 19.3 Practice Sites

## Definitions

- **Online Playground**: A web-based code editor that allows you to write HTML, CSS, and JavaScript and see the results instantly, without needing to install anything on your computer.
- **Frontend Challenges**: Websites that provide you with a design file and ask you to recreate it using code.

## Beginner Level Introduction

You cannot learn to write HTML just by reading about it. You have to write code.

If you are just starting out and don't want to deal with creating files and folders on your computer, online playgrounds are the perfect place to experiment. 

When you feel comfortable with the basics, challenge sites are the best way to bridge the gap between "following a tutorial" and "building a real project."

## Deep Dive

### Online Code Playgrounds

These tools are invaluable for rapid prototyping or sharing code with others when asking for help.

1. **CodePen (codepen.io)**: The most popular frontend playground. It has three simple panels (HTML, CSS, JS) and a live preview. It's excellent for testing out small UI components or animations.
2. **CodeSandbox (codesandbox.io)**: A much more powerful tool. It runs a full Node.js environment in the browser. It is best used when you graduate from vanilla HTML/CSS and start learning frameworks like React or Vue.
3. **JSFiddle (jsfiddle.net)**: Very similar to CodePen, heavily used on Stack Overflow for providing reproducible code examples.

### Frontend Challenge Websites

Once you know the HTML tags, the hardest part is figuring out *what* to build. These sites solve that problem.

1. **Frontend Mentor (frontendmentor.io)**: Highly recommended. They provide professional Figma design files and image assets for projects ranging from "Newbie" (a single QR code card) to "Guru" (multi-page web apps). You write the code on your own machine and submit the result.
2. **CSSBattle (cssbattle.dev)**: A highly gamified site where your goal is to replicate a specific geometric target using the smallest amount of HTML/CSS code possible. (Great for learning CSS, but enforces terrible semantic HTML habits, so use with caution).
3. **100 Days of Code (100daysofcode.com)**: Not a website, but a public commitment challenge on Twitter/X to code for at least an hour a day for 100 days. It is a fantastic way to stay motivated and build a portfolio.

## Examples

<details>
<summary><strong>Example: How to approach a Frontend Challenge</strong></summary>

When you download a design from Frontend Mentor, don't just start writing HTML randomly.

1. **Analyze the Design**: Look at the picture. Draw imaginary boxes around the major sections. Where is the `<header>`? Where is the `<main>` content?
2. **Write the HTML First**: Write out all your semantic tags (`<article>`, `<h1>`, `<p>`, `<button>`) *before* you write a single line of CSS. Put placeholder text inside them.
3. **Add Classes**: Add CSS classes to your HTML elements based on their purpose (e.g., `class="card-title"`, `class="btn-primary"`).
4. **Style**: Open your CSS file and start applying layouts (Flexbox/Grid), colors, and typography to match the design.

</details>
