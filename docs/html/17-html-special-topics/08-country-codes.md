---
sidebar_position: 8
---

# 17.8 Country Codes

## Definitions

- **Country Code**: A standardized two-letter code used to represent a specific country or dependent area.
- **ISO 3166-1 alpha-2**: The internationally recognized standard for two-letter country codes (e.g., "US" for United States, "GB" for United Kingdom).

## Beginner Level Introduction

As we learned in the previous section on Language Codes, you can combine a language code with a country code to specify the exact regional dialect of a web page.

While the language code is always lowercase, the country code is generally written in uppercase, separated by a hyphen.

- Language Code: `en` (English)
- Country Code: `US` (United States)
- Combined: `en-US`

## Deep Dive

### When to use Country Codes

Using a country code is critical when a language varies significantly between regions.

For example, Spanish (`es`) is spoken in many countries. If your website is specifically tailored to users in Mexico, you should use `es-MX`. If it is tailored to users in Spain, use `es-ES`.

This helps search engines deliver your page to the correct local audience. If a user in Mexico searches for a term in Spanish, Google will prioritize pages with `es-MX` over pages with `es-ES`.

### Common Country Codes

| Country | Code |
|---|---|
| United States | `US` |
| United Kingdom | `GB` |
| Canada | `CA` |
| Australia | `AU` |
| France | `FR` |
| Germany | `DE` |
| Japan | `JP` |
| China | `CN` |
| India | `IN` |
| Brazil | `BR` |
| Mexico | `MX` |
| Spain | `ES` |

## Examples

<details>
<summary><strong>Example: Targeting Regional Dialects</strong></summary>

```html
<!-- English as spoken in the United Kingdom -->
<html lang="en-GB">

<!-- French as spoken in Canada -->
<html lang="fr-CA">

<!-- Portuguese as spoken in Brazil -->
<html lang="pt-BR">

<!-- Traditional Chinese as spoken in Taiwan -->
<html lang="zh-TW">
```

</details>
