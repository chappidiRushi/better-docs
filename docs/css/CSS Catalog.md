---

id: css-catalog
title: Property Catalog
sidebar_position: 2
--------------------
## CSS Property Catalog

> A categorized reference of CSS properties with examples and possible values.

---

## Fonts

````css
font-family: 'Arial', sans-serif; /* family-name | generic-family */
font-size: 16px; /* absolute-size | relative-size | length | percentage */
font-weight: 400; /* normal | bold | bolder | lighter | 100–900 */
font-style: italic; /* normal | italic | oblique */
font-variant: small-caps; /* normal | small-caps */
font-stretch: expanded; /* ultra-condensed → ultra-expanded */
font-size-adjust: 0.5; /* none | number */
font-kerning: normal; /* auto | normal | none */
font-optical-sizing: auto; /* auto | none */
font-feature-settings: 'liga' on; /* normal | feature-tag value */
font-variation-settings: 'wght' 700; /* normal | axis-tag value */
font-synthesis: weight style; /* none | weight | style */
font-language-override: normal; /* normal | string */

/* Shorthand */
font: italic small-caps bold 16px/1.5 "Arial", sans-serif;
````

---

## Text

```css
color: #333; /* color */
text-align: center; /* left | right | center | justify | start | end */
text-decoration: underline; /* none | underline | overline | line-through */
text-transform: uppercase; /* none | capitalize | uppercase | lowercase */
text-indent: 2em; /* length | percentage */
letter-spacing: 0.05em; /* normal | length */
word-spacing: 0.2em; /* normal | length */
line-height: 1.5; /* normal | number | length | percentage */
white-space: nowrap; /* normal | nowrap | pre | pre-wrap | pre-line */
text-overflow: ellipsis; /* clip | ellipsis */
vertical-align: middle; /* baseline | sub | super | top | middle | bottom */
writing-mode: vertical-rl; /* horizontal-tb | vertical-rl | vertical-lr */
direction: rtl; /* ltr | rtl */
unicode-bidi: bidi-override; /* normal | embed | bidi-override */
```

---

## Backgrounds

````css
background-color: #fff; /* color */
background-image: url(bg.png); /* none | image */
background-repeat: no-repeat; /* repeat | repeat-x | repeat-y | no-repeat */
background-position: center; /* position */
background-size: cover; /* auto | cover | contain | length */
background-attachment: fixed; /* scroll | fixed | local */
background-origin: padding-box; /* border-box | padding-box | content-box */
background-clip: content-box; /* border-box | padding-box | content-box */
background-blend-mode: multiply;

/* Shorthand */
background: url(bg.png) no-repeat center / cover fixed padding-box content-box;
````

---

## Box Model

```css
width: 100px; /* auto | length | percentage */
height: 100px; /* auto | length | percentage */
min-width: 0; /* length | percentage */
max-width: 100%; /* length | percentage */
margin: 1rem; /* length | percentage | auto */
padding: 1rem; /* length | percentage */
box-sizing: border-box; /* content-box | border-box */
```

---

## Borders & Outline

````css
border-width: 1px; /* length */
border-style: solid; /* none | solid | dashed | dotted | double */
border-color: #000; /* color */
border-radius: 8px; /* length | percentage */
outline: 2px solid red; /* outline-width style color */


/* Shorthands */
border: 1px solid black;
border-top: 1px solid red;
border-right: 1px solid red;
border-bottom: 1px solid red;
border-left: 1px solid red;
border-image: url(border.png) 30 / 10 / 0 stretch;
````

---

## Display & Visibility

```css
display: flex; /* block | inline | inline-block | flex | grid | none */
visibility: hidden; /* visible | hidden | collapse */
opacity: 0.8; /* number (0–1) */
```

---

## Positioning

```css
position: absolute; /* static | relative | absolute | fixed | sticky */
top: 0; /* length | percentage | auto */
right: 0;
bottom: 0;
left: 0;
z-index: 10; /* auto | integer */
```

---

## Flexbox

````css
display: flex;
flex-direction: row; /* row | row-reverse | column | column-reverse */
flex-wrap: wrap; /* nowrap | wrap | wrap-reverse */
justify-content: space-between; /* flex-start | center | space-between */
align-items: center; /* stretch | center | flex-start | flex-end */
align-content: space-around; /* stretch | center | space-around */
flex: 1 1 auto; /* grow shrink basis */
flex-grow: 1;
flex-shrink: 1;
flex-basis: auto;
order: 1; /* integer */

/* Shorthand */
flex-flow: row wrap;
````

---

## Grid

