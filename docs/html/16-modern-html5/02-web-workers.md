---
sidebar_position: 2
---

# 16.2 Web Workers

## Definitions

- **Web Worker**: A JavaScript running in the background, independently of other scripts, without affecting the performance of the page.
- **Main Thread**: The primary thread of execution in a web browser where the UI is updated and most JavaScript is run.
- **Blocking**: When a heavy computation stops the Main Thread, causing the web page to freeze and become unresponsive.

## Beginner Level Introduction

JavaScript is a "single-threaded" language. This means it can only do one thing at a time. 

If you write a script that takes 5 seconds to calculate a massive math problem, the entire web page will freeze for 5 seconds. The user won't be able to click buttons, scroll, or type in forms until the calculation is finished. This is called blocking the main thread.

HTML5 introduced **Web Workers** to solve this. A Web Worker allows you to run a JavaScript file in a separate background thread. It does the heavy lifting behind the scenes, allowing the main webpage to remain perfectly smooth and responsive.

## Deep Dive

### How Web Workers Operate

Because Web Workers run in a separate thread, they do **not** have access to the main page's DOM (Document Object Model). 
A Web Worker cannot:
- Change an HTML element.
- Access the `window` object.
- Access the `document` object.

A Web Worker *can*:
- Make network requests (using `fetch` or `XMLHttpRequest`).
- Access `navigator` and `location`.
- Use `setTimeout` and `setInterval`.
- Use WebSockets.

### Communication via Messaging

If a Web Worker can't touch the DOM, how does it show its results to the user? 

The main script and the Web Worker script communicate by passing messages back and forth using the `postMessage()` method and listening for the `onmessage` event.

1. **Main Script:** Sends data to the worker -> `worker.postMessage(data)`
2. **Worker Script:** Does the heavy math.
3. **Worker Script:** Sends result back -> `postMessage(result)`
4. **Main Script:** Receives result and updates the DOM.

### Terminating a Worker

Web Workers consume system resources. When a worker has finished its job, you should terminate it to free up memory.
- From the main script: `worker.terminate();`
- From inside the worker script itself: `close();`

## Examples

<details>
<summary><strong>Example: Implementing a Web Worker</strong></summary>

For this example, you need two separate files: the main HTML file, and the JavaScript worker file.

**File 1: worker.js (The Background Script)**
```javascript
// This script runs in the background. It cannot touch the HTML.

// Listen for messages from the main script
onmessage = function(e) {
  // e.data contains the message sent from the main script
  const userNumber = e.data;
  
  // Do some heavy calculation (e.g., finding prime numbers, complex math)
  let result = 0;
  for (let i = 0; i < 1000000000; i++) {
    result += i * userNumber;
  }
  
  // Send the final result back to the main script
  postMessage(result);
}
```

**File 2: index.html (The Main Script)**
```html
<p>Enter a number for the heavy calculation:</p>
<input type="number" id="myNum" value="5">
<button onclick="startCalculation()">Calculate</button>

<p>Result: <span id="result">Waiting...</span></p>

<!-- 
  Notice you can still click this button and the page works perfectly, 
  even while the heavy calculation is running! 
-->
<button onclick="alert('The UI is not frozen!')">Test UI Responsiveness</button>

<script>
  let w;

  function startCalculation() {
    // 1. Check if the browser supports Web Workers
    if (typeof(Worker) !== "undefined") {
      
      // 2. Initialize the worker, pointing to the external JS file
      if (typeof(w) == "undefined") {
        w = new Worker("worker.js");
      }
      
      const num = document.getElementById("myNum").value;
      
      // 3. Send data TO the worker
      w.postMessage(num);
      
      document.getElementById("result").innerText = "Calculating in background...";

      // 4. Listen for the result coming BACK from the worker
      w.onmessage = function(event) {
        // Update the DOM with the result
        document.getElementById("result").innerText = event.data;
        
        // Terminate the worker since it's done
        w.terminate();
        w = undefined;
      };
      
    } else {
      document.getElementById("result").innerHTML = "Sorry! No Web Worker support.";
    }
  }
</script>
```

</details>
