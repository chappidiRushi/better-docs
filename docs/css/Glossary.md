---

id: css-glossary
title: CSS Glossary
sidebar_position: 1
-------------------

# CSS Glossary (Interview‑Friendly)

---

## 🟢 Beginner Level (Core Fundamentals)

* **CSS (Cascading Style Sheets)** is a stylesheet language used to describe the presentation of HTML documents.

* **Rule Set** consists of a selector and a declaration block.

* **Selector** defines which elements a set of styles applies to.

* **Declaration** is a property–value pair that applies a style.

* **Property** is a specific aspect of styling (e.g., `color`, `margin`).

* **Value** specifies how a property is applied.

* **Box Model** defines how elements are sized and spaced.

* **Content Box** contains the actual content of the element.

* **Padding** is the space between content and border.

* **Border** surrounds padding and content.

* **Margin** is the outer space separating elements.

* **Box Sizing** controls how width and height are calculated.

* **Display** determines how an element participates in layout.

* **Block** elements create line breaks and fill available width.

* **Inline** elements flow within text and ignore vertical dimensions.

* **Inline‑Block** elements combine inline flow with block sizing.

* **Typography** refers to text‑related styling.

* **Font Family** defines prioritized fonts for text rendering.

* **Font Size** controls text size.

* **Line Height** defines vertical spacing between lines of text.

* **Text Alignment** controls horizontal alignment of text.

* **Color Model** defines how colors are represented (RGB, HSL, HEX).

* **Background** is a shorthand for background‑related properties.

### ⚠️ Beginner Interview Traps

* Confusing **margin vs padding** when asked about spacing bugs.
* Assuming `width` includes padding/border without mentioning `box-sizing`.
* Saying **inline elements can’t have width/height** without qualifying exceptions.

---

## 🟡 Intermediate Level (Layout, Responsiveness, Behavior)

* **Cascade** is the algorithm that determines which styles win when multiple rules apply.

* **Specificity** is the weight system used to resolve conflicting CSS rules.

* **Source Order** refers to the order in which rules appear in the stylesheet.

* **Importance** is the override mechanism provided by `!important`.

* **Inheritance** is the mechanism by which some properties are passed from parent to child elements.

* **Initial Value** is the default value a property takes when not specified.

* **Computed Value** is the resolved value after inheritance and cascading.

* **Used Value** is the value after layout calculations are applied.

* **Positioning** controls how elements are placed in the document.

* **Static** is the default positioning mode.

* **Relative** positions an element relative to itself.

* **Absolute** removes an element from normal flow and positions it relative to an ancestor.

* **Fixed** positions an element relative to the viewport.

* **Sticky** toggles between relative and fixed based on scroll.

* **Overflow** controls how content exceeding its container is handled.

* **Float** shifts elements left or right within a container.

* **Clear** prevents elements from wrapping around floats.

* **Flexbox** is a one‑dimensional layout model for distributing space.

* **Flex Container** is an element with `display: flex`.

* **Flex Item** is a direct child of a flex container.

* **Main Axis** is the primary direction of flex layout.

* **Cross Axis** is perpendicular to the main axis.

* **Responsive Design** adapts layout to different screen sizes.

* **Media Query** conditionally applies styles based on device characteristics.

* **Mobile‑First** is the strategy of styling for small screens first.

* **Pseudo‑Class** targets an element state (e.g., `:hover`).

* **Pseudo‑Element** targets part of an element (e.g., `::before`).

* **Transition** animates changes between property values.

* **Animation** defines keyframe‑based motion.

### ⚠️ Intermediate Interview Traps

* Failing to explain **specificity vs source order** with an example.
* Misstating what **`position: absolute` is relative to**.
* Treating Flexbox as two‑dimensional (it’s not).
* Forgetting that **media queries don’t change specificity**.

---

## 🔴 Hardcore Level (Rendering, Performance, Architecture)

* **Grid** is a two‑dimensional layout system for rows and columns.

* **Grid Container** establishes a grid formatting context.

* **Grid Item** is a direct child of a grid container.

* **Track** is a row or column in a grid.

* **Implicit Grid** is auto‑generated grid tracks.

* **Explicit Grid** is developer‑defined grid tracks.

* **Z‑Index** controls stacking order of positioned elements.

* **Stacking Context** is a hierarchy that determines rendering order.

* **Opacity** controls element transparency.

* **Reflow** is recalculation of layout due to structural changes.

* **Repaint** is redrawing pixels without layout changes.

* **Performance** refers to minimizing layout thrashing and paint costs.

* **CSS Variables** are custom properties scoped to the DOM.

* **Preprocessor** is a tool that extends CSS with variables and logic.

* **Post‑Processor** transforms CSS after it is written (e.g., Autoprefixer).

* **Browser Compatibility** is ensuring styles work across user agents.

* **Progressive Enhancement** builds core styles first, then adds complexity.

* **Graceful Degradation** ensures acceptable experience on older browsers.

### ⚠️ Hardcore Interview Traps

* Not knowing **what creates a stacking context**.
* Saying `z-index` works without positioning.
* Confusing **reflow vs repaint** and their performance impact.
* Overusing animations on layout‑affecting properties (`top`, `left`).
