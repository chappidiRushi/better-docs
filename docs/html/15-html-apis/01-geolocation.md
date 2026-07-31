---
sidebar_position: 1
---

# 15.1 Geolocation

## Definitions

- **Geolocation API**: An HTML5 API that allows the user to provide their geographical location to web applications.
- **Latitude and Longitude**: The coordinate system used to pinpoint a location on Earth.
- **HTTPS**: Hypertext Transfer Protocol Secure. The Geolocation API only works on secure contexts (HTTPS).

## Beginner Level Introduction

### What is Geolocation?

The HTML Geolocation API is used to locate a user's position. 

Because this compromises privacy, the position is not available unless the user explicitly approves it. When a website tries to use the Geolocation API, the browser will pop up a prompt asking the user to "Allow" or "Block" the location request.

## Deep Dive

### How it Works

Geolocation is accessed via JavaScript using the `navigator.geolocation` object.

The most important method is `getCurrentPosition()`, which takes three arguments:
1. **Success callback**: A function that runs if the location is successfully retrieved. It receives a `Position` object containing the latitude, longitude, and accuracy.
2. **Error callback (optional)**: A function that runs if something goes wrong (e.g., the user denied permission, or the GPS is off).
3. **Options (optional)**: Settings like `enableHighAccuracy` or `timeout`.

### Security Restrictions

Because location data is highly sensitive:
1. **User Permission is Mandatory**: You cannot bypass the browser's permission prompt.
2. **HTTPS is Required**: Modern browsers will block the Geolocation API entirely if the website is not served over a secure HTTPS connection. (It will work on `localhost` for development purposes).

### Accuracy

The accuracy of the location depends on the device:
- A desktop computer connected via Ethernet will guess the location based on the IP address (low accuracy, often just the city).
- A smartphone with a GPS chip enabled can pinpoint the location within a few meters (high accuracy).

### Tracking Movement

If you are building a navigation app or a fitness tracker, getting the location once is not enough. You can use the `watchPosition()` method. It acts like an event listener, calling the success function every time the user's location changes. You stop tracking using `clearWatch()`.

## Examples

<details>
<summary><strong>Example: Getting the User's Coordinates</strong></summary>

```html
<button onclick="getLocation()">Where am I?</button>
<p id="demo"></p>

<script>
  const displayElement = document.getElementById("demo");

  function getLocation() {
    // 1. Check if the browser supports Geolocation
    if (navigator.geolocation) {
      // 2. Call the API, passing the success and error functions
      navigator.geolocation.getCurrentPosition(showPosition, showError);
    } else { 
      displayElement.innerHTML = "Geolocation is not supported by this browser.";
    }
  }

  // 3. The Success Callback
  function showPosition(position) {
    displayElement.innerHTML = "Latitude: " + position.coords.latitude + 
    "<br>Longitude: " + position.coords.longitude;
  }

  // 4. The Error Callback
  function showError(error) {
    switch(error.code) {
      case error.PERMISSION_DENIED:
        displayElement.innerHTML = "User denied the request for Geolocation."
        break;
      case error.POSITION_UNAVAILABLE:
        displayElement.innerHTML = "Location information is unavailable."
        break;
      case error.TIMEOUT:
        displayElement.innerHTML = "The request to get user location timed out."
        break;
      case error.UNKNOWN_ERROR:
        displayElement.innerHTML = "An unknown error occurred."
        break;
    }
  }
</script>
```

</details>

<details>
<summary><strong>Example: Displaying the Location on Google Maps</strong></summary>

```html
<button onclick="showMap()">Show on Map</button>
<div id="map-container"></div>

<script>
  function showMap() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(function(position) {
        const lat = position.coords.latitude;
        const lon = position.coords.longitude;
        
        // Construct a Google Maps Embed URL
        const mapUrl = `https://maps.google.com/maps?q=${lat},${lon}&z=15&output=embed`;
        
        // Create an iframe to display the map
        document.getElementById("map-container").innerHTML = 
          `<iframe width="500" height="400" src="${mapUrl}"></iframe>`;
      });
    }
  }
</script>
```

</details>
