---
id: interview
title: Interview
sidebar_position: 1
-------------------

# CSS Interview Q&A

> 100+ questions. Short. Brutal. Precise. This is senior-level CSS literacy.

---

## 🟢 Beginner Level (Foundations)

### Q1. What does CSS stand for?

**A.** Cascading Style Sheets.

### Q2. Why is CSS called “cascading”?

**A.** Because multiple sources of styles cascade together via priority rules.

### Q3. What is a rule set?

**A.** Selector + declaration block.

### Q4. What is the box model?

**A.** Content, padding, border, margin.

### Q5. Default `box-sizing`?

**A.** `content-box`.

### Q6. Difference between margin and padding?

**A.** Margin affects outside spacing; padding affects inside spacing and background.

### Q7. Do margins collapse horizontally?

**A.** No, only vertically.

### Q8. What is `display: block`?

**A.** Full-width, new line, participates in block layout.

### Q9. What is `display: inline`?

**A.** Inline flow, no vertical box sizing.

### Q10. Can inline elements have padding?

**A.** Yes, but vertical padding doesn’t affect line height.

### Q11. Difference between `none` and `hidden` visibility?

**A.** Layout removal vs invisible rendering.

### Q12. What unit is relative to root font size?

**A.** `rem`.

### Q13. What unit is relative to parent font size?

**A.** `em`.

### Q14. What is inheritance?

**A.** Some properties flow from parent to child.

### Q15. Name a non-inherited property.

**A.** `margin`.

### Q16. What does `background-color` apply to?

**A.** Content + padding + border (not margin).

### Q17. What does `font-family` accept?

**A.** A prioritized fallback list.

### Q18. What is a shorthand property?

**A.** A single declaration setting multiple related properties.

### Q19. Does CSS care about whitespace?

**A.** Generally no, except in selectors and values.

### Q20. Is CSS case-sensitive?

**A.** No (except in custom identifiers and strings).

---

## 🟡 Intermediate Level (This filters people out)

### Q21. Explain the cascade order.

**A.** Origin → importance → specificity → source order.

### Q22. Which has higher specificity: class or attribute?

**A.** Equal.

### Q23. Specificity of `#id .a b.c`?

**A.** (1,2,1).

### Q24. Does inline style beat `!important`?

**A.** No, `!important` still wins.

### Q25. What creates a containing block?

**A.** Positioning, transforms, filters, viewport.

### Q26. What is `position: relative` actually for?

**A.** Establishing a positioning context.

### Q27. What happens to absolutely positioned elements?

**A.** Removed from normal flow.

### Q28. Why doesn’t `z-index` work sometimes?

**A.** No positioning or stacking context isolation.

### Q29. When does `position: sticky` fail?

**A.** Parent overflow or scroll boundary exceeded.

### Q30. Difference between Flexbox and Grid?

**A.** 1D vs 2D layout models.

### Q31. What defines the main axis in Flexbox?

**A.** `flex-direction`.

### Q32. Default `flex-direction`?

**A.** `row`.

### Q33. What is flex-basis?

**A.** Initial main-size of a flex item.

### Q34. What does `min-width: auto` mean in flex items?

**A.** Content-based minimum size.

### Q35. What causes overflow in flex containers?

**A.** Long content + min-width constraints.

### Q36. What is `@media`?

**A.** Conditional rule based on environment.

### Q37. Do media queries affect specificity?

**A.** No.

### Q38. What are pseudo-classes?

**A.** State-based selectors.

### Q39. What are pseudo-elements?

**A.** Sub-parts of elements.

### Q40. Can pseudo-elements exist without content?

**A.** No, unless `content` is defined.

### Q41. Difference between `opacity` and `rgba()`?

**A.** Opacity affects entire element subtree.

### Q42. What is `overflow: auto`?

**A.** Scrollbars only when needed.

### Q43. What is a replaced element?

**A.** Element whose rendering is external (e.g., `img`).

### Q44. Does `height: 100%` always work?

**A.** No, parent must have explicit height.

### Q45. What is the initial containing block?

**A.** The viewport.

---

## 🔴 Hardcore Level (Senior / Browser-level understanding)

### Q46. What creates a stacking context?

**A.** z-indexed positioned elements, transforms, opacity < 1, filters, etc.

### Q47. Can stacking contexts be nested?

**A.** Yes.

### Q48. Can z-index escape a stacking context?

**A.** Never.

### Q49. What is reflow?

**A.** Layout recalculation.

