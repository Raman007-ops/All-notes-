# CSS Foundations --- Notes 02

**Topics:** Basic and advanced selectors, pseudo-classes,
pseudo-elements, icons, box-shadow, inheritance, display, positioning,
and Flexbox.

> This is a practical reference, not a list to memorize in one sitting.
> CSS has a large and evolving grammar; the sections below cover
> standard selectors, important properties, common values, and useful
> website-building patterns. Check MDN for browser support and exact
> edge cases.

------------------------------------------------------------------------

# 1. CSS Selectors

A selector identifies which HTML element(s) a CSS rule applies to.

``` css
selector {
  property: value;
}
```

## 1.1 Basic selectors

  ---------------------------------------------------------------------------------------
  Selector          Name              What it selects   Example
  ----------------- ----------------- ----------------- ---------------------------------
  `*`               Universal         Every element     `* { box-sizing: border-box; }`

  `p`               Type/element      Every `<p>`       `p { color: gray; }`

  `.card`           Class             Elements with     `.card { padding: 1rem; }`
                                      class `card`      

  `#header`         ID                Element with ID   `#header { height: 60px; }`
                                      `header`          

  `h1, h2`          Grouping          Elements matching `h1, h2 { line-height: 1.2; }`
                                      either selector   
  ---------------------------------------------------------------------------------------

Classes are normally preferred for reusable styling. An ID should be
unique in the document and has high specificity.

## 1.2 Combinators

Combinators describe relationships between elements.

  -----------------------------------------------------------------------
  Syntax                  Name                    What it selects
  ----------------------- ----------------------- -----------------------
  `A B`                   Descendant (space)      Any B nested anywhere
                                                  inside A

  `A > B`                 Child                   B elements that are
                                                  direct children of A

  `A + B`                 Next-sibling            The first B immediately
                                                  after A, sharing the
                                                  same parent

  `A ~ B`                 Subsequent-sibling      All later B siblings
                                                  after A, sharing the
                                                  same parent
  -----------------------------------------------------------------------

``` css
.card p { color: #64748b; }        /* Any paragraph inside .card */
.card > p { margin: 0; }           /* Direct child paragraphs only */
h2 + p { margin-top: 0; }          /* Paragraph immediately after h2 */
h2 ~ p { color: slategray; }       /* All later paragraph siblings */
```

## 1.3 Attribute selectors

  -----------------------------------------------------------------------
  Syntax                              Matches
  ----------------------------------- -----------------------------------
  `[disabled]`                        Element has the attribute

  `[type="email"]`                    Exact attribute value

  `[class~="active"]`                 Attribute contains `active` as a
                                      whitespace-separated word

  `[lang\|="en"]`                     Exact `en` or value beginning
                                      `en-`, e.g. `en-US`

  `[href^="https"]`                   Value begins with `https`

  `[href$=".pdf"]`                    Value ends with `.pdf`

  `[class*="btn"]`                    Value contains substring `btn`

  `[data-state="open" i]`             ASCII case-insensitive value match

  `[data-state="OPEN" s]`             Explicit case-sensitive matching
  -----------------------------------------------------------------------

Operators: - `=` exact match. - `~=` whitespace-separated word. - `|=`
exact value or value followed by a hyphen. - `^=` begins with. - `$=`
ends with. - `*=` contains.

``` css
input[required] { border-color: crimson; }
a[href^="https"] { text-decoration: underline; }
a[href$=".pdf"]::after { content: " (PDF)"; }
```

## 1.4 Combining selectors

``` css
button.primary { background: royalblue; } /* button AND .primary */
.card.active { border-color: blue; }       /* same element has both classes */
.card .active { color: red; }              /* .active descendant inside .card */
```

A space changes the meaning: `.card.active` and `.card .active` are not
the same selector.

------------------------------------------------------------------------

# 2. Advanced Selectors

## 2.1 Logical and relational selectors

  -----------------------------------------------------------------------
  Selector                What it does            Specificity
  ----------------------- ----------------------- -----------------------
  `:is(A, B)`             Matches if the element  Specificity of the most
                          matches any selector in specific argument
                          the list                

  `:where(A, B)`          Same matching behavior  Always zero
                          as `:is()`              

  `:not(A)`               Matches elements that   Specificity of the most
                          do not match the        specific argument
                          argument                

  `:has(A)`               Matches an element      Specificity of the most
                          based on a relative     specific argument
                          selector relationship,  
                          often its descendants   

  `:scope`                Represents the scoping  Pseudo-class
                          root in a selector      specificity
                          context                 
  -----------------------------------------------------------------------

``` css
:is(h1, h2, h3) { line-height: 1.2; }
:where(header, main, footer) a { color: inherit; }
button:not(.primary) { background: #e2e8f0; }
.card:has(img) { padding-top: 0; }
.form:has(input:invalid) { border-color: crimson; }
```

`:where()` is useful for low-specificity defaults. `:has()` can style a
parent or another related element based on what it contains or what is
adjacent to it.

