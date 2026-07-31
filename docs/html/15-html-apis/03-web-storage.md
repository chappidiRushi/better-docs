---
sidebar_position: 3
---

# 15.3 Web Storage

## Definitions

- **Web Storage API**: Provides mechanisms by which browsers can store key/value pairs locally within the user's browser, much more intuitively than using cookies.
- **Local Storage (`localStorage`)**: Stores data with no expiration date. The data remains even after the browser is closed or the computer is restarted.
- **Session Storage (`sessionStorage`)**: Stores data for one session. The data is lost when the browser tab is closed.

## Beginner Level Introduction

Before HTML5, web applications had to store user data in **Cookies**, which were included in every single server request. This was slow, insecure, and limited to about 4KB of data.

HTML5 introduced Web Storage, which is faster, more secure, and allows you to store a large amount of data (usually at least 5MB) locally on the user's computer. The data is *not* sent to the server with every HTTP request; it is only available via JavaScript.

There are two objects for storing data:
- `window.localStorage`
- `window.sessionStorage`

## Deep Dive

### How Web Storage Works

Both `localStorage` and `sessionStorage` use the exact same methods. Data is always stored as a string key/value pair.

**Basic Methods:**
1. `setItem(key, value)`: Saves data to the storage.
2. `getItem(key)`: Retrieves data based on the key.
3. `removeItem(key)`: Deletes a specific item based on the key.
4. `clear()`: Deletes everything in the storage for that specific domain.

```javascript
// Saving data
localStorage.setItem("username", "JohnDoe");

// Retrieving data
let user = localStorage.getItem("username");
```

### Storing Objects

Web Storage can only store strings. If you want to store a JavaScript object or an array (like a user's shopping cart or preferences), you must convert it to a JSON string using `JSON.stringify()` before saving it, and convert it back to an object using `JSON.parse()` when retrieving it.

```javascript
const userProfile = { name: "Jane", theme: "dark" };

// Save as string
localStorage.setItem("profile", JSON.stringify(userProfile));

// Retrieve and parse back to object
const savedData = JSON.parse(localStorage.getItem("profile"));
```

### Security Considerations

**Never store sensitive information (like passwords, credit card numbers, or secure authentication tokens) in Web Storage.**

Web Storage is vulnerable to Cross-Site Scripting (XSS) attacks. If a hacker manages to run malicious JavaScript on your website, that script has full access to read everything inside `localStorage` and send it to the hacker's server. Secure authentication tokens should be stored in HTTP-Only, Secure cookies instead.

Web Storage is fantastic for saving UI state (e.g., "is the sidebar collapsed?"), light mode/dark mode preferences, or caching non-sensitive API data to make the site load faster.

## Examples

<details>
<summary><strong>Example: Saving a Theme Preference (Local Storage)</strong></summary>

```html
<p>Choose your theme. If you refresh or close the browser, your choice is remembered!</p>

<button onclick="setTheme('light')">Light Mode</button>
<button onclick="setTheme('dark')">Dark Mode</button>
<button onclick="clearTheme()">Reset</button>

<script>
  // 1. When the page loads, check if a theme is already saved
  document.addEventListener("DOMContentLoaded", function() {
    const savedTheme = localStorage.getItem("preferredTheme");
    if (savedTheme) {
      applyTheme(savedTheme);
    }
  });

  // 2. Function to save the theme and apply it
  function setTheme(themeName) {
    // Save to localStorage
    localStorage.setItem("preferredTheme", themeName);
    applyTheme(themeName);
  }

  // 3. Helper function to change the background color
  function applyTheme(themeName) {
    if (themeName === 'dark') {
      document.body.style.backgroundColor = "black";
      document.body.style.color = "white";
    } else {
      document.body.style.backgroundColor = "white";
      document.body.style.color = "black";
    }
  }

  // 4. Function to clear the saved data
  function clearTheme() {
    localStorage.removeItem("preferredTheme");
    applyTheme('light'); // Revert to default
    alert("Preferences cleared!");
  }
</script>
```

</details>

<details>
<summary><strong>Example: A Simple Shopping Cart (Session Storage)</strong></summary>

```html
<!-- 
  This data will be lost when the tab is closed, 
  which is often desired for an active shopping session.
-->
<button onclick="addToCart('Apple')">Add Apple</button>
<button onclick="addToCart('Banana')">Add Banana</button>
<button onclick="viewCart()">View Cart</button>
<p id="cartDisplay"></p>

<script>
  function addToCart(item) {
    // Get existing cart or create empty array
    let cartString = sessionStorage.getItem("shoppingCart");
    let cart = cartString ? JSON.parse(cartString) : [];
    
    // Add new item
    cart.push(item);
    
    // Save back to sessionStorage as a string
    sessionStorage.setItem("shoppingCart", JSON.stringify(cart));
    alert(item + " added to cart!");
  }

  function viewCart() {
    let cartString = sessionStorage.getItem("shoppingCart");
    if (cartString) {
      let cart = JSON.parse(cartString);
      document.getElementById("cartDisplay").innerText = "Items in cart: " + cart.join(", ");
    } else {
      document.getElementById("cartDisplay").innerText = "Cart is empty.";
    }
  }
</script>
```

</details>