### Q50. What is repaint?

**A.** Pixel redraw without layout.

### Q51. Which is more expensive: reflow or repaint?

**A.** Reflow.

### Q52. What triggers layout thrashing?

**A.** Interleaving layout reads and writes.

### Q53. Why animate transform instead of left?

**A.** Avoids layout and paint.

### Q54. What thread handles compositing?

**A.** The compositor thread.

### Q55. What is `will-change`?

**A.** A performance hint to the browser.

### Q56. Why is `will-change` dangerous?

**A.** It can increase memory usage.

### Q57. Difference between implicit and explicit grid?

**A.** Auto-created vs developer-defined tracks.

### Q58. What is grid auto-placement?

**A.** Algorithm that places items automatically.

### Q59. What does `subgrid` solve?

**A.** Nested alignment problems.

### Q60. What is layout containment?

**A.** Isolating layout effects for performance.

### Q61. What CSS properties are composite-only?

**A.** `transform`, `opacity`.

### Q62. What is paint containment?

**A.** Preventing paint from leaking outside bounds.

### Q63. Why is `box-shadow` expensive?

**A.** Large blur regions require repainting.

### Q64. What is `content-visibility`?

**A.** Skips rendering off-screen content.

### Q65. What is FOIT?

**A.** Flash of Invisible Text.

### Q66. What is FOUT?

**A.** Flash of Unstyled Text.

### Q67. How does CSS affect LCP?

**A.** Render-blocking styles delay paint.

### Q68. What is critical CSS?

**A.** Minimal CSS for initial render.

### Q69. What is a formatting context?

**A.** A layout environment (block, flex, grid).

### Q70. What is BFC?

**A.** Block Formatting Context.

### Q71. What creates a BFC?

**A.** Overflow hidden, floats, absolute positioning.

### Q72. Why are BFCs useful?

**A.** Isolate floats and margins.

### Q73. What is logical properties?

**A.** Writing-mode–aware CSS properties.

### Q74. What is `:has()`?

**A.** A relational pseudo-class selector.

### Q75. Why was `:has()` delayed for years?

**A.** Performance concerns.

### Q76. What is selector matching direction?

**A.** Right-to-left.

### Q77. Why are deeply nested selectors bad?

**A.** Slow matching, brittle coupling.

### Q78. What is style recalculation?

**A.** Resolving computed styles.

### Q79. What is layout containment vs isolation?

**A.** Preventing layout influence across boundaries.

### Q80. What is the cost of `filter: blur()`?

**A.** Heavy GPU and paint cost.

---

## ☠️ Final Boss Round (If you survive this, you’re senior)

### Q81. Why does `height: 100vh` break on mobile?

**A.** Dynamic browser UI changes viewport height.

### Q82. Solution for mobile viewport bugs?

**A.** Dynamic viewport units (`dvh`).

### Q83. What is scroll-driven animation?

**A.** Animations tied to scroll position.

### Q84. Why is `overflow: hidden` dangerous?

**A.** It creates clipping and new formatting contexts.

### Q85. What is CSS containment used for?

**A.** Performance isolation.

### Q86. Why avoid global resets?

**A.** They break defaults and accessibility.

### Q87. What is `prefers-reduced-motion`?

**A.** Accessibility media query.

### Q88. What is `forced-colors`?

**A.** High-contrast accessibility mode.

### Q89. Difference between `auto-fill` and `auto-fit`?

**A.** Track collapsing behavior.

### Q90. What is intrinsic sizing?

**A.** Size based on content.

### Q91. What is `min-content`?

**A.** Smallest unbreakable size.

### Q92. What is `max-content`?

**A.** Largest content-based size.

### Q93. What is `clamp()`?

**A.** Bounded fluid value function.

### Q94. Why is CSS hard?

**A.** Multiple interacting algorithms, not syntax.

### Q95. What separates juniors from seniors in CSS?

**A.** Understanding rendering and performance.

### Q96. Can CSS be deterministic?

**A.** Yes, once the cascade is understood.

### Q97. What breaks CSS at scale?

**A.** Global scope and poor architecture.

### Q98. What is CSS architecture?

**A.** Organizing styles for scalability.

### Q99. Name a CSS architecture pattern.

**A.** BEM, ITCSS, OOCSS.

### Q100. One sentence: what is mastery in CSS?

**A.** Predicting browser behavior before writing code.

### Q101. Bonus: what do most devs misunderstand?

**A.** The cascade and rendering cost.