## 2.2 Structural selectors

  -----------------------------------------------------------------------
  Selector                            What it selects
  ----------------------------------- -----------------------------------
  `:root`                             Document root; normally `<html>`

  `:empty`                            Element with no qualifying child
                                      nodes; whitespace text can mean it
                                      is not empty

  `:first-child`                      First element child of its parent

  `:last-child`                       Last element child

  `:only-child`                       Only element child

  `:first-of-type`                    First sibling of that element type

  `:last-of-type`                     Last sibling of that element type

  `:only-of-type`                     Only sibling of that element type

  `:nth-child()`                      Position among all element siblings

  `:nth-last-child()`                 Position counted from the end

  `:nth-of-type()`                    Position among siblings of the same
                                      type

  `:nth-last-of-type()`               Same-type position counted from the
                                      end
  -----------------------------------------------------------------------

### `:nth-child()` formulas

  Formula    Matches
  ---------- ------------------------------
  `odd`      1st, 3rd, 5th, 7th, ...
  `even`     2nd, 4th, 6th, 8th, ...
  `n`        Every position
  `2n`       2, 4, 6, 8, ...
  `2n + 1`   1, 3, 5, 7, ...
  `3n`       3, 6, 9, 12, ...
  `n + 3`    3rd and every later position
  `-n + 3`   First three positions
  `5`        Only the fifth position

The general pattern is `an + b`, where `n` is a non-negative integer.

``` css
li:nth-child(odd) { background: #f8fafc; }
li:nth-child(3n) { font-weight: bold; }
p:nth-of-type(2) { color: royalblue; }
li:nth-last-child(2) { color: tomato; }
```

You can filter the set before counting:

``` css
:nth-child(2 of .featured) {
  outline: 2px solid royalblue;
}
```

### `:first-child` vs `:first-of-type`

-   `p:first-child`: a paragraph that is the first element child,
    regardless of what other tag types are siblings.
-   `p:first-of-type`: the first paragraph among its siblings, even if
    another element type appears before it.

## 2.3 Other useful relational selectors

  -----------------------------------------------------------------------
  Selector                            Meaning
  ----------------------------------- -----------------------------------
  `:lang(en)`                         Matches elements based on language

  `:dir(rtl)`                         Matches elements based on text
                                      direction

  `:defined`                          Matches defined custom elements and
                                      built-in elements

  `:target`                           Element targeted by the URL
                                      fragment

  `:target-within`                    Element that is, or contains, the
                                      fragment target

  `:host`                             Shadow host in a shadow tree

  `:host(selector)`                   Shadow host matching a selector

  `:host-context(selector)`           Shadow host based on an ancestor
                                      context; check support before
                                      relying on it
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 3. CSS Pseudo-Classes

A pseudo-class selects an element based on state, interaction, position,
or relationship. It uses one colon: `:hover`.

## 3.1 Link and interaction states

  -----------------------------------------------------------------------
  Pseudo-class                        Result
  ----------------------------------- -----------------------------------
  `:link`                             Unvisited hyperlink

  `:visited`                          Visited hyperlink; styling is
                                      restricted for privacy

  `:any-link`                         Any hyperlink

  `:hover`                            Pointer is hovering over the
                                      element

  `:active`                           Element is being activated,
                                      e.g. while a mouse button is
                                      pressed

  `:focus`                            Element currently has focus

  `:focus-visible`                    Browser determines focus should be
                                      visibly indicated

  `:focus-within`                     Element itself or a descendant has
                                      focus
  -----------------------------------------------------------------------

``` css
a:link { color: #2563eb; }
a:visited { color: #7c3aed; }
a:hover { text-decoration: underline; }
button:active { transform: scale(.98); }
input:focus-visible { outline: 3px solid #93c5fd; }
.form-group:focus-within { border-color: royalblue; }
```

Traditional link order is remembered as LVHA: `:link`, `:visited`,
`:hover`, `:active`. Cascade rules still apply.

## 3.2 Form pseudo-classes

  Pseudo-class           Result
  ---------------------- ---------------------------------------------
  `:enabled`             Enabled form control
  `:disabled`            Disabled form control
  `:checked`             Checked checkbox/radio or selected option
  `:indeterminate`       Control in indeterminate state
  `:required`            Control has `required`
  `:optional`            Control is not required
  `:valid`               Value satisfies validation constraints
  `:invalid`             Value fails validation constraints
  `:user-valid`          User-interacted control with valid value
  `:user-invalid`        User-interacted control with invalid value
  `:in-range`            Range-constrained value is within range
  `:out-of-range`        Range-constrained value is outside range
  `:read-only`           Element is not user-editable
  `:read-write`          Element is user-editable
  `:placeholder-shown`   Input is currently showing placeholder text
  `:default`             Default button/option in a relevant group
  `:autofill`            Browser-autofilled control

``` css
input:required { border-left: 3px solid royalblue; }
input:invalid { border-color: crimson; }
input:disabled { background: #e2e8f0; cursor: not-allowed; }
input:checked + label { color: seagreen; }
input::placeholder { color: #94a3b8; }
```

`:invalid` can match before the user interacts with a form. Use
`:user-invalid` when errors should appear after user interaction.

## 3.3 Structural and relational pseudo-classes

Also remember: `:root`, `:empty`, `:first-child`, `:last-child`,
`:only-child`, `:first-of-type`, `:last-of-type`, `:only-of-type`,
`:nth-child()`, `:nth-last-child()`, `:nth-of-type()`,
`:nth-last-of-type()`, `:not()`, `:is()`, `:where()`, `:has()`,
`:scope`, `:lang()`, and `:dir()`.

