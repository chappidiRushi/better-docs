---
sidebar_position: 4
---

# 19.4 Cheat Sheets

## Definitions

- **Cheat Sheet**: A concise set of notes used for quick reference, typically condensing a large amount of information onto a single page.

## Beginner Level Introduction

No developer memorizes every single HTML tag or CSS property. It is completely normal (and expected) for professional software engineers to Google things every single day.

Cheat sheets are incredibly useful to have open on a second monitor or printed out on your desk while you are learning. They save you from having to read through long documentation pages just to remember the exact spelling of an attribute.

## Deep Dive

### Best HTML Cheat Sheets

1. **MDN Web Docs - HTML Reference**:
   *(developer.mozilla.org/en-US/docs/Web/HTML/Element)*
   While not a traditional 1-page cheat sheet, this is the definitive, authoritative source for all HTML tags. If you need to know exactly what attributes a `<video>` tag accepts, this is where you go.

2. **OverAPI HTML Cheat Sheet**:
   *(overapi.com/html)*
   A highly interactive, dense wall of HTML tags. Clicking any tag instantly links you to detailed documentation.

3. **HTML5 Periodic Table**:
   *(joshduck.com/periodic-table.html)*
   A visually brilliant representation of HTML5 elements laid out like the periodic table of elements, grouped by category (forms, tabular data, metadata, etc.).

4. **Emmet Cheat Sheet**:
   *(docs.emmet.io/cheat-sheet/)*
   Emmet is a plugin built into VS Code that allows you to write HTML extremely fast using abbreviations (e.g., typing `ul>li*3` and pressing Tab generates a list with 3 items). This cheat sheet is essential once you want to speed up your coding.

## Examples

<details>
<summary><strong>Example: How to use the Emmet Cheat Sheet</strong></summary>

Instead of typing out every angle bracket manually, you can use Emmet abbreviations in VS Code.

If you type this Emmet abbreviation:
`nav>ul>li.nav-item*3>a`

And press the `Tab` key, it instantly generates:
```html
<nav>
  <ul>
    <li class="nav-item"><a href=""></a></li>
    <li class="nav-item"><a href=""></a></li>
    <li class="nav-item"><a href=""></a></li>
  </ul>
</nav>
```
Learning Emmet from a cheat sheet will easily double your HTML coding speed.

</details>
