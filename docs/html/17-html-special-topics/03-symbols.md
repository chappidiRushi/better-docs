---
sidebar_position: 3
---

# 17.3 HTML Symbols

## Definitions

- **HTML Symbol**: A character that is not present on a standard computer keyboard (like mathematical symbols, Greek letters, or currency signs).
- **Unicode**: An international encoding standard by which each letter, digit, or symbol is assigned a unique numeric value that applies across different platforms and programs.

## Beginner Level Introduction

Many mathematical, technical, and currency symbols are not present on a normal keyboard. 

To display these symbols on an HTML page, you can use the same technique used for HTML Entities: the **Entity Name** or the **Entity Number** (decimal or hexadecimal).

For example, to display the Euro sign (€), you can use:
- Entity Name: `&euro;`
- Decimal Number: `&#8364;`
- Hexadecimal Number: `&#x20AC;`

## Deep Dive

### How Symbols Work

Computers only understand numbers. When you type an "A", your computer actually stores the number 65 (in the ASCII standard). 

The modern standard for character encoding is **UTF-8**, which covers almost all of the characters and symbols in the world. 
When an HTML page has `<meta charset="UTF-8">` defined in its `<head>`, the browser knows how to translate the numbers (entity numbers) into the correct visual symbols.

### Categories of Symbols

#### Mathematical Symbols
| Result | Description | Entity Name | Entity Number |
|---|---|---|---|
| `∀` | For all | `&forall;` | `&#8704;` |
| `∂` | Partial differential | `&part;` | `&#8706;` |
| `∑` | Sum | `&sum;` | `&#8721;` |
| `√` | Square root | `&radic;` | `&#8730;` |
| `∞` | Infinity | `&infin;` | `&#8734;` |
| `≠` | Not equal to | `&ne;` | `&#8800;` |

#### Currency Symbols
| Result | Description | Entity Name | Entity Number |
|---|---|---|---|
| `¢` | Cent | `&cent;` | `&#162;` |
| `£` | Pound | `&pound;` | `&#163;` |
| `¥` | Yen | `&yen;` | `&#165;` |
| `€` | Euro | `&euro;` | `&#8364;` |

#### Greek Letters
| Result | Description | Entity Name | Entity Number |
|---|---|---|---|
| `Α` | Alpha | `&Alpha;` | `&#913;` |
| `Β` | Beta | `&Beta;` | `&#914;` |
| `Γ` | Gamma | `&Gamma;` | `&#915;` |
| `Δ` | Delta | `&Delta;` | `&#916;` |
| `Ω` | Omega | `&Omega;` | `&#937;` |

#### Other Common Symbols
| Result | Description | Entity Name | Entity Number |
|---|---|---|---|
| `©` | Copyright | `&copy;` | `&#169;` |
| `®` | Registered trademark | `&reg;` | `&#174;` |
| `™` | Trademark | `&trade;` | `&#8482;` |

## Examples

<details>
<summary><strong>Example: Using Symbols in Text</strong></summary>

```html
<p>The price of the item is &euro;50.</p>

<p>I will love you to &infin; and beyond.</p>

<p>The formula for the area of a circle is &pi;r&sup2;.</p>

<footer>
  <p>&copy; 2024 Tech Innovators Inc. All rights reserved.</p>
</footer>
```

</details>
