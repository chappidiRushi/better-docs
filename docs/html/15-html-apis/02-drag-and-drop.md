---
sidebar_position: 2
---

# 15.2 Drag and Drop

## Definitions

- **Drag and Drop (DnD)**: A user interface concept where the user clicks on a virtual object and drags it to a different location or onto another virtual object.
- **Draggable Attribute**: A global HTML attribute that indicates whether an element can be dragged.
- **DataTransfer Object**: A JavaScript object used to hold the data that is being dragged during a drag and drop operation.

## Beginner Level Introduction

Drag and Drop is a very common feature in modern web applications. You use it when you upload a file to Google Drive, or when you move a task from "To Do" to "Done" on a Kanban board like Trello.

In HTML5, Drag and Drop is a native standard. Any element on a webpage can be made draggable.

To make an element draggable, you simply add the `draggable="true"` attribute to it.

```html
<img src="logo.png" draggable="true">
```
*(Note: Links (`<a>`) and images (`<img>`) are draggable by default in most browsers).*

## Deep Dive

### The Drag and Drop Events

While making an element draggable is easy, actually *doing* something when it is dragged and dropped requires JavaScript. The HTML5 API relies heavily on event listeners.

**Events on the Draggable Element:**
- `dragstart`: Fires when the user starts dragging the element.
- `drag`: Fires continuously while the element is being dragged.
- `dragend`: Fires when the drag operation ends (mouse release).

**Events on the Drop Target (The Drop Zone):**
- `dragenter`: Fires when the dragged element enters the drop zone.
- `dragover`: Fires continuously as the dragged element hovers over the drop zone. **(Crucial: By default, HTML elements cannot be dropped into other elements. To allow a drop, you MUST prevent the default behavior of the `dragover` event).**
- `dragleave`: Fires when the dragged element leaves the drop zone.
- `drop`: Fires when the dragged element is dropped on the drop zone.

### The DataTransfer Object

When you drag an element, how does the drop zone know *what* was dropped? 

The event object passed to the drag events contains a `dataTransfer` property. 
During `dragstart`, you use `dataTransfer.setData()` to store the ID or the data of the dragged element. 
During the `drop` event, you use `dataTransfer.getData()` to retrieve it and move the element in the DOM.

## Examples

<details>
<summary><strong>Example: A Complete Drag and Drop Implementation</strong></summary>

```html
<style>
  /* Styling for the drop zones */
  .drop-zone {
    width: 200px;
    height: 200px;
    padding: 10px;
    border: 2px dashed #aaaaaa;
    display: inline-block;
    margin: 10px;
  }
  
  /* Styling for the draggable item */
  .draggable-item {
    width: 180px;
    height: 50px;
    background-color: lightblue;
    text-align: center;
    line-height: 50px;
    cursor: grab;
  }
</style>

<!-- Drop Zone 1 -->
<div id="zone1" class="drop-zone" 
     ondrop="drop(event)" 
     ondragover="allowDrop(event)">
  
  <!-- The Draggable Element -->
  <!-- 
    Special Attributes:
    - draggable="true": Enables dragging.
    - ondragstart: The JS function that runs when dragging begins.
  -->
  <div id="drag1" class="draggable-item" draggable="true" ondragstart="drag(event)">
    Drag Me!
  </div>

</div>

<!-- Drop Zone 2 -->
<div id="zone2" class="drop-zone" 
     ondrop="drop(event)" 
     ondragover="allowDrop(event)">
</div>

<script>
  // 1. allowDrop: Fired on the drop zone while hovering
  function allowDrop(ev) {
    // CRITICAL: We must prevent default to allow dropping
    ev.preventDefault();
  }

  // 2. drag: Fired on the draggable element when dragging starts
  function drag(ev) {
    // Store the ID of the dragged element ('drag1') in the dataTransfer object
    ev.dataTransfer.setData("text", ev.target.id);
  }

  // 3. drop: Fired on the drop zone when the mouse is released
  function drop(ev) {
    ev.preventDefault();
    // Retrieve the ID we stored earlier
    var data = ev.dataTransfer.getData("text");
    
    // Append the dragged element to the new drop zone in the DOM
    ev.target.appendChild(document.getElementById(data));
  }
</script>
```

</details>