## 3.4 Other state pseudo-classes

  Pseudo-class            Meaning
  ----------------------- ---------------------------------------
  `:fullscreen`           Element is in fullscreen mode
  `:picture-in-picture`   Element is in picture-in-picture mode
  `:modal`                Element is in a modal state
  `:popover-open`         Popover is open
  `:playing`              Media is playing
  `:paused`               Media is paused
  `:muted`                Media is muted
  `:seeking`              Media is being seeked
  `:buffering`            Media is buffering
  `:stalled`              Media has stalled

Some media-state pseudo-classes have limited support or apply only to
particular media elements. Verify browser compatibility for production
use.

------------------------------------------------------------------------

# 4. CSS Pseudo-Elements

A pseudo-element styles a specific part of an element or creates a
generated box. It normally uses two colons: `::before`.

  Pseudo-element             What it styles/creates
  -------------------------- -----------------------------------------------------
  `::before`                 Generated box before the element's content
  `::after`                  Generated box after the element's content
  `::first-letter`           First typographic letter/punctuation of a block
  `::first-line`             First formatted line of a block
  `::selection`              User-selected text
  `::placeholder`            Placeholder text in form controls
  `::marker`                 List marker box
  `::file-selector-button`   Button part of a file input
  `::backdrop`               Backdrop behind modal/fullscreen top-layer elements
  `::cue`                    WebVTT captions/subtitles
  `::part()`                 Exposed part of a shadow-DOM component
  `::slotted()`              Elements assigned to a slot in a shadow tree

## 4.1 `::before` and `::after`

``` css
.badge::before {
  content: "NEW";
  display: inline-block;
  margin-right: 8px;
  padding: 3px 6px;
  color: white;
  background: royalblue;
  border-radius: 4px;
}

.external-link::after {
  content: " ↗";
}
```

-   Usually need `content` to generate.
-   Can be positioned, sized, colored, and decorated.
-   Useful for decorative lines, badges, icons, and shapes.
-   Do not put essential information only in generated content;
    assistive-technology handling can vary.

## 4.2 Other examples

``` css
.article p::first-letter {
  font-size: 3rem;
  float: left;
}

.article p::first-line {
  font-weight: bold;
}

::selection {
  color: white;
  background: royalblue;
}

input::placeholder {
  color: #94a3b8;
}

li::marker {
  color: royalblue;
  font-size: 1.2em;
}

dialog::backdrop {
  background: rgb(15 23 42 / 60%);
  backdrop-filter: blur(4px);
}
```

`::placeholder` styles the placeholder text. `:placeholder-shown`
selects the input while its placeholder is displayed.

------------------------------------------------------------------------

# 5. CSS Specificity and Cascade

When several rules set the same property on an element, the cascade
determines which declaration wins.

## 5.1 Specificity

A useful simplified score has four columns:

`(inline styles, IDs, classes/attributes/pseudo-classes, types/pseudo-elements)`

  Selector              Specificity
  --------------------- -------------
  `p`                   `(0,0,0,1)`
  `.card`               `(0,0,1,0)`
  `#header`             `(0,1,0,0)`
  `div.card`            `(0,0,1,1)`
  `#app .card p`        `(0,1,1,1)`
  `:where(.card)`       `(0,0,0,0)`
  `:is(#main, .card)`   `(0,1,0,0)`

Rules: - IDs outrank classes. - Classes, attributes, and pseudo-classes
share a column. - Type selectors and pseudo-elements share a lower
column. - Universal selector and combinators add no specificity. -
`:where()` has zero specificity. - `:is()`, `:not()`, and `:has()` use
the most specific argument. - `!important` changes cascade priority; it
is not a specificity score.

The cascade also considers origin, importance, cascade layers,
specificity, scoping proximity, and source order. Later source order
only breaks a tie after higher-priority factors.

## 5.2 `!important`

``` css
.button {
  color: white !important;
}
```

Marks a declaration important. Avoid using it as a routine override;
understand the cascade and fix the correct rule instead.

------------------------------------------------------------------------

# 6. CSS Icons

CSS can size, align, color, and decorate icons. The actual icon artwork
can come from SVG, an icon font, a Unicode character, or a CSS-drawn
shape.

## 6.1 SVG icons --- recommended for most interfaces

``` html
<button class="icon-button" aria-label="Search">
  <svg viewBox="0 0 24 24" aria-hidden="true">
    <circle cx="10.5" cy="10.5" r="6.5"></circle>
    <path d="m15.5 15.5 5 5"></path>
  </svg>
</button>
```

``` css
.icon-button svg {
  width: 24px;
  height: 24px;
  fill: none;
  stroke: currentColor;
  stroke-width: 2;
  stroke-linecap: round;
  stroke-linejoin: round;
}
```

  SVG property/attribute   Result
  ------------------------ -----------------------------------------
  `viewBox`                Defines the internal coordinate system
  `width`, `height`        Rendered dimensions
  `fill`                   Interior paint
  `stroke`                 Outline/line paint
  `stroke-width`           Line thickness
  `stroke-linecap`         Line ends: `butt`, `round`, `square`
  `stroke-linejoin`        Corners: `miter`, `round`, `bevel`
  `currentColor`           Makes icon follow the CSS `color` value

## 6.2 Icon fonts

Icon fonts map glyphs to symbols. An icon library usually supplies the
font, CSS class, and glyph mapping.

``` css
.icon {
  font-family: "MyIconFont";
  font-style: normal;
}
```

