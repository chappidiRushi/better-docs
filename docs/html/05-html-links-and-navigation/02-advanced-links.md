---
sidebar_position: 2
---

# 5.2 Advanced Links

## Definitions

- **Bookmark Link (Anchor Link)**: A link that jumps to a specific section of a webpage, rather than opening a new page entirely.
- **`rel` Attribute**: Specifies the relationship between the current document and the linked document/resource.
- **Image Link**: Using an `<img>` element as the clickable content of an `<a>` element, instead of text.

## Beginner Level Introduction

### Link Colors (Styling)

As mentioned in the previous section, default browser links are blue, purple, and red. You can (and almost always will) change this using CSS.

You use CSS pseudo-classes to style the different states of a link:
- `a:link`: A normal, unvisited link.
- `a:visited`: A link the user has visited.
- `a:hover`: A link when the user mouses over it.
- `a:active`: A link the moment it is clicked.

```css
/* Example CSS */
a:link { color: green; text-decoration: none; }
a:visited { color: darkgreen; }
a:hover { color: red; text-decoration: underline; }
```

### Image Links

Links don't have to be text. You can wrap any HTML element inside an `<a>` tag to make it clickable. The most common use case is making an image clickable.

```html
<a href="https://www.example.com">
  <img src="banner.jpg" alt="Click here to visit example.com">
</a>
```

## Deep Dive

### Bookmark Links (Jumping within a page)

Bookmark links are incredibly useful for long webpages (like a Wikipedia article or a Terms of Service page). They allow the user to click a link and instantly scroll to a specific section on the *same* page.

**Step 1: Create the target**
Give the element you want to jump to a unique `id` attribute.
```html
<h2 id="section-pricing">Pricing Plans</h2>
```

**Step 2: Create the link**
In the `href` attribute of your link, use the `#` symbol followed by the `id` of the target element.
```html
<a href="#section-pricing">Jump to Pricing</a>
```

If you want to jump to a specific section on a *different* page, combine the URL and the hash:
```html
<a href="about.html#team-members">Meet the Team</a>
```

### External Resources and the `rel` Attribute

The `rel` attribute defines the relationship between the page containing the link and the page being linked to. This is crucial for SEO and security.

Common values for `rel` on `<a>` tags:

- `rel="nofollow"`: Tells search engines "I am linking to this site, but I do not endorse it." This prevents search engine "link juice" from passing to the target site. Often used for paid links or user-generated content (like blog comments) to prevent spam.
- `rel="noopener noreferrer"`: Used for security when opening links with `target="_blank"`. It prevents the newly opened page from gaining access to the original page's `window.opener` object, preventing a specific type of phishing attack.
- `rel="author"`: Indicates that the link points to the author of the document.

## Examples

<details>
<summary><strong>Example: Table of Contents using Bookmarks</strong></summary>

```html
<h1>Long Article Document</h1>

<!-- The Navigation / Table of Contents -->
<nav>
  <ul>
    <!-- 
      Special href syntax: #id
      - Instructs the browser to scroll down to the element with that ID.
    -->
    <li><a href="#chapter1">Chapter 1: Introduction</a></li>
    <li><a href="#chapter2">Chapter 2: The Rising Action</a></li>
  </ul>
</nav>

<!-- Much later in the document... -->
<div style="margin-top: 1000px;">
  <!-- The Target Elements -->
  <h2 id="chapter1">Chapter 1: Introduction</h2>
  <p>It was a dark and stormy night...</p>

  <h2 id="chapter2">Chapter 2: The Rising Action</h2>
  <p>Suddenly, a knock at the door...</p>
</div>
```

</details>

<details>
<summary><strong>Example: Secure External Link with SEO Rules</strong></summary>

```html
<p>
  This blog post was sponsored by 
  <!--
    Special Attributes:
    - target="_blank": Opens in new tab.
    - rel="nofollow": Tells Google not to pass ranking credit (required for sponsored links).
    - rel="noopener": Ensures the new tab cannot hijack this page.
  -->
  <a href="https://www.sketchy-sponsor.com" 
     target="_blank" 
     rel="nofollow noopener">
    Sketchy Sponsor Co.
  </a>
</p>
```

</details>