````css
display: grid;
grid-template-columns: 1fr 1fr; /* track-list */
grid-template-rows: auto;
grid-template-areas: "a b" "c d";
grid-column: 1 / 3; /* start / end */
grid-row: auto;
grid-auto-columns: auto;
grid-auto-rows: auto;
grid-auto-flow: row;
gap: 1rem; /* length */

/* Shorthands */
grid-template: "a b" 1fr "c d" 1fr / 1fr 1fr;
grid: auto-flow dense / 1fr 1fr;
````

---

## Overflow & Scrolling

```css
overflow: hidden; /* visible | hidden | scroll | auto */
overflow-x: auto;
overflow-y: auto;
scroll-behavior: smooth; /* auto | smooth */
scroll-snap-type: x mandatory; /* none | axis proximity/mandatory */
```

---

## Transforms

```css
transform: rotate(45deg); /* none | transform-function */
transform-origin: center; /* position */
transform-style: preserve-3d; /* flat | preserve-3d */
```

---

## Transitions

````css
transition-property: all; /* none | property */
transition-duration: 0.3s; /* time */
transition-timing-function: ease-in-out; /* timing-function */
transition-delay: 0s; /* time */

/* Shorthand */
transition: opacity 0.3s ease-in-out 0s;
````

---

## Animations

````css
animation-name: fade;
animation-duration: 1s;
animation-timing-function: ease;
animation-delay: 0s;
animation-iteration-count: infinite; /* number | infinite */
animation-direction: alternate; /* normal | reverse | alternate */
animation-fill-mode: forwards; /* none | forwards | backwards | both */
animation-play-state: running; /* running | paused */
animation-composition: add;
animation-timeline: scroll();

/* Shorthand */
animation: fade 1s ease-in-out 0s infinite alternate both running;
````

---

## Filters & Effects

```css
filter: blur(5px); /* none | filter-function */
backdrop-filter: blur(10px);
box-shadow: 0 2px 10px rgba(0,0,0,.2);
text-shadow: 1px 1px 2px black;
```

---

## Tables

```css
border-collapse: collapse; /* collapse | separate */
border-spacing: 2px;
table-layout: fixed; /* auto | fixed */
caption-side: bottom; /* top | bottom */
```

---

## Lists

````css
list-style-type: disc; /* disc | circle | square | none */
list-style-position: inside; /* inside | outside */
list-style-image: url(bullet.png);

/* Shorthand */
list-style: square inside url(icon.png);
````

---

## UI & Interaction

```css
cursor: pointer; /* auto | pointer | text | move | wait */
user-select: none; /* auto | none | text | all */
pointer-events: none; /* auto | none */
resize: both; /* none | both | horizontal | vertical */
```

---

## Miscellaneous

```css
object-fit: cover; /* fill | contain | cover | none | scale-down */
object-position: center;
clip-path: circle(50%);
mask-image: url(mask.png);
caret-color: red;
accent-color: blue;
```

---

> This catalog is intended to be **exhaustive**: it includes *all standardized CSS properties* across CSS Levels 1–4+ (layout, typography, UI, animations, scrolling, logical properties, SVG-related properties, media, containment, compositing, masking, shapes, counters, pagination, speech, and more). Vendor‑prefixed properties and fully deprecated properties are intentionally excluded unless historically relevant, as they are not part of the active CSS specifications.

---

## Layout, Sizing & Flow (Missing)

```css
aspect-ratio: 16 / 9; /* auto | ratio */
contain: layout paint size style; /* none | strict | content | flags */
container-type: inline-size; /* normal | size | inline-size */
container-name: sidebar; /* none | identifier */
float: left; /* left | right | none | inline-start | inline-end */
clear: both; /* none | left | right | both | inline-start | inline-end */
isolation: isolate; /* auto | isolate */
```

---

## Columns

````css
column-count: 3; /* auto | integer */
column-width: 200px; /* auto | length */
column-gap: 1rem; /* normal | length */
column-rule-width: 1px;
column-rule-style: solid;
column-rule-color: black;
column-span: all; /* none | all */
column-fill: balance; /* auto | balance */

/* Shorthands */
columns: 200px 3;
column-rule: 1px solid black;
````

---

## Logical Properties (Missing)

```css
margin-inline: 1rem;
margin-block: 2rem;
padding-inline-start: 1rem;
border-inline-end-width: 2px;
inset-inline-start: 0;
block-size: 100px;
inline-size: 100%;
min-block-size: 0;
max-inline-size: 100%;
```

---

## Advanced Text & Typography (Missing)