They are easy to size and color using font properties, but need font
loading and correct glyph mapping. SVG is usually more flexible.

## 6.3 Unicode symbols and emoji

``` html
<span aria-hidden="true">★</span>
```

-   Quick for simple decorative symbols.
-   Appearance depends on operating system and font.
-   Emoji may render in color and may not match the site's visual style.
-   Do not use an unexplained symbol as the only label for an important
    action.

## 6.4 Simple CSS-drawn hamburger icon

``` html
<button class="menu-button" aria-label="Open menu">
  <span></span>
</button>
```

``` css
.menu-button {
  width: 44px;
  height: 44px;
  display: grid;
  place-items: center;
  border: 0;
  background: transparent;
}

.menu-button span,
.menu-button span::before,
.menu-button span::after {
  display: block;
  width: 22px;
  height: 2px;
  background: currentColor;
  border-radius: 2px;
}

.menu-button span {
  position: relative;
}

.menu-button span::before,
.menu-button span::after {
  content: "";
  position: absolute;
  left: 0;
}

.menu-button span::before { top: -7px; }
.menu-button span::after { top: 7px; }
```

## 6.5 Icon alignment and accessibility

``` css
.button {
  display: inline-flex;
  align-items: center;
  gap: 8px;
}

.button svg {
  width: 1.1em;
  height: 1.1em;
  flex: none;
}
```

-   Icon-only buttons need an accessible name, e.g. `aria-label`.
-   Decorative icons should generally use `aria-hidden="true"`.
-   Use actual `<button>` and `<a>` elements for interactive controls.
-   Do not communicate status by color/icon alone; provide text or
    another accessible cue.
-   Complex or meaningful SVGs need an accessible equivalent.

------------------------------------------------------------------------

# 7. CSS `box-shadow`

Adds one or more shadows around an element's box. A shadow does not take
up layout space.

## 7.1 Syntax

``` css
box-shadow: [inset] offset-x offset-y [blur-radius] [spread-radius] [color];
```

  Part              What it does
  ----------------- ----------------------------------------------
  `inset`           Draws shadow inside the box
  `offset-x`        Positive moves right; negative moves left
  `offset-y`        Positive moves down; negative moves up
  `blur-radius`     Softens edge; cannot be negative
  `spread-radius`   Expands or contracts shadow; can be negative
  `color`           Shadow color
  `none`            No shadow

## 7.2 Examples

``` css
/* No shadow */
box-shadow: none;

/* Subtle */
box-shadow: 0 1px 3px rgb(0 0 0 / 10%);

/* Medium */
box-shadow: 0 4px 12px rgb(0 0 0 / 12%);

/* Large */
box-shadow: 0 12px 32px rgb(0 0 0 / 15%);

/* Inset */
box-shadow: inset 0 2px 4px rgb(0 0 0 / 10%);
```

Multiple shadows are comma-separated. The first listed shadow is painted
on top.

``` css
.card {
  box-shadow:
    0 1px 2px rgb(15 23 42 / 5%),
    0 8px 24px rgb(15 23 42 / 8%);
}
```

## 7.3 `box-shadow` vs `filter: drop-shadow()`

-   `box-shadow` follows the element's box and border radius.
-   `drop-shadow()` follows the alpha silhouette of rendered content,
    useful for transparent PNG/SVG shapes.

``` css
.logo {
  filter: drop-shadow(0 4px 6px rgb(0 0 0 / 25%));
}
```

Use consistent shadow tokens. Do not rely on shadows alone to
communicate focus or state.

------------------------------------------------------------------------

# 8. CSS Inheritance

Inheritance means some property values pass from a parent element to its
descendants when the child does not specify its own value.

## 8.1 Common inherited properties

Many text-related properties inherit: - `color` - `font-family` -
`font-size` - `font-style` - `font-weight` - `line-height` -
`letter-spacing` - `text-align` - `text-indent` - `text-transform` -
`visibility` - `cursor` - `list-style-type` - `quotes`

``` css
body {
  color: #172033;
  font-family: Arial, sans-serif;
  line-height: 1.6;
}
```

Descendant text generally inherits these values unless another rule
changes them.

## 8.2 Common non-inherited properties

These generally do not inherit automatically: - `margin` - `padding` -
`border` - `width` - `height` - `background` - `display` - `position` -
`box-shadow` - `transform` - `gap`

A child appears over its parent's background, but that does not mean the
child's own `background` property inherited.

## 8.3 CSS-wide keywords

  Keyword          Meaning
  ---------------- ------------------------------------------------------
  `inherit`        Use the parent's computed value
  `initial`        Use the property's initial value
  `unset`          Inherit if normally inherited; otherwise use initial
  `revert`         Roll back to a value from an earlier cascade origin
  `revert-layer`   Roll back to an earlier cascade layer

``` css
.child { color: inherit; }
button { font: inherit; }
.box { display: initial; }
```

For `display`, the initial value is `inline`, so `display: initial` can
make a `<div>` behave differently from its usual browser default.

## 8.4 `all` shorthand

``` css
.reset {
  all: unset;
}
```

Resets nearly all CSS properties on the element to a CSS-wide keyword.
It does not affect custom properties, `direction`, or `unicode-bidi`.
Use carefully because it can remove useful browser defaults.

## 8.5 Custom properties inherit by default

``` css
:root {
  --brand: #2563eb;
}

.button {
  background: var(--brand);
}
```

