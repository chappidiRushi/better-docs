---
id: compound-components
title: Compound Components
sidebar_position: 14
-------------------
# Compound Components

> Guide for building flexible and reusable React components using the Compound Components pattern

---

## What are Compound Components?

Compound components are a pattern where multiple components work together and share implicit state, usually via context, allowing a clean API for parent-child composition.

---

## Core Concepts

| Concept          | Purpose                                                      |
| ---------------- | ------------------------------------------------------------ |
| Parent Component | Provides shared state and context to its children            |
| Child Components | Consume the shared state from context and render accordingly |
| Context          | Mechanism to share state without prop drilling               |

---

## Example: Tabs Component

<details>
<summary>Example: Tabs with Compound Components</summary>

```js
import React, { useState, createContext, useContext } from 'react';

const TabsContext = createContext();

function Tabs({ children, defaultIndex = 0 }) {
  const [activeIndex, setActiveIndex] = useState(defaultIndex);
  return (
    <TabsContext.Provider value={{ activeIndex, setActiveIndex }}>
      <div>{children}</div>
    </TabsContext.Provider>
  );
}

function TabList({ children }) {
  return <div className="tab-list">{children}</div>;
}

function Tab({ index, children }) {
  const { activeIndex, setActiveIndex } = useContext(TabsContext);
  return (
    <button
      className={activeIndex === index ? 'active' : ''}
      onClick={() => setActiveIndex(index)}
    >
      {children}
    </button>
  );
}

function TabPanels({ children }) {
  const { activeIndex } = useContext(TabsContext);
  return <div className="tab-panels">{children[activeIndex]}</div>;
}

function TabPanel({ children }) {
  return <div>{children}</div>;
}

function App() {
  return (
    <Tabs defaultIndex={0}>
      <TabList>
        <Tab index={0}>Tab 1</Tab>
        <Tab index={1}>Tab 2</Tab>
      </TabList>
      <TabPanels>
        <TabPanel>Content 1</TabPanel>
        <TabPanel>Content 2</TabPanel>
      </TabPanels>
    </Tabs>
  );
}
```

</details>

---

## Benefits

| Benefit             | Description                                                              |
| ------------------- | ------------------------------------------------------------------------ |
| Flexible API        | Users can place child components in any order and still work correctly   |
| Avoid Prop Drilling | Context shares state implicitly among children                           |
| Reusable            | Components can be used independently or together in various layouts      |
| Encapsulation       | Parent manages state, children consume it without knowing implementation |

---

## Best Practices

* Use context internally to share state among children.
* Keep API simple and intuitive.
* Avoid exposing unnecessary internal state to consumers.
* Combine with hooks to simplify state management.
* Use TypeScript or PropTypes to enforce correct child components if needed.

---

## Notes & Caveats

* Compound components rely heavily on React context, so be mindful of performance if state updates frequently.
* Ensure that children are only used within the parent component to avoid errors.
* This pattern is ideal for components like Tabs, Accordions, Dropdowns, and Modals.