```css
text-decoration-color: red;
text-decoration-style: wavy;
text-decoration-thickness: 2px;
text-emphasis-style: dot;
text-emphasis-color: red;
text-orientation: upright; /* mixed | upright | sideways */
text-align-last: justify;
hyphens: auto; /* none | manual | auto */
line-break: strict; /* auto | loose | normal | strict */
word-break: break-word; /* normal | break-all | keep-all */
overflow-wrap: anywhere; /* normal | break-word | anywhere */
quotes: "«" "»";
```

---

## Generated Content & Counters (Missing)

```css
content: counter(section);
counter-reset: section;
counter-increment: section;
```

---

## Scrolling & Scroll Snap (Missing)

```css
scroll-snap-align: start; /* none | start | end | center */
scroll-snap-stop: always; /* normal | always */
scroll-margin: 1rem;
scroll-padding: 1rem;
overscroll-behavior: contain; /* auto | contain | none */
```

---

## Animations (Advanced / Missing)

```css
animation-composition: add; /* replace | add | accumulate */
animation-timeline: scroll(); /* auto | timeline */
```

---

## View & Media Queries Related (Missing)

```css
view-transition-name: fade;
```

---

## SVG Presentation Properties (Missing)

```css
fill: red;
fill-opacity: 0.5;
stroke: black;
stroke-width: 2;
stroke-linecap: round;
stroke-linejoin: bevel;
stroke-dasharray: 4 2;
stroke-dashoffset: 1;
vector-effect: non-scaling-stroke;
```

---

## Masking & Compositing

````css
mask-image: url(mask.png);
mask-repeat: no-repeat;
mask-position: center;
mask-size: contain;
mask-mode: luminance;
mask-origin: content-box;
mask-clip: border-box;
mask-composite: intersect;
mask-type: luminance;
mix-blend-mode: multiply; /* normal | multiply | screen | overlay */