Use `@property` to register a custom property and control its syntax and
inheritance:

``` css
@property --progress {
  syntax: "<percentage>";
  inherits: false;
  initial-value: 0%;
}
```

------------------------------------------------------------------------

# 9. CSS `display`

`display` controls the kind of layout box an element generates and how
its children are laid out.

## 9.1 Common values

  -----------------------------------------------------------------------
  Value                               Result
  ----------------------------------- -----------------------------------
  `block`                             Block-level box; normally begins a
                                      new line and fills available inline
                                      space

  `inline`                            Inline box; flows with text

  `inline-block`                      Inline flow, but accepts width and
                                      height like a block box

  `none`                              Generates no box; removed from
                                      layout

  `contents`                          Element itself generates no
                                      principal box; descendants may
                                      still generate boxes

  `flow-root`                         Block box establishing a new block
                                      formatting context

  `list-item`                         Generates principal box and list
                                      marker

  `flex`                              Block-level flex container

  `inline-flex`                       Inline-level flex container

  `grid`                              Block-level grid container

  `inline-grid`                       Inline-level grid container
  -----------------------------------------------------------------------

## 9.2 Block, inline, and inline-block

### `display: block`

-   Normally starts on a new line.
-   Width defaults to available inline space.
-   Width, height, padding, and margin can be set.
-   Common default examples: `div`, `p`, `section`, headings.

### `display: inline`

-   Flows within a line.
-   Does not normally start a new line.
-   Width and height do not size a non-replaced inline box in the usual
    way.
-   Common default examples: `span`, `a`, `strong`.

### `display: inline-block`

-   Sits inline beside text/other inline-level boxes.
-   Accepts explicit width and height.
-   Useful for badges, compact buttons, and labels.

## 9.3 Hiding elements: three different methods

  ----------------------------------------------------------------------------
  CSS                    Visible?          Layout space?     Important
                                                             behavior
  ---------------------- ----------------- ----------------- -----------------
  `display: none`        No                No                Not rendered

  `visibility: hidden`   No                Yes               Hidden box keeps
                                                             its space

  `opacity: 0`           Transparent       Yes               Can still receive
                                                             focus and pointer
                                                             events
  ----------------------------------------------------------------------------

`opacity:  0` does not automatically make an element inaccessible or
non-interactive.

## 9.4 Other useful display values

``` css
.wrapper { display: flow-root; }
.flex-row { display: flex; }
.inline-flex-row { display: inline-flex; }
.grid-layout { display: grid; }
```

`flow-root` creates a block formatting context, useful for containing
floats and controlling some margin interactions.

CSS also has table display values: `table`, `inline-table`,
`table-row-group`, `table-header-group`, `table-footer-group`,
`table-row`, `table-cell`, `table-column-group`, `table-column`, and
`table-caption`. For real tabular data, use semantic HTML table
elements.

Modern multi-keyword syntax can express outer and inner display roles:
`block flex`, `inline flex`, `block grid`, `inline flow-root`.

------------------------------------------------------------------------

# 10. CSS Positioning

The `position` property controls how an element is placed in normal flow
or positioned relative to a containing block.

## 10.1 Position values

  -----------------------------------------------------------------------
  Value                   In normal flow?         Meaning / common use
  ----------------------- ----------------------- -----------------------
  `static`                Yes                     Default; offsets do not
                                                  position it

  `relative`              Yes                     Remains in flow, then
                                                  can be visually offset;
                                                  often anchors absolute
                                                  children

  `absolute`              No                      Positioned relative to
                                                  its containing block;
                                                  badges, overlays

  `fixed`                 No                      Usually positioned
                                                  relative to viewport;
                                                  floating controls

  `sticky`                Yes                     Acts relative until
                                                  scroll threshold, then
                                                  sticks within scroll
                                                  constraints
  -----------------------------------------------------------------------

## 10.2 `static`

``` css
.box {
  position: static;
}
```

-   Default for most elements.
-   Remains in normal flow.
-   `top`, `right`, `bottom`, and `left` do not move it.
-   Does not establish an absolute-positioning containing block merely
    by being static.

## 10.3 `relative`

``` css
.card {
  position: relative;
}

.badge {
  position: absolute;
  top: 0;
  right: 0;
}
```

-   Original layout space is preserved.
-   Offsets visually move the element from its normal position.
-   Commonly establishes a containing block for absolutely positioned
    descendants.
-   Can create a stacking context when `z-index` is not `auto`.

## 10.4 `absolute`

``` css
.card {
  position: relative;
}

.close {
  position: absolute;
  top: 12px;
  right: 12px;
}
```

-   Removed from normal flow.
-   Positioned against its containing block, commonly the padding box of
    the nearest ancestor that establishes an absolute-positioning
    containing block.
-   If no suitable ancestor exists, the initial containing block is
    used.

## 10.5 `fixed`

``` css
.chat-button {
  position: fixed;
  right: 24px;
  bottom: 24px;
}
```

-   Removed from normal flow.
-   Usually positioned relative to viewport.
-   Useful for floating action buttons, persistent controls, and fixed
    headers.
-   Certain ancestor properties such as transforms, filters, or
    perspective can establish a containing block for fixed descendants
    and change the expected behavior.

## 10.6 `sticky`

``` css
.header {
  position: sticky;
  top: 0;
  z-index: 100;
}
```

