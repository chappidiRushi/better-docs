---
sidebar_position: 1
---

# 5.1 HTML Links

## Definitions

- **Link (Hyperlink)**: A reference to data that the user can follow by clicking or tapping. It is the defining feature of the World Wide Web.
- **Anchor Element (`<a>`)**: The HTML element used to create hyperlinks.
- **URL (Uniform Resource Locator)**: The address of a given unique resource on the Web.
- **Absolute URL**: A full web address that includes the protocol (e.g., `https://`) and the domain name (e.g., `www.example.com`).
- **Relative URL**: A path to a file relative to the current page's location. It does not include the domain name.

## Beginner Level Introduction

### The Anchor Element

Links are found in nearly all web pages. They allow users to click their way from page to page.

In HTML, links are defined with the `<a>` (anchor) tag.

```html
<a href="url">link text</a>
```
- The `href` (Hypertext REFerence) attribute specifies the destination address of the link.
- The `link text` is the visible part that the user clicks on.

By default, links will appear as follows in all browsers:
- An unvisited link is underlined and **blue**.
- A visited link is underlined and **purple**.
- An active link is underlined and **red**.

### Absolute vs Relative URLs

When linking to pages on *other* websites, you must use an **Absolute URL**.

```html
<a href="https://www.google.com">Go to Google</a>
```

When linking to pages within your *own* website, it is best practice to use **Relative URLs**. This makes your site easier to move from a local testing environment to a live server because the domain name isn't hardcoded.

```html
<!-- Links to a file in the same folder -->
<a href="about.html">About Us</a>

<!-- Links to a file in a sub-folder called "images" -->
<a href="images/map.html">View Map</a>
```

## Deep Dive

### Opening Links (`target` attribute)

By default, when a user clicks a link, the linked page will open in the current browser window/tab, replacing the page they were just on.

To change this behavior, use the `target` attribute.

- `target="_self"`: Default. Opens the document in the same window/tab as it was clicked.
- `target="_blank"`: Opens the document in a new window or tab. (Commonly used for external links).
- `target="_parent"`: Opens the document in the parent frame.
- `target="_top"`: Opens the document in the full body of the window (breaks out of iframes).

### Email Links (`mailto:`)

You can create a link that automatically opens the user's default email program (like Outlook or Apple Mail) and starts a new email.

To do this, use `mailto:` inside the `href` attribute.

```html
<a href="mailto:someone@example.com">Send email</a>
```

You can even pre-fill the subject line:
```html
<a href="mailto:someone@example.com?subject=Hello%20There">Send email</a>
```
*(Note: spaces are replaced by `%20` in URLs).*

### Download Links

If you want a link to download a file instead of navigating to it (e.g., downloading a PDF instead of opening it in the browser), add the `download` boolean attribute.

```html
<a href="report.pdf" download>Download Annual Report</a>
```

You can also specify a new filename for the downloaded file by giving the `download` attribute a value.

```html
<a href="report_2023_vfinal.pdf" download="2023_Report.pdf">Download</a>
```

## Examples

<details>
<summary><strong>Example: Standard Linking</strong></summary>

```html
<!-- 
  Special Attribute: href 
  - Essential for the anchor tag to function as a link.
-->
<p>You can read more about HTML at <a href="https://developer.mozilla.org/">MDN Web Docs</a>.</p>
```

</details>

<details>
<summary><strong>Example: Opening in a New Tab</strong></summary>

```html
<!-- 
  Special Attribute: target="_blank"
  - Opens the link in a new tab.
  - Security tip: When using target="_blank" on external links, 
    it was historically recommended to add rel="noopener noreferrer" 
    to prevent the new tab from accessing the original tab's window object.
    Modern browsers now do this automatically, but you may still see it in older code.
-->
<a href="https://www.wikipedia.org/" target="_blank">Open Wikipedia in new tab</a>
```

</details>

<details>
<summary><strong>Example: Complex Email Link</strong></summary>

```html
<!-- 
  Special href prefix: mailto:
  - ?subject= sets the subject
  - &body= sets the body content
-->
<a href="mailto:support@example.com?subject=Need%20Help&body=Please%20describe%20your%20issue:">
  Contact Support
</a>
```

</details>
