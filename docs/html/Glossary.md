---

id: glossary
title: Glossary
sidebar_position: 1
-------------------

# HTML Glossary (Interview‑Friendly)

---

* **HTML (HyperText Markup Language)** is a declarative markup language used to structure documents on the web.

* **HTML Living Standard** is the continuously updated HTML specification maintained by WHATWG.

* **Element** is a semantic unit defined by a tag, attributes, and content, represented as a node in the DOM.

* **Tag** is the syntactic marker (`<tag>`) that denotes the start or end of an element.

* **Void Element** is an element that cannot have content or an end tag (e.g., `<img>`, `<br>`).

* **Attribute** is a name–value pair that modifies element behavior or semantics.

* **Boolean Attribute** is an attribute whose presence represents true and absence represents false.

* **Global Attributes** are attributes applicable to all HTML elements (e.g., `id`, `class`, `hidden`).

* **Custom Data Attributes (`data-*`)** are author-defined attributes for embedding custom metadata.

* **Document** is the root of the DOM tree representing an HTML page.

* **DOM (Document Object Model)** is a tree-based in-memory representation of the document used by browsers.

* **Node** is the base interface for all DOM tree objects (elements, text, comments).

* **Text Node** is a DOM node representing textual content.

* **Comment Node** is a DOM node representing an HTML comment.

* **DOCTYPE** is a required declaration that triggers standards-compliant parsing.

* **Standards Mode** is the browser rendering mode enabled by a valid DOCTYPE.

* **Quirks Mode** is a legacy rendering mode for backward compatibility.

* **Parsing** is the process of converting HTML bytes into a DOM tree.

* **Error Recovery** is the browser’s ability to handle invalid HTML by applying parsing rules.

* **Content Model** defines what elements or content are permitted inside another element.

* **Flow Content** is the main content category used inside the `<body>` element.

* **Phrasing Content** is inline-level text and text-related elements.

* **Sectioning Content** defines document outline and landmarks (e.g., `<section>`, `<article>`).

* **Metadata Content** is content that sets up document metadata (e.g., `<meta>`, `<title>`).

* **Interactive Content** is content intended for user interaction (e.g., `<button>`, `<a>`).

* **Embedded Content** is content that imports external resources (e.g., `<img>`, `<video>`).

* **Accessible Name** is the computed label exposed to assistive technologies.

* **ARIA (Accessible Rich Internet Applications)** is a set of attributes that extend accessibility semantics.

* **Landmark Role** is an accessibility role defining major document regions.

* **Form Control** is an element that allows user input and participates in form submission.

* **Constraint Validation** is the browser’s native form validation mechanism.

* **Serialization** is the process of converting form controls into name/value pairs.

* **Shadow DOM** is an encapsulated DOM subtree with scoped styles and markup.

* **Custom Element** is a user-defined HTML element registered via the Custom Elements API.

* **Web Component** is a standards-based, reusable UI unit composed of Custom Elements and Shadow DOM.

* **Template Element (`<template>`)** is an inert DOM fragment used for cloning.

* **Canvas** is a bitmap rendering surface controlled entirely by JavaScript.

* **SVG (Scalable Vector Graphics)** is a retained-mode, XML-based vector graphics format.

* **Critical Rendering Path** is the sequence of steps browsers perform to render pixels.

* **Preload Scanner** is a speculative parser that discovers resources early.

* **Script Blocking** occurs when script execution pauses HTML parsing.

* **`async` Script** executes independently of document parsing order.

* **`defer` Script** executes after parsing but before `DOMContentLoaded`.

* **Module Script** is an ES module-based script with deferred execution and scoped imports.

* **CSP (Content Security Policy)** is a security mechanism that restricts resource loading.

* **Sandboxing** is the restriction of embedded content capabilities, commonly via `<iframe>`.

* **Origin** is the tuple of scheme, host, and port defining a security boundary.

* **Viewport** is the layout area in which the document is rendered.

* **Responsive Markup** is HTML structured to adapt across device sizes.

* **HTML Namespace** is the XML namespace implicitly used for HTML elements in the DOM.

* **Foreign Content** refers to non-HTML namespaces embedded in HTML (e.g., SVG, MathML).

* **DOM Tree Order** is the depth-first order used by browsers to traverse nodes.

* **Tree Builder** is the HTML parser stage that constructs the DOM from tokens.