-   Starts in normal flow.
-   Sticks after reaching an inset threshold.
-   Remains constrained by its scroll container and containing block.
-   Needs a non-`auto` inset on the relevant axis, such as `top: 0`.
-   Ancestor overflow/scroll behavior can affect the relevant scroll
    container.
-   It may not visibly stick if the available space or containing block
    constraints do not allow it.

## 10.7 Offset properties

  Property                   Purpose
  -------------------------- ----------------------------------------
  `top`                      Top inset/offset
  `right`                    Right inset/offset
  `bottom`                   Bottom inset/offset
  `left`                     Left inset/offset
  `inset`                    Shorthand for top, right, bottom, left
  `inset-block`              Logical block-axis inset shorthand
  `inset-inline`             Logical inline-axis inset shorthand
  `inset-block-start/end`    Logical block start/end offsets
  `inset-inline-start/end`   Logical inline start/end offsets

Values include lengths, percentages, and `auto`.

``` css
.box {
  position: absolute;
  inset: 10px 20px auto auto;
}
```

## 10.8 `z-index` and stacking contexts

``` css
.modal {
  position: fixed;
  z-index: 1000;
}
```

-   `z-index` accepts `auto` or an integer (positive, zero, or
    negative).
-   Higher stack levels generally appear in front within the same
    stacking context.
-   Flex and grid items can use `z-index` without being positioned.
-   A child cannot escape its ancestor's stacking context just by using
    a huge number.

Common stacking-context creators include the root element, positioned
elements with non-auto `z-index`, fixed/sticky elements, flex/grid items
with non-auto `z-index`, `opacity` below `1`, transforms, filters,
`isolation: isolate`, and certain containment/will-change settings.

------------------------------------------------------------------------

# 11. CSS Flexbox

Flexbox is a one-dimensional layout system. It arranges direct children
along a main axis and a cross axis. It is ideal for navbars, toolbars,
aligned controls, card rows, and component layouts.

## 11.1 Enable Flexbox

``` css
.container {
  display: flex;
}
```

The container becomes a flex container; its direct children become flex
items.

## 11.2 Main axis and cross axis

  -----------------------------------------------------------------------
  `flex-direction`        Main axis               Cross axis
  ----------------------- ----------------------- -----------------------
  `row`                   Inline direction        Block direction
                          (commonly horizontal)   (commonly vertical)

  `row-reverse`           Reversed inline         Block direction
                          direction               

  `column`                Block direction         Inline direction
                          (commonly vertical)     (commonly horizontal)

  `column-reverse`        Reversed block          Inline direction
                          direction               
  -----------------------------------------------------------------------

The physical directions depend on writing mode and direction.

## 11.3 `flex-direction`

  Value              Result
  ------------------ -------------------------
  `row`              Items in a row; default
  `row-reverse`      Reversed row
  `column`           Items in a column
  `column-reverse`   Reversed column

``` css
.container {
  display: flex;
  flex-direction: column;
}
```

## 11.4 `justify-content`

Distributes items along the main axis.

  -----------------------------------------------------------------------
  Value                               Result
  ----------------------------------- -----------------------------------
  `flex-start`                        Group items at main-start

  `flex-end`                          Group items at main-end

  `center`                            Group items at center

  `space-between`                     Equal gaps between items; no extra
                                      outer gap

  `space-around`                      Equal space around each item; outer
                                      spaces are half the between-item
                                      spaces

  `space-evenly`                      Equal gaps between items and at
                                      outer edges

  `start`                             Logical start

  `end`                               Logical end

  `left` / `right`                    Physical horizontal alignment where
                                      applicable

  `normal`                            Normal alignment behavior
  -----------------------------------------------------------------------

``` css
.navbar {
  display: flex;
  justify-content: space-between;
}
```

## 11.5 `align-items`

Aligns items on the cross axis.

  Value                       Result
  --------------------------- -----------------------------------------------
  `stretch`                   Stretches items with auto cross-size; default
  `flex-start`                Cross-start
  `flex-end`                  Cross-end
  `center`                    Cross-axis center
  `baseline`                  Aligns text baselines
  `first baseline`            First baseline alignment
  `last baseline`             Last baseline alignment
  `start` / `end`             Logical cross-start/end
  `self-start` / `self-end`   Item's own start/end
  `normal`                    Normal alignment behavior

``` css
.navbar {
  display: flex;
  align-items: center;
}
```

## 11.6 `align-self`

Overrides `align-items` for one flex item. Values include: `auto`,
`normal`, `stretch`, `center`, `start`, `end`, `self-start`, `self-end`,
`flex-start`, `flex-end`, `baseline`, `first baseline`, and
`last baseline`.

``` css
.special-item {
  align-self: flex-end;
}
```

## 11.7 `flex-wrap`

  Value            Result
  ---------------- -------------------------------------------
  `nowrap`         All items stay on one flex line; default
  `wrap`           Items can wrap onto multiple lines
  `wrap-reverse`   Lines wrap in the reverse cross direction

``` css
.cards {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}
```

## 11.8 `flex-flow`

Shorthand for `flex-direction` and `flex-wrap`.

``` css
.container {
  flex-flow: row wrap;
}
```

## 11.9 `gap`, `row-gap`, `column-gap`

-   `gap`: sets row and column gaps.
-   `row-gap`: space between flex lines in a multi-line flex container.
-   `column-gap`: space between items along the row direction.