/* Shorthand */
mask: url(mask.png) no-repeat center / contain luminance;
```css
mask-repeat: no-repeat;
mask-position: center;
mask-size: contain;
mask-composite: intersect;
mix-blend-mode: multiply; /* normal | multiply | screen | overlay */
````

---

## Shapes & Motion

````css
shape-outside: circle(50%);
shape-margin: 1rem;
shape-image-threshold: 0.5;
offset-path: path('M0,0 L100,100');
offset-distance: 50%;
offset-rotate: auto;

/* Shorthand */
offset: path('M0,0 L100,100') auto 50% auto;
````

---

## Pagination & Print (Missing)

```css
page-break-before: always;
page-break-after: avoid;
page-break-inside: auto;
orphans: 2;
widows: 2;
```

---

## Math, Ruby & East Asian (Missing)

```css
ruby-position: over;
ruby-align: center;
math-style: compact;
```

---

## Speech (Rare but Standardized)

```css
speak: auto;
voice-family: male;
pause-before: 200ms;
```

---
## Border (Expanded / Missing)

```css
border-top-width: 1px;
border-right-width: 1px;
border-bottom-width: 1px;
border-left-width: 1px;
border-top-style: solid;
border-right-style: solid;
border-bottom-style: solid;
border-left-style: solid;
border-top-color: red;
border-right-color: red;
border-bottom-color: red;
border-left-color: red;
border-image-source: url(border.png);
border-image-slice: 30;
border-image-width: 10;
border-image-outset: 0;
border-image-repeat: stretch;
```

---

## Background (Expanded / Missing)

```css
background-blend-mode: multiply; /* normal | multiply | screen | overlay */
```

---

## Flexbox (Expanded / Missing)

```css
row-gap: 1rem;
column-gap: 1rem;
flex-grow: 1;
flex-shrink: 1;
flex-basis: auto;
align-self: center; /* auto | stretch | center | flex-start | flex-end */
```

---

## Grid (Expanded / Missing)

```css
grid-auto-columns: auto;
grid-auto-rows: auto;
grid-auto-flow: row; /* row | column | dense */
grid-template-areas: "a b" "c d";
grid-area: a;
justify-items: center;
justify-self: center;
align-self: center;
place-items: center;
place-content: space-between;
place-self: center;
```

---

## Scrollbars (Standardized)

```css
scrollbar-width: thin; /* auto | thin | none */
scrollbar-color: red blue; /* thumb track */
```

---

## Images & Replaced Content (Missing)

```css
image-rendering: crisp-edges; /* auto | smooth | crisp-edges | pixelated */
image-orientation: from-image; /* from-image | none */
image-resolution: 300dpi;
```

---

## Containment & Performance (Expanded)

```css
content-visibility: auto; /* visible | auto | hidden */
contain-intrinsic-size: 300px;
```

---

## Pointer, Touch & Input (Missing)

```css
touch-action: manipulation; /* auto | none | pan-x | pan-y | manipulation */
caret-shape: block; /* auto | bar | block | underscore */
```

---

## Viewport & Sizing Units Related

```css
zoom: 1; /* non-standard but widely implemented */
```

---

## SVG Text & Layout (Additional)

```css
dominant-baseline: middle;
alignment-baseline: central;
baseline-shift: super;
```

---

## Deprecated but Still Documented (Aliases)

```css
clip: rect(0, 0, 0, 0); /* deprecated */
page-break-before: always;
page-break-after: always;
page-break-inside: avoid;
```

---
## Outline (Longhands – Missing)

```css
outline-width: 2px; /* length */
outline-style: solid; /* auto | none | solid | dotted | dashed | double */
outline-color: blue; /* color */
```

---

## Text (Additional Missing)

```css
text-decoration-line: underline; /* none | underline | overline | line-through */
text-decoration-skip-ink: auto; /* auto | none */
text-emphasis-position: over right; /* over | under + left | right */
text-emphasis: dot red; /* shorthand */
text-justify: inter-word; /* auto | inter-word | inter-character | none */
text-combine-upright: all; /* none | all | digits */
hanging-punctuation: first last; /* none | first | last | allow-end | force-end */
tab-size: 4; /* number | length */
```

---

## Writing Modes & Flow (Missing)

```css
text-size-adjust: 100%; /* percentage | none */
line-clamp: 3; /* none | integer */
```

---

## Breaks (Modern Pagination – Missing)

```css
break-before: page; /* auto | avoid | page | column | region */
break-after: avoid;
break-inside: avoid;
```

---

## Overflow & Scrolling (Longhands – Missing)

```css
overflow-inline: hidden; /* visible | hidden | scroll | auto */
overflow-block: auto;
overscroll-behavior-x: contain;
overscroll-behavior-y: none;
scroll-margin-top: 1rem;
scroll-margin-right: 1rem;
scroll-margin-bottom: 1rem;
scroll-margin-left: 1rem;
scroll-padding-top: 1rem;
scroll-padding-right: 1rem;
scroll-padding-bottom: 1rem;
scroll-padding-left: 1rem;
```

---

## Transforms (3D & Missing)

```css
perspective: 800px; /* none | length */
perspective-origin: center;
backface-visibility: hidden; /* visible | hidden */
```

---

## Will-Change & Rendering Hints (Missing)

```css
will-change: transform, opacity; /* auto | property */
```

---

## Appearance & System UI (Missing)

```css
appearance: none; /* auto | none | button | textfield */
color-scheme: light dark; /* normal | light | dark */
forced-color-adjust: none; /* auto | none */
print-color-adjust: exact; /* economy | exact */
```

---

## Lists (Shorthand – Missing)

```css
list-style: square inside url(icon.png);
```

---

## Columns (Shorthand – Missing)

```css
columns: 200px 3; /* column-width column-count */
column-rule: 1px solid black; /* shorthand */
```

---

## Masking (Remaining Standard Longhands)

```css
mask-mode: luminance; /* match-source | luminance | alpha */
mask-origin: content-box;
mask-clip: border-box;
mask-type: luminance; /* luminance | alpha */
```

---

## SVG Paint & Rendering (Missing)

```css
fill-rule: evenodd; /* nonzero | evenodd */
clip-rule: nonzero;
stop-color: red;
stop-opacity: 1;
flood-color: black;
flood-opacity: 0.5;
lighting-color: white;
color-interpolation: srgb;
color-interpolation-filters: linearRGB;
color-rendering: optimizeQuality;
shape-rendering: geometricPrecision;
text-rendering: optimizeLegibility;
paint-order: fill stroke markers;
marker-start: url(#m);
marker-mid: url(#m);
marker-end: url(#m);
```

---

## SVG Layout & Visibility (Missing)

```css
display: inline;
overflow: visible;
visibility: visible;
```

---

## Speech (Additional Standard Properties)

```css
speak-as: spell-out;
pause-after: 200ms;
pause: 200ms;
rest-before: 100ms;
rest-after: 100ms;
```

---

## Logical Shorthands (Completion Note)

```css
inset: 0; /* logical shorthand for top/right/bottom/left */
margin-inline-start: 1rem;
margin-inline-end: 1rem;
padding-block-start: 1rem;
padding-block-end: 1rem;
border-block-start-width: 1px;
border-block-end-width: 1px;
```

---