* **Tokenizer** is the parser component that converts character streams into tokens.

* **Insertion Mode** is the parser state that determines how tokens are handled.

* **Foster Parenting** is a parser rule for handling misnested table content.

* **Adoption Agency Algorithm** is the parser algorithm that fixes misnested formatting elements.

* **Formatting Element** is an inline element that affects text presentation (e.g., `<b>`, `<i>`).

* **Active Formatting Elements List** is a parser structure used during error recovery.

* **Implicit End Tag** is an automatically generated end tag inserted by the parser.

* **DOM Token List** is a space-separated token interface (e.g., `classList`, `relList`).

* **Reflecting Attribute** is an attribute whose value is mirrored as a DOM property.

* **IDL Attribute** is a JavaScript-exposed property defined by Web IDL.

* **Enumerated Attribute** is an attribute with a fixed set of allowed keyword values.

* **URL Attribute** is an attribute whose value is parsed as a URL.

* **Event Target** is any object that can receive DOM events.

* **Event Bubbling** is the propagation of events from target to root.

* **Event Capturing** is the propagation of events from root to target.

* **Trusted Event** is a browser-generated event (`isTrusted === true`).

* **DOMContentLoaded** is fired when the DOM is fully parsed.

* **Load Event** fires when all dependent resources are loaded.

* **Render Tree** is the structure used by the browser to paint content.

* **Layout (Reflow)** is the calculation of element geometry.

* **Paint** is the process of filling pixels.

* **Composite** is the stage where layers are merged and displayed.

* **Repaint** is a visual update without layout recalculation.

* **Reflow** is a layout recalculation triggered by DOM or style changes.

* **Containment** is a CSS/HTML optimization boundary for layout and paint.

* **Intrinsic Size** is the natural size of replaced elements.

* **Replaced Element** is an element whose rendering is external to CSS (e.g., `<img>`, `<video>`).

* **Form Owner** is the form element associated with a control.

* **Form-Associated Custom Element** is a custom element that participates in form submission.

* **Successful Control** is a form control included in submission.

* **Name Collision** occurs when multiple controls share the same name.

* **Submitter** is the control that triggered form submission.

* **BFC (Browsing Context)** is an environment in which a document is presented.

* **Top-Level Browsing Context** is the main page context (window).

* **Nested Browsing Context** is an embedded context (iframe).

* **Session History** is the list of navigations in a browsing context.

* **Navigation Algorithm** defines how user agents load new documents.

* **Same-Origin Policy** restricts cross-origin DOM access.

* **CORS** is a controlled relaxation of the Same-Origin Policy.

* **Opaque Origin** is an origin with restricted access (e.g., sandboxed iframe).

* **Lazy Loading** defers resource loading (`loading="lazy"`).

* **Fetch Priority** hints browser resource importance.

* **Speculative Parsing** is parallel discovery of resources during parsing.

* **Inert Subtree** is DOM content excluded from interaction and accessibility.

* **Tab Order** is the navigation sequence for focusable elements.

* **Focus Management** is control of keyboard focus movement.

* **User Activation** is a transient state required for certain APIs.

* **Transient Activation** is a short-lived user gesture signal.

* **HTML Sanitization** is the removal of unsafe markup.

* **Trusted Types** is a security mechanism preventing DOM XSS sinks.

* **Declarative Shadow DOM** allows shadow trees in HTML markup.

* **Slotting** is the projection of light DOM into shadow DOM slots.

* **Part Attribute** exposes shadow DOM styling hooks.

* **CSSOM** is the object model representing parsed CSS.

* **Intersection Observer** is an API for visibility tracking.

* **Resize Observer** detects element size changes.

* **Mutation Observer** observes DOM changes.

* **HTML Collection** is a live, ordered DOM collection.

* **NodeList** is a collection of nodes (live or static).

* **Live Collection** updates automatically with DOM changes.

* **Static Collection** is a snapshot at query time.

* **Parsing Context** defines how HTML fragments are parsed.

* **InnerHTML** is a DOM API that parses and serializes HTML fragments.

* **OuterHTML** serializes an element including itself.

* **Document Fragment** is a lightweight container for DOM nodes.

* **Advisory Information** is non-essential metadata (e.g., `title` attribute).

* **Presentational Hint** is HTML-provided styling guidance overridden by CSS.