``` css
.container {
  display: flex;
  gap: 16px;
  row-gap: 24px;
  column-gap: 12px;
}
```

Unlike margins, gaps do not add space before the first or after the last
item.

## 11.10 `flex-grow`

Controls how much an item grows when positive free space is available.

-   Initial value: `0`.
-   `0`: does not grow to absorb free space.
-   Positive unitless number: participates in free-space distribution.
-   Negative values are invalid.

``` css
.item { flex-grow: 1; }
```

## 11.11 `flex-shrink`

Controls how much an item shrinks when there is insufficient space.

-   Initial value: `1`.
-   `0`: do not shrink.
-   Positive unitless number: relative shrink factor, weighted by flex
    base size.
-   Negative values are invalid.

``` css
.item { flex-shrink: 1; }
```

## 11.12 `flex-basis`

Sets the initial main size before free space is distributed.

Common values: - `auto` - `content` - Lengths: `200px`, `20rem` -
Percentages - Intrinsic sizing keywords where allowed: `min-content`,
`max-content`, `fit-content` - Math functions: `calc()`, `min()`,
`max()`, `clamp()`

``` css
.sidebar { flex-basis: 260px; }
.content { flex-basis: 0; }
```

## 11.13 `flex` shorthand

Shorthand for `flex-grow`, `flex-shrink`, and `flex-basis`.

  Value               Practical meaning
  ------------------- ---------------------------------------
  `flex: 0 1 auto`    Initial behavior
  `flex: 1`           Commonly expands as `1 1 0%`
  `flex: auto`        `1 1 auto`
  `flex: none`        `0 0 auto`; neither grows nor shrinks
  `flex: 0 0 250px`   250px basis; no grow or shrink

``` css
.main { flex: 1; }
.sidebar { flex: 0 0 260px; }
```

The one-number shorthand commonly uses a `0%` basis, which can differ
from an explicit `0` in some indefinite-size situations.

## 11.14 `order`

Changes visual order of flex items.

``` css
.featured { order: -1; }
```

-   Default is `0`.
-   Lower values appear earlier.
-   Equal values retain source order.
-   Does not change DOM order, screen-reader reading order, or keyboard
    navigation order. Do not use it to create confusing content order.

## 11.15 `align-content`

Aligns multiple flex lines on the cross axis when extra cross-axis space
is available. It does not align individual items on a single line.

Values include: `normal`, `flex-start`, `flex-end`, `center`,
`space-between`, `space-around`, `space-evenly`, `stretch`, `start`,
`end`, `baseline`, `first baseline`, and `last baseline`.

``` css
.container {
  display: flex;
  flex-wrap: wrap;
  align-content: space-between;
  min-height: 400px;
}
```

If there is only one flex line, `align-content` usually has no visible
effect.

## 11.16 Flexbox alignment memory aid

  -----------------------------------------------------------------------
  Property                            What it controls
  ----------------------------------- -----------------------------------
  `justify-content`                   Distribution along main axis

  `align-items`                       Alignment of items along cross axis

  `align-self`                        Cross-axis alignment of one item

  `align-content`                     Distribution of multiple flex lines
                                      along cross axis

  `gap`                               Space between items/lines
  -----------------------------------------------------------------------

For Flexbox, use these direct properties. `justify-items` does not
position flex items along the main axis.

------------------------------------------------------------------------

# 12. Practical Flexbox Patterns

## 12.1 Center content horizontally and vertically

``` css
.center {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}
```

## 12.2 Navbar

``` css
.navbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 24px;
}
```

## 12.3 Sidebar and main content

``` css
.layout {
  display: flex;
  min-height: 100vh;
}

.sidebar {
  flex: 0 0 260px;
}

.main-content {
  flex: 1;
  min-width: 0;
}
```

`min-width: 0` is often important because flex items have an automatic
minimum size that can otherwise prevent the main content from shrinking.

## 12.4 Responsive card row

``` css
.cards {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.card {
  flex: 1 1 240px;
}
```

## 12.5 Push one item to the far end

``` css
.navbar {
  display: flex;
  align-items: center;
}

.navbar .account {
  margin-left: auto;
}
```

## 12.6 Vertical stack

``` css
.stack {
  display: flex;
  flex-direction: column;
  gap: 16px;
}
```

------------------------------------------------------------------------

# 13. Quick Comparison Tables

## 13.1 Selector vs pseudo-class vs pseudo-element

  Type                 Syntax         Purpose
  -------------------- -------------- ------------------------------
  Basic selector       `.card`        Selects an element by class
  Combinator           `.card > p`    Selects by relationship
  Attribute selector   `[disabled]`   Selects by attribute
  Pseudo-class         `:hover`       Selects state or condition
  Pseudo-element       `::before`     Styles or creates a part/box

## 13.2 Position comparison

  Position     Normal flow?   Offsets?   Typical use
  ------------ -------------- ---------- ------------------------------
  `static`     Yes            No         Default layout
  `relative`   Yes            Yes        Anchor for absolute children
  `absolute`   No             Yes        Badges, overlays
  `fixed`      No             Yes        Floating action button
  `sticky`     Yes            Yes        Sticky header

## 13.3 Display comparison

  -----------------------------------------------------------------------
  Display                 New line in normal      Width/height usable?
                          flow?                   
  ----------------------- ----------------------- -----------------------
  `block`                 Yes                     Yes

  `inline`                No                      Not in the usual way

  `inline-block`          No                      Yes

  `flex`                  Block-level flex        Direct children become
                          container               flex items

  `inline-flex`           Inline-level flex       Direct children become
                          container               flex items

  `grid`                  Block-level grid        Direct children become
                          container               grid items

  `inline-grid`           Inline-level grid       Direct children become
                          container               grid items

  `none`                  No box                  Not rendered
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 14. Common Mistakes

1.  Confusing `.card .title` with `.card.title`. A space means
    descendant; no space means the same element has both classes.
2.  Confusing `:first-child` with `:first-of-type`.
3.  Using `::before` or `::after` without a `content` value.
4.  Putting essential information only in generated pseudo-element
    content.
5.  Using `!important` to fix every override instead of understanding
    the cascade.
6.  Assuming every property inherits. `color` often inherits; `margin`,
    `padding`, `border`, and `display` do not.
7.  Thinking `display: none` is the same as `opacity: 0`.
8.  Using `position: absolute` without understanding its containing
    block.
9.  Assuming `z-index: 999999` beats every other element. Stacking
    contexts isolate descendants.
10. Expecting sticky positioning to work without an inset such as
    `top: 0`.
11. Confusing `justify-content` and `align-items`; identify the main and
    cross axes first.
12. Using `align-content` to center items on a single flex line; use
    `align-items`.
13. Forgetting Flexbox affects direct children, not every nested
    descendant.
14. Using `order` to repair semantic or keyboard order; it changes
    visual order only.
15. Forgetting `min-width: 0` when long content prevents a flex child
    from shrinking.

------------------------------------------------------------------------

# 15. Active Recall Checklist

## Selectors

-   [ ] Difference between class and ID selectors?
-   [ ] Difference between descendant and child combinators?
-   [ ] What do `+` and `~` select?
-   [ ] What does `[href^="https"]` match?
-   [ ] Difference between `.card .active` and `.card.active`?

## Advanced selectors and pseudo-selectors

-   [ ] Difference between `:is()` and `:where()`?
-   [ ] What does `:has()` allow you to select?
-   [ ] Difference between `:nth-child()` and `:nth-of-type()`?
-   [ ] Difference between pseudo-class and pseudo-element?
-   [ ] What do `::before` and `::after` need to generate content?

## Icons and shadows

-   [ ] Why is SVG useful for interface icons?
-   [ ] How can an SVG icon follow text color?
-   [ ] What does `inset` do in `box-shadow`?
-   [ ] How do you create multiple shadows?

## Inheritance and display

-   [ ] Which common text properties inherit?
-   [ ] Difference between `inherit`, `initial`, and `unset`?
-   [ ] Difference between `block`, `inline`, and `inline-block`?
-   [ ] Difference between `display: none` and `visibility: hidden`?

## Position

-   [ ] Which position values remain in normal flow?
-   [ ] What establishes a containing block for an absolute element?
-   [ ] Why might sticky positioning fail?
-   [ ] Why can a high `z-index` still appear behind another element?

## Flexbox

-   [ ] What are the main and cross axes?
-   [ ] What changes when `flex-direction` becomes `column`?
-   [ ] Difference between `justify-content` and `align-items`?
-   [ ] What do `flex-grow`, `flex-shrink`, and `flex-basis` control?
-   [ ] When should you use `align-content`?

------------------------------------------------------------------------

# 16. Mini Practice Tasks

Try each task before looking at the examples above.

1.  Select only direct child buttons inside `.toolbar`.
2.  Give every second table row a different background.
3.  Style an input differently when it is focused.
4.  Add a `NEW` badge before a title using `::before`.
5.  Style a card only when it contains an image using `:has()`.
6.  Build an icon-only search button using inline SVG and an accessible
    name.
7.  Create a card with a subtle shadow and a stronger hover shadow.
8.  Make buttons inherit the page font.
9.  Compare `display: none` and `visibility: hidden`.
10. Put a close button in the top-right of a card using relative and
    absolute positioning.
11. Build a sticky header.
12. Create a fixed circular chat button at the bottom-right.
13. Build a navbar using Flexbox.
14. Center a login form with Flexbox.
15. Build a 260px sidebar and a main content area that fills remaining
    space.
16. Create a wrapping responsive card row using `flex-wrap`, `flex`, and
    `gap`.

------------------------------------------------------------------------

# 17. Reference and Further Reading

-   MDN CSS selectors:
    https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_selectors
-   MDN pseudo-classes:
    https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes
-   MDN pseudo-elements:
    https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements
-   MDN specificity:
    https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity
-   MDN display:
    https://developer.mozilla.org/en-US/docs/Web/CSS/display
-   MDN position:
    https://developer.mozilla.org/en-US/docs/Web/CSS/position
-   MDN Flexbox:
    https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout
-   MDN inheritance:
    https://developer.mozilla.org/en-US/docs/Web/CSS/inheritance
-   MDN box-shadow:
    https://developer.mozilla.org/en-US/docs/Web/CSS/box-shadow
-   MDN SVG: https://developer.mozilla.org/en-US/docs/Web/SVG

> **Revision rule:** Learn the common selectors and layout properties
> first. Understand the matching and axis logic. Use this file as a
> reference for less common values instead of trying to memorize every
> item.
