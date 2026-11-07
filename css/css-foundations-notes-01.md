# CSS Foundations --- Reference Notes 01

> **Topics:** CSS colors, CSS units, the box model, gradients, and
> backgrounds\
> **Purpose:** A practical reference for understanding what each
> property/value does while designing websites.
>
> **Scope note:** CSS accepts many values that cannot be exhaustively
> listed: colors can be written in several notations, lengths can use
> calculations and custom properties, and images can be URLs or
> generated images. This handbook covers the standard value types, major
> keywords, common functions, and the values you are most likely to use.
> It distinguishes properties from values and explains the visual result
> of each.

------------------------------------------------------------------------

# 1. CSS Colors

CSS colors control the color of text, backgrounds, borders, outlines,
shadows, SVG-related content, and other painted parts of an element.

## 1.1 Where colors can be used

  -----------------------------------------------------------------------
  Property                            What it colors
  ----------------------------------- -----------------------------------
  `color`                             Text and usually
                                      `currentColor`-based content

  `background-color`                  The element's background

  `border-color`                      The element's border

  `outline-color`                     The outline drawn outside the
                                      border

  `text-decoration-color`             Underlines, overlines, and
                                      line-through decorations

  `text-emphasis-color`               Text emphasis marks

  `caret-color`                       The text insertion caret in
                                      editable controls

  `column-rule-color`                 The rule between CSS columns

  `box-shadow`                        The shadow color

  `text-shadow`                       The text shadow color

  `fill`                              The interior paint of SVG shapes

  `stroke`                            The outline paint of SVG shapes
  -----------------------------------------------------------------------

A color value can also be used inside gradients, shadows, and other CSS
functions.

## 1.2 Named colors

CSS has a set of predefined named colors, such as:

``` css
color: red;
color: blue;
color: tomato;
color: rebeccapurple;
color: cornflowerblue;
color: transparent;
```

-   A named color is a readable keyword representing a predefined color.
-   `transparent` means fully transparent black in color-value terms. It
    is commonly used when a color should contribute no visible paint.
-   `currentColor` is a special keyword, not a fixed color name. It
    takes the computed value of the element's `color` property.

``` css
.card {
  color: #243047;
  border: 1px solid currentColor;
}
```

Here, the border uses the same color as the text.

## 1.3 Hexadecimal colors

Hex colors use a `#` followed by hexadecimal digits.

  Syntax        Meaning                            Example
  ------------- ---------------------------------- --------------------
  `#RGB`        Short RGB; each digit is doubled   `#f00` = `#ff0000`
  `#RRGGBB`     Red, green, blue channels          `#ff0000` = red
  `#RGBA`       Short RGB plus alpha               `#f008`
  `#RRGGBBAA`   RGB plus alpha                     `#ff000080`

The alpha channel controls opacity: - `00` = fully transparent - `80` =
approximately 50% opaque - `FF` = fully opaque

``` css
.panel {
  color: #172033;
  background-color: #f4f7fb;
  border-color: #dbe3ef;
}
```

## 1.4 RGB and RGBA

RGB represents a color using red, green, and blue channels.

Modern syntax:

``` css
color: rgb(255 0 0);
color: rgb(255 0 0 / 50%);
```

Comma syntax is also supported:

``` css
color: rgb(255, 0, 0);
color: rgba(255, 0, 0, 0.5);
```

  -----------------------------------------------------------------------
  Part                    Accepted form           Result
  ----------------------- ----------------------- -----------------------
  Red                     Number from `0` to      Controls red intensity
                          `255`, or percentage    

  Green                   Number from `0` to      Controls green
                          `255`, or percentage    intensity

  Blue                    Number from `0` to      Controls blue intensity
                          `255`, or percentage    

  Alpha                   Number from `0` to `1`, Controls transparency
                          or percentage           
  -----------------------------------------------------------------------

Examples:

``` css
color: rgb(0 0 0);          /* black */
color: rgb(255 255 255);    /* white */
color: rgb(255 0 0);        /* red */
color: rgb(0 128 0);        /* green */
color: rgb(0 0 255);        /* blue */
color: rgb(20 30 40 / 0.7); /* translucent color */
```

## 1.5 HSL and HSLA

HSL describes color using hue, saturation, and lightness.

``` css
color: hsl(220 70% 50%);
color: hsl(220 70% 50% / 0.6);
```

  Part         Meaning                              Typical range
  ------------ ------------------------------------ ------------------------
  Hue          Position on the color wheel          `0`--`360` degrees
  Saturation   Strength or vividness of the color   `0%`--`100%`
  Lightness    How light or dark the color is       `0%`--`100%`
  Alpha        Transparency                         `0`--`1` or percentage

Useful hue landmarks: - `0deg` / `360deg`: red - `60deg`: yellow -
`120deg`: green - `180deg`: cyan - `240deg`: blue - `300deg`: magenta

Examples:

``` css
color: hsl(0 100% 50%);      /* red */
color: hsl(120 100% 25%);    /* dark green */
color: hsl(240 100% 50%);    /* blue */
color: hsl(0 0% 0%);         /* black */
color: hsl(0 0% 100%);       /* white */
```

## 1.6 Other modern color functions

These are useful to recognize. You do not need to memorize every color
space immediately.

  Function    What it represents
  ----------- ----------------------------------------------------------
  `hwb()`     Hue, whiteness, blackness
  `lab()`     CIE Lab color space
  `lch()`     Lightness, chroma, hue
  `oklab()`   Perceptually oriented OKLab color space
  `oklch()`   Lightness, chroma, hue in OKLCH
  `color()`   A color in a specified color space, such as `display-p3`

Examples:

``` css
color: hwb(200 10% 20%);
color: oklch(65% 0.16 250);
color: color(display-p3 1 0.2 0.1);
```

Browser support and display capability vary for some wide-gamut color
spaces. Use a sensible fallback when supporting older browsers or
devices matters.

## 1.7 Color-related keywords and functions

  -----------------------------------------------------------------------
  Value                               What it does
  ----------------------------------- -----------------------------------
  `currentColor`                      Uses the element's computed text
                                      color

  `transparent`                       Fully transparent color

  `inherit`                           Takes the parent's computed value

  `initial`                           Resets the property to its initial
                                      value

  `unset`                             Acts as `inherit` for inherited
                                      properties, otherwise `initial`

  `revert`                            Rolls the property back to the
                                      value from an earlier cascade
                                      origin

  `revert-layer`                      Rolls the property back to an
                                      earlier cascade layer

  `color-mix()`                       Mixes two colors in a chosen color
                                      space

  `light-dark()`                      Selects a light or dark color based
                                      on the used color scheme
  -----------------------------------------------------------------------

Examples:

``` css
.brand {
  color: #2563eb;
}

.brand-border {
  border-color: currentColor;
}

.muted {
  color: color-mix(in srgb, #111827 60%, white);
}
```

## 1.8 Opacity versus alpha

These are not identical:

``` css
.card {
  opacity: 0.5;
}
```

-   `opacity` affects the entire rendered element, including its text,
    children, images, and background.
-   Alpha inside a background color affects only that background color.

``` css
.card {
  background-color: rgb(0 0 0 / 50%);
  color: white;
}
```

In this example, the background is translucent but the text remains
fully opaque.

## 1.9 Practical color palette example

``` css
:root {
  --color-primary: #2563eb;
  --color-primary-hover: #1d4ed8;
  --color-text: #172033;
  --color-muted: #64748b;
  --color-background: #f8fafc;
  --color-surface: #ffffff;
  --color-border: #e2e8f0;
  --color-success: #16a34a;
  --color-warning: #d97706;
  --color-danger: #dc2626;
}

body {
  color: var(--color-text);
  background-color: var(--color-background);
}
```

------------------------------------------------------------------------

# 2. CSS Units

CSS units specify measurements such as width, height, spacing, font
size, border thickness, and animation duration.

A numeric value usually needs a unit when the property expects a length,
angle, time, or resolution. Some properties accept unitless numbers.

## 2.1 Unit categories

  -----------------------------------------------------------------------
  Category                Units                   Common uses
  ----------------------- ----------------------- -----------------------
  Absolute lengths        `px`, `cm`, `mm`, `Q`,  Borders, print layouts,
                          `in`, `pc`, `pt`        fixed measurements

  Font-relative lengths   `em`, `rem`, `ex`,      Typography and
                          `ch`, `cap`, `ic`,      component sizing
                          `lh`, `rlh`             

  Viewport-relative       `vw`, `vh`, `vi`, `vb`, Viewport-based sizing
  lengths                 `vmin`, `vmax`          

  Container-query lengths `cqw`, `cqh`, `cqi`,    Sizing relative to a
                          `cqb`, `cqmin`, `cqmax` query container

  Percentages             `%`                     Relative sizing,
                                                  positioning, spacing

  Angles                  `deg`, `grad`, `rad`,   Gradients and
                          `turn`                  transforms

  Time                    `s`, `ms`               Transitions and
                                                  animations

  Resolution              `dpi`, `dpcm`, `dppx`   Resolution media
                                                  queries and
                                                  image-related contexts
  -----------------------------------------------------------------------

## 2.2 Absolute length units

  Unit   Meaning              Conversion
  ------ -------------------- -------------------------------
  `px`   CSS pixel            Reference unit for screen CSS
  `in`   Inch                 `1in = 96px`
  `cm`   Centimeter           `1cm = 96/2.54px`
  `mm`   Millimeter           `1mm = 1/10cm`
  `Q`    Quarter-millimeter   `1Q = 1/40cm`
  `pt`   Point                `1pt = 1/72in`
  `pc`   Pica                 `1pc = 12pt`

`px` is the most common absolute CSS length. A CSS pixel is not
necessarily one physical screen pixel on a high-density display.

``` css
.box {
  width: 300px;
  border: 1px solid black;
}
```

## 2.3 Font-relative units

  -----------------------------------------------------------------------
  Unit                    Relative to             Example use
  ----------------------- ----------------------- -----------------------
  `em`                    Computed font size of   Padding that scales
                          the element (with       with text
                          property-specific       
                          inheritance behavior)   

  `rem`                   Font size of the root   Site-wide spacing and
                          element                 typography

  `ex`                    x-height of the         Specialized typography
                          relevant font           

  `ch`                    Advance measure of the  Text line width
                          `0` glyph               

  `cap`                   Cap height of the font  Typography alignment

  `ic`                    Advance measure of the  CJK typography
                          CJK water ideograph     

  `lh`                    Line height of the      Vertical rhythm
                          element                 

  `rlh`                   Line height of the root Root-relative vertical
                          element                 rhythm
  -----------------------------------------------------------------------

Examples:

``` css
html {
  font-size: 16px;
}

h1 {
  font-size: 2rem; /* 32px when root size is 16px */
}

.button {
  padding: 0.75em 1.25em; /* scales with the button's text size */
}

.article {
  max-width: 65ch; /* approximately 65 zero-glyph advances */
}
```

Important: - `em` is useful when a component's spacing should scale with
its own text. - `rem` is useful for consistent site-wide sizing. - `ch`
is useful for controlling readable text line lengths. - `lh` and `rlh`
can help create line-height-based spacing systems.

## 2.4 Viewport-relative units

Traditional viewport units:

  Unit     Meaning
  -------- --------------------------------------
  `vw`     1% of viewport width
  `vh`     1% of viewport height
  `vmin`   1% of the smaller viewport dimension
  `vmax`   1% of the larger viewport dimension

Logical viewport units:

  Unit   Meaning
  ------ ---------------------------------------
  `vi`   1% of the viewport's inline dimension
  `vb`   1% of the viewport's block dimension

Dynamic and small/large viewport variants:

  -----------------------------------------------------------------------
  Family            Width             Height            Meaning
  ----------------- ----------------- ----------------- -----------------
  Default           `vw`, `vh`        `vi`, `vb`        Default viewport
                                                        sizing

  Small             `svw`, `svh`,                       Small viewport,
                    `svi`, `svb`                        often with
                                                        browser UI
                                                        expanded

  Large             `lvw`, `lvh`,                       Large viewport,
                    `lvi`, `lvb`                        often with
                                                        browser UI
                                                        retracted

  Dynamic           `dvw`, `dvh`,                       Tracks the
                    `dvi`, `dvb`                        dynamic viewport
                                                        as browser UI
                                                        changes
  -----------------------------------------------------------------------

Examples:

``` css
.hero {
  min-height: 100vh;
}

.heading {
  font-size: clamp(2rem, 5vw, 4.5rem);
}
```

On mobile, `100vh` can behave unexpectedly as browser toolbars expand
and collapse. Consider `100dvh` for a dynamic viewport height, with a
fallback if needed:

``` css
.full-screen {
  min-height: 100vh;
  min-height: 100dvh;
}
```

## 2.5 Container query units

These units are relative to the dimensions of an eligible query
container:

  Unit      Meaning
  --------- ---------------------------------------------
  `cqw`     1% of query container width
  `cqh`     1% of query container height
  `cqi`     1% of query container inline size
  `cqb`     1% of query container block size
  `cqmin`   1% of the smaller query-container dimension
  `cqmax`   1% of the larger query-container dimension

Example:

``` css
.card-list {
  container-type: inline-size;
}

.card-title {
  font-size: 5cqi;
}
```

Use container units when a component should respond to the size of its
parent container rather than the whole browser viewport.

## 2.6 Percentages (`%`)

A percentage is relative to a reference value defined by the property.

Examples:

``` css
.child {
  width: 50%;
}

.overlay {
  left: 50%;
}

.panel {
  padding: 5%;
}
```

Important: percentage reference rules differ by property. For example,
percentage width is generally based on the containing block's width,
while percentage height often depends on the containing block having a
definite height. Always check the specific property's percentage rule.

## 2.7 Unitless numbers

Some properties accept numbers without units.

  -----------------------------------------------------------------------------
  Property                      Example                 Result
  ----------------------------- ----------------------- -----------------------
  `line-height`                 `line-height: 1.5`      Line height is 1.5
                                                        times the element's
                                                        font size

  `font-weight`                 `font-weight: 700`      Bold weight

  `opacity`                     `opacity: 0.5`          50% opacity

  `z-index`                     `z-index: 10`           Stack level

  `flex`                        `flex: 1`               Flex growth shorthand
                                                        behavior

  `scale`                       `scale: 1.1`            110% scale

  `animation-iteration-count`   `3`                     Run three times

  `order`                       `2`                     Ordering value in
                                                        flex/grid layout
  -----------------------------------------------------------------------------

A unitless `0` is accepted for many length properties:

``` css
.box {
  margin: 0;
  padding: 0;
}
```

Do not assume every number can be unitless. Follow the property's value
grammar.

## 2.8 Angle units

  Unit     Meaning
  -------- -----------------------------------
  `deg`    Degrees; full circle = `360deg`
  `grad`   Gradians; full circle = `400grad`
  `rad`    Radians; full circle = `2πrad`
  `turn`   Turns; full circle = `1turn`

Examples:

``` css
.gradient {
  background: linear-gradient(90deg, red, blue);
}

.rotated {
  transform: rotate(0.25turn);
}
```

## 2.9 Time units

  Unit   Meaning
  ------ --------------
  `s`    Seconds
  `ms`   Milliseconds
  `1s`   1000ms

``` css
.button {
  transition: background-color 200ms ease;
}
```

## 2.10 CSS math functions

These functions let you calculate or compare values.

  --------------------------------------------------------------------------
  Function                What it does            Example
  ----------------------- ----------------------- --------------------------
  `calc()`                Performs arithmetic     `calc(100% - 2rem)`

  `min()`                 Chooses the smallest    `min(90%, 1200px)`
                          value                   

  `max()`                 Chooses the largest     `max(1rem, 2vw)`
                          value                   

  `clamp()`               Clamps a value between  `clamp(1rem, 2vw, 2rem)`
                          minimum and maximum     

  `round()`               Rounds a value using a  `round(10px, 3px)`
                          chosen strategy         

  `mod()`                 Modulo/remainder with   `mod(10, 3)`
                          divisor sign behavior   

  `rem()`                 Remainder operation     `rem(10, 3)`

  `abs()`                 Absolute value          `abs(-10px)`

  `sign()`                Returns the sign of a   `sign(-2)`
                          value                   
  --------------------------------------------------------------------------

Examples:

``` css
.container {
  width: min(90%, 1200px);
}

.section {
  padding-inline: clamp(1rem, 4vw, 4rem);
}

.title {
  font-size: clamp(2rem, 5vw, 4rem);
}
```

For `calc()`, operators `+` and `-` need whitespace around them. Values
combined in a calculation must have compatible or mathematically
resolvable types.

------------------------------------------------------------------------

# 3. CSS Box Model

The CSS box model describes how an element's content, padding, border,
and margin occupy space.

## 3.1 The four parts

1.  **Content:** The actual text, image, or child content.
2.  **Padding:** Space between content and border.
3.  **Border:** The line surrounding padding and content.
4.  **Margin:** Space outside the border, separating the element from
    other elements.

``` text
+-------------------------------------------+
|                  MARGIN                   |
|  +-------------------------------------+  |
|  |               BORDER                |  |
|  |  +-------------------------------+  |  |
|  |  |            PADDING            |  |  |
|  |  |  +-------------------------+  |  |  |
|  |  |  |        CONTENT          |  |  |  |
|  |  |  +-------------------------+  |  |  |
|  |  +-------------------------------+  |  |
|  +-------------------------------------+  |
+-------------------------------------------+
```

## 3.2 `width`

Sets the preferred width of the content box by default, or the border
box when `box-sizing: border-box` is used.

Common values: - Length: `300px`, `20rem` - Percentage: `50%` -
Intrinsic keywords: `min-content`, `max-content`, `fit-content` -
`auto` - Math functions: `calc()`, `min()`, `max()`, `clamp()`

``` css
.box {
  width: 300px;
}
```

Result: The element's width is set to 300 CSS pixels, subject to the
box-sizing model and layout constraints.

## 3.3 `height`

Sets the preferred height of the content box by default, or border box
with `border-box`.

Common values: - Length: `200px`, `20rem` - Percentage: `50%` -
Intrinsic keywords: `min-content`, `max-content`, `fit-content` -
`auto` - Math functions

``` css
.box {
  height: 200px;
}
```

Result: The element gets a preferred height of 200px. Content may
overflow if it does not fit.

## 3.4 `min-width` and `max-width`

  -----------------------------------------------------------------------
  Property                            What it does
  ----------------------------------- -----------------------------------
  `min-width`                         Prevents the used width from
                                      becoming smaller than the minimum

  `max-width`                         Prevents the used width from
                                      becoming larger than the maximum
  -----------------------------------------------------------------------

Accepted value families include lengths, percentages, intrinsic sizing
keywords where permitted, `none` for `max-width`, and math functions.

``` css
.container {
  width: 90%;
  max-width: 1200px;
  min-width: 280px;
}
```

Result: The container is flexible but is constrained to stay within the
chosen minimum and maximum.

## 3.5 `min-height` and `max-height`

  -----------------------------------------------------------------------
  Property                            What it does
  ----------------------------------- -----------------------------------
  `min-height`                        Prevents the used height from
                                      becoming smaller than the minimum

  `max-height`                        Prevents the used height from
                                      becoming larger than the maximum
  -----------------------------------------------------------------------

``` css
.hero {
  min-height: 70vh;
  max-height: 900px;
}
```

`max-height: none` means there is no maximum-height constraint.

## 3.6 `padding`

Padding creates inner space between the content and the border. Padding
is inside the element's background painting area.

Properties: - `padding-top` - `padding-right` - `padding-bottom` -
`padding-left` - `padding` (shorthand) - `padding-block`,
`padding-inline` - `padding-block-start`, `padding-block-end` -
`padding-inline-start`, `padding-inline-end`

Shorthand value patterns:

  -----------------------------------------------------------------------
  Values                              Result
  ----------------------------------- -----------------------------------
  `padding: 10px`                     All four sides = 10px

  `padding: 10px 20px`                Top/bottom = 10px; left/right =
                                      20px

  `padding: 10px 20px 30px`           Top = 10px; left/right = 20px;
                                      bottom = 30px

  `padding: 10px 20px 30px 40px`      Top, right, bottom, left
                                      respectively
  -----------------------------------------------------------------------

``` css
.card {
  padding: 24px;
}

.card-compact {
  padding: 12px 16px;
}
```

Padding accepts non-negative lengths and percentages. Negative padding
is invalid.

## 3.7 `margin`

Margin creates outer space outside the border.

Properties: - `margin-top` - `margin-right` - `margin-bottom` -
`margin-left` - `margin` (shorthand) - `margin-block`, `margin-inline` -
`margin-block-start`, `margin-block-end` - `margin-inline-start`,
`margin-inline-end`

Margin shorthand uses the same one-to-four value pattern as padding.

Special values: - Lengths such as `16px` - Percentages - `auto` -
Negative lengths are allowed

``` css
.card {
  margin-bottom: 24px;
}

.container {
  width: min(90%, 1200px);
  margin-inline: auto;
}
```

Result of `margin-inline: auto`: in a typical block layout with a
constrained width and available horizontal space, the remaining inline
space is distributed around the element, centering it.

Vertical margins between block boxes can collapse in certain normal-flow
situations. Padding does not collapse.

## 3.8 `border`

A border is drawn around the padding and content.

The main border properties are:

  -----------------------------------------------------------------------
  Property                            What it controls
  ----------------------------------- -----------------------------------
  `border-width`                      Thickness

  `border-style`                      Line style

  `border-color`                      Color

  `border`                            Shorthand for width, style, and
                                      color

  `border-top`, `border-right`,       Side-specific shorthands
  `border-bottom`, `border-left`      

  `border-block`, `border-inline`     Logical border shorthands

  `border-radius`                     Rounded corners
  -----------------------------------------------------------------------

### Border width values

Common keywords: - `thin` - `medium` - `thick`

Also accepts non-negative lengths such as `1px`, `2px`, `0.1rem`.

### Border style values

  -----------------------------------------------------------------------
  Value                               Visual result
  ----------------------------------- -----------------------------------
  `none`                              No border; computed width is
                                      effectively zero for border layout

  `hidden`                            No border; also has special
                                      behavior in table border conflict
                                      resolution

  `solid`                             Single solid line

  `dashed`                            Dashed line

  `dotted`                            Dotted line

  `double`                            Two parallel lines

  `groove`                            Carved/grooved 3D appearance

  `ridge`                             Raised/ridged 3D appearance

  `inset`                             Makes the border look inset

  `outset`                            Makes the border look outset
  -----------------------------------------------------------------------

### Border shorthand

``` css
.box {
  border: 1px solid #dbe3ef;
}
```

The usual order is width, style, color. The style is important: if
omitted, the initial `none` style means no visible border.

## 3.9 `border-radius`

Rounds the corners of an element's border box.

Properties: - `border-top-left-radius` - `border-top-right-radius` -
`border-bottom-right-radius` - `border-bottom-left-radius` -
`border-radius`

Values: - One length or percentage per corner - One to four values for
the four corners - A slash can separate horizontal and vertical radii
for elliptical corners

``` css
.card {
  border-radius: 16px;
}

.pill {
  border-radius: 999px;
}

.ellipse-corners {
  border-radius: 30px / 15px;
}
```

`border-radius: 50%` often creates a circle when the element is square,
or an ellipse when it is not.

## 3.10 `box-sizing`

Controls how `width` and `height` are interpreted.

  -----------------------------------------------------------------------
  Value                               Meaning
  ----------------------------------- -----------------------------------
  `content-box`                       Width/height apply to content only;
                                      padding and border are added
                                      outside

  `border-box`                        Width/height include content,
                                      padding, and border

  `inherit`, `initial`, `unset`,      CSS-wide keywords
  `revert`, `revert-layer`            
  -----------------------------------------------------------------------

Example:

``` css
.content-box {
  box-sizing: content-box;
  width: 200px;
  padding: 20px;
  border: 5px solid;
  /* Outer width = 200 + 40 + 10 = 250px */
}

.border-box {
  box-sizing: border-box;
  width: 200px;
  padding: 20px;
  border: 5px solid;
  /* Outer width = 200px */
}
```

A common global reset:

``` css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

## 3.11 Box dimension formulas

With `content-box`:

-   Total width = content width + left/right padding + left/right
    border + left/right margin.
-   Total height = content height + top/bottom padding + top/bottom
    border + top/bottom margin.

With `border-box`:

-   Declared width includes content + left/right padding + left/right
    border.
-   Declared height includes content + top/bottom padding + top/bottom
    border.
-   Margin remains outside the declared dimensions.

## 3.12 `overflow`

Controls what happens when content does not fit an element's box.

Properties: - `overflow` - `overflow-x` - `overflow-y` -
`overflow-inline` - `overflow-block`

Values:

  -----------------------------------------------------------------------
  Value                               Result
  ----------------------------------- -----------------------------------
  `visible`                           Overflow can paint outside the box;
                                      default in common cases

  `hidden`                            Clips overflow; no user-facing
                                      scrollbars, but programmatic
                                      scrolling may still be possible

  `clip`                              Clips overflow and does not create
                                      a scroll container

  `scroll`                            Creates a scroll container and
                                      provides scrolling mechanism even
                                      if not needed

  `auto`                              Browser provides scrolling when
                                      needed
  -----------------------------------------------------------------------

``` css
.panel {
  max-height: 300px;
  overflow-y: auto;
}
```

## 3.13 `box-shadow`

Adds one or more shadows around an element's box.

Syntax:

``` css
box-shadow: offset-x offset-y blur-radius spread-radius color;
```

Optional keyword: - `inset`: draws the shadow inside the box.

``` css
.card {
  box-shadow: 0 4px 16px 0 rgb(15 23 42 / 10%);
}

.input {
  box-shadow: inset 0 1px 2px rgb(0 0 0 / 10%);
}
```

Multiple shadows are comma-separated:

``` css
.card {
  box-shadow:
    0 1px 2px rgb(0 0 0 / 5%),
    0 8px 24px rgb(0 0 0 / 8%);
}
```

## 3.14 `outline`

An outline is drawn outside the border and does not take up layout
space.

Properties: - `outline-width` - `outline-style` - `outline-color` -
`outline-offset` - `outline` shorthand

Outline styles use border-like styles: `auto`, `none`, `solid`,
`dashed`, `dotted`, `double`, `groove`, `ridge`, `inset`, `outset` as
applicable.

``` css
button:focus-visible {
  outline: 3px solid #93c5fd;
  outline-offset: 3px;
}
```

Do not remove keyboard focus indicators unless you replace them with a
clearly visible alternative.

------------------------------------------------------------------------

# 4. CSS Gradients

A gradient is a generated image that smoothly transitions between two or
more color stops. Gradients are commonly used as backgrounds and can
also be layered with other background images.

## 4.1 Gradient types

  -----------------------------------------------------------------------
  Function                            Result
  ----------------------------------- -----------------------------------
  `linear-gradient()`                 Color transition along a straight
                                      line

  `repeating-linear-gradient()`       Repeats a linear gradient pattern

  `radial-gradient()`                 Color transition radiating from a
                                      central point

  `repeating-radial-gradient()`       Repeats a radial gradient pattern

  `conic-gradient()`                  Color transition around a center
                                      point

  `repeating-conic-gradient()`        Repeats a conic gradient pattern
  -----------------------------------------------------------------------

## 4.2 Linear gradient

Syntax:

``` css
background-image: linear-gradient(direction, color-stop1, color-stop2, ...);
```

Direction can be: - An angle: `0deg`, `45deg`, `90deg`, `180deg` - A
side/corner keyword: `to top`, `to right`, `to bottom`, `to left` - A
corner: `to top right`, `to bottom left`

Examples:

``` css
.hero {
  background-image: linear-gradient(to right, #2563eb, #7c3aed);
}

.hero-diagonal {
  background-image: linear-gradient(135deg, #0f172a, #2563eb);
}

.hero-vertical {
  background-image: linear-gradient(to bottom, #ffffff, #dbeafe);
}
```

For linear gradients, `0deg` points upward and `90deg` points to the
right.

## 4.3 Color stops

A color stop is a color placed at a position along the gradient line.

``` css
background: linear-gradient(
  90deg,
  red 0%,
  orange 35%,
  yellow 70%,
  green 100%
);
```

-   Stops can use percentages or lengths.
-   A stop without a position is placed according to the gradient's
    stop-distribution rules.
-   Two positions on a stop can create a color hint/transition boundary
    behavior.
-   Hard transitions can be created by placing adjacent stops at the
    same position.

``` css
background: linear-gradient(
  90deg,
  #2563eb 0%,
  #2563eb 50%,
  #7c3aed 50%,
  #7c3aed 100%
);
```

## 4.4 Radial gradient

A radial gradient spreads outward from a central point.

Syntax:

``` css
background-image: radial-gradient(shape size at position, color-stops);
```

Shape: - `ellipse` (default) - `circle`

Size keywords: - `closest-side` - `farthest-side` - `closest-corner` -
`farthest-corner` (default sizing behavior for the default ellipse)

Position can use keywords, lengths, or percentages, similar to
background positioning.

Examples:

``` css
background: radial-gradient(circle, #60a5fa, #1e3a8a);

background: radial-gradient(
  ellipse at top right,
  #bfdbfe,
  #1d4ed8 65%,
  #0f172a
);
```

## 4.5 Conic gradient

A conic gradient transitions colors around a center point, like a color
wheel.

Syntax:

``` css
background-image: conic-gradient(from angle at position, color-stops);
```

Examples:

``` css
background: conic-gradient(
  from 0deg,
  red,
  yellow,
  lime,
  cyan,
  blue,
  magenta,
  red
);
```

A conic gradient can be useful for pie-chart-like visuals, color wheels,
and decorative rings. For actual data visualizations, use semantic
charting tools rather than relying on a decorative CSS gradient.

## 4.6 Repeating gradients

Repeating gradients repeat their color-stop pattern.

``` css
background: repeating-linear-gradient(
  45deg,
  #e2e8f0 0 10px,
  #ffffff 10px 20px
);
```

``` css
background: repeating-radial-gradient(
  circle,
  #93c5fd 0 5px,
  #1d4ed8 5px 10px
);
```

``` css
background: repeating-conic-gradient(
  #2563eb 0deg 10deg,
  #ffffff 10deg 20deg
);
```

## 4.7 Multiple gradients and overlays

Background images can be layered. The first listed image is painted on
top.

``` css
.hero {
  background-image:
    linear-gradient(rgb(15 23 42 / 65%), rgb(15 23 42 / 65%)),
    url("hero.jpg");
  background-size: cover;
  background-position: center;
}
```

The gradient acts as a dark overlay over the photograph, improving text
contrast.

## 4.8 Gradient tips

-   Gradients are images, so they use `background-image`, not
    `background-color`.
-   A gradient does not need a separate image file.
-   Add more color stops to control the transition.
-   Use alpha colors to make translucent overlays.
-   Combine a gradient with a photo by layering background images.
-   A gradient itself does not automatically create a rounded shape; use
    `border-radius` on the element.

------------------------------------------------------------------------

# 5. CSS Backgrounds

Background properties control the images and colors painted behind an
element's content, padding, and border. Backgrounds do not normally
affect document layout.

## 5.1 `background-color`

Sets the background color.

Accepted values: - Named colors: `tomato` - Hex: `#ffffff` - RGB:
`rgb(255 255 255)` - HSL: `hsl(0 0% 100%)` - Other supported CSS color
functions - `transparent` - CSS-wide keywords such as `inherit`,
`initial`, `unset`, `revert`, and `revert-layer`

``` css
body {
  background-color: #f8fafc;
}
```

## 5.2 `background-image`

Sets one or more background images.

Common values: - `none` - `url("image.jpg")` - `linear-gradient(...)` -
`radial-gradient(...)` - `conic-gradient(...)` - Repeating gradient
functions - Multiple comma-separated background layers

``` css
.hero {
  background-image: url("hero.jpg");
}

.banner {
  background-image: linear-gradient(120deg, #2563eb, #7c3aed);
}
```

The first listed background layer is closest to the user. The background
color is painted beneath the image layers.

## 5.3 `background-repeat`

Controls whether a background image repeats.

  -----------------------------------------------------------------------
  Value                               Result
  ----------------------------------- -----------------------------------
  `repeat`                            Repeats horizontally and vertically

  `repeat-x`                          Repeats horizontally

  `repeat-y`                          Repeats vertically

  `no-repeat`                         Does not repeat

  `space`                             Repeats as many times as fit,
                                      distributing space between tiles

  `round`                             Repeats and scales tiles so an
                                      integer number fits

  `repeat space`                      Explicit horizontal and vertical
                                      repeat behavior

  `no-repeat repeat`                  No horizontal repeat; vertical
                                      repeat
  -----------------------------------------------------------------------

``` css
body {
  background-image: url("pattern.png");
  background-repeat: repeat;
}

.hero {
  background-image: url("hero.jpg");
  background-repeat: no-repeat;
}
```

The two-value form specifies horizontal and vertical behavior.

## 5.4 `background-position`

Controls the starting position of a background image within its
positioning area.

Common keyword values: - `left` - `center` - `right` - `top` - `bottom`

It also accepts: - Percentages: `50% 50%` - Lengths: `20px 40px` -
Edge-offset forms such as `right 20px bottom 10px` - One, two, three, or
four-part position syntax as permitted by the grammar

Examples:

``` css
.hero {
  background-position: center;
}

.hero-top {
  background-position: center top;
}

.hero-custom {
  background-position: 70% 30%;
}

.hero-edge {
  background-position: right 20px bottom 10px;
}
```

For a typical two-value position: - First value controls horizontal
position. - Second value controls vertical position. - `center` centers
the image on that axis.

## 5.5 `background-size`

Controls the rendered size of a background image.

  -----------------------------------------------------------------------
  Value                               Result
  ----------------------------------- -----------------------------------
  `auto`                              Uses the image's natural size or
                                      intrinsic sizing behavior

  `cover`                             Scales the image to cover the
                                      positioning area; some parts may be
                                      cropped

  `contain`                           Scales the whole image to fit
                                      inside the positioning area; empty
                                      space may remain

  Length                              Sets image width, with height
                                      usually determined automatically

  Percentage                          Sizes the image relative to the
                                      background positioning area

  Two values                          Sets width and height

  `auto 100%`                         Automatic width and full
                                      positioning-area height

  `100% auto`                         Full positioning-area width and
                                      automatic height
  -----------------------------------------------------------------------

Examples:

``` css
.hero {
  background-image: url("hero.jpg");
  background-size: cover;
  background-position: center;
}

.logo-background {
  background-image: url("logo.svg");
  background-repeat: no-repeat;
  background-size: contain;
}
```

Remember: - `cover` prioritizes filling the area. - `contain`
prioritizes showing the entire image. - Neither value changes the
element's own width or height.

## 5.6 `background-attachment`

Controls whether the background image scrolls with the element or is
fixed relative to the viewport.

  -----------------------------------------------------------------------
  Value                               Result
  ----------------------------------- -----------------------------------
  `scroll`                            Background scrolls with the
                                      element's content/box behavior;
                                      default

  `fixed`                             Background is fixed relative to the
                                      viewport

  `local`                             Background scrolls with the
                                      element's scrollable contents
  -----------------------------------------------------------------------

``` css
.hero {
  background-attachment: fixed;
}
```

`fixed` can behave differently on mobile browsers and may have
performance implications. Test it on actual target devices.

## 5.7 `background-origin`

Sets the box used as the origin/positioning area for the background
image.

  Value           Result
  --------------- -----------------------------------------------------
  `border-box`    Positioning area starts at the border box
  `padding-box`   Positioning area starts at the padding box; default
  `content-box`   Positioning area is the content box

``` css
.card {
  background-origin: content-box;
}
```

This affects background image positioning and sizing reference, not the
background's clipping boundary.

## 5.8 `background-clip`

Controls how far the background painting extends.

  -----------------------------------------------------------------------
  Value                               Result
  ----------------------------------- -----------------------------------
  `border-box`                        Background paints to the outside
                                      edge of the border; default

  `padding-box`                       Background paints to the padding
                                      edge, not beneath the border

  `content-box`                       Background paints only behind the
                                      content box

  `text`                              Clips background painting to the
                                      foreground text glyphs (commonly
                                      used for gradient text with
                                      additional text-color handling)
  -----------------------------------------------------------------------

``` css
.card {
  background-clip: padding-box;
}
```

Gradient text example:

``` css
.gradient-text {
  background: linear-gradient(90deg, #2563eb, #9333ea);
  background-clip: text;
  color: transparent;
}
```

For broader browser compatibility, some projects also use
`-webkit-background-clip: text`.

## 5.9 `background-blend-mode`

Controls how background image layers blend with each other and the
background color.

Common blend modes:

  Value           General effect
  --------------- ----------------------------------------------
  `normal`        No special blending
  `multiply`      Darkens by multiplying color channels
  `screen`        Lightens
  `overlay`       Combines multiply/screen-like behavior
  `darken`        Keeps the darker channel result
  `lighten`       Keeps the lighter channel result
  `color-dodge`   Brightens based on the blend color
  `color-burn`    Darkens based on the blend color
  `hard-light`    Strong contrast blend
  `soft-light`    Softer contrast blend
  `difference`    Subtracts color channels by difference
  `exclusion`     Lower-contrast difference-like blend
  `hue`           Uses hue from the blend layer
  `saturation`    Uses saturation from the blend layer
  `color`         Uses hue and saturation from the blend layer
  `luminosity`    Uses luminosity from the blend layer

``` css
.hero {
  background-color: #1e3a8a;
  background-image: url("hero.jpg");
  background-blend-mode: multiply;
}
```

The list above contains the commonly used blend modes; CSS blending also
defines additional compositing behavior in related properties.

## 5.10 `background` shorthand

The `background` shorthand can set multiple background properties at
once.

It can include: - `background-color` - `background-image` -
`background-position` - `background-size` (after `/` in the shorthand) -
`background-repeat` - `background-origin` - `background-clip` -
`background-attachment`

Example:

``` css
.hero {
  background:
    linear-gradient(rgb(15 23 42 / 60%), rgb(15 23 42 / 60%)),
    url("hero.jpg") center / cover no-repeat;
}
```

Important: - Background layers are comma-separated. - The first image
layer is on top. - A background color is specified only once, in the
final layer. - Unspecified longhand background properties are reset to
their initial values by the shorthand. Be careful when overriding only
one background detail.

## 5.11 Multiple background layers

``` css
.card {
  background:
    radial-gradient(circle at top right, rgb(96 165 250 / 30%), transparent 40%),
    linear-gradient(135deg, #ffffff, #f1f5f9);
}
```

Each layer can have its own position, size, repeat, attachment, origin,
and clip values. Values for multiple layers are separated by commas.

## 5.12 Background versus image element

Use a CSS background when: - The image is decorative. - The image is a
visual surface behind text or content. - Cropping with `cover` is
desirable. - Multiple image/gradient layers are useful.

Use an HTML `<img>` when: - The image is meaningful content. - It needs
alternative text. - It should participate as content in the document. -
You need image-specific semantics or responsive image markup.

------------------------------------------------------------------------

# 6. Quick Website-Building Recipes

These recipes combine the topics in this note.

## 6.1 Centered page container

``` css
.container {
  width: min(90%, 1200px);
  margin-inline: auto;
}
```

## 6.2 Card

``` css
.card {
  box-sizing: border-box;
  padding: 24px;
  border: 1px solid #e2e8f0;
  border-radius: 16px;
  background-color: #ffffff;
  box-shadow: 0 8px 24px rgb(15 23 42 / 8%);
}
```

## 6.3 Full-cover hero background

``` css
.hero {
  min-height: 80vh;
  background:
    linear-gradient(rgb(15 23 42 / 65%), rgb(15 23 42 / 65%)),
    url("hero.jpg") center / cover no-repeat;
  color: white;
}
```

## 6.4 Gradient button

``` css
.button {
  padding: 12px 24px;
  border: 0;
  border-radius: 10px;
  color: white;
  background: linear-gradient(90deg, #2563eb, #7c3aed);
}
```

## 6.5 Responsive typography

``` css
.page-title {
  font-size: clamp(2rem, 5vw, 4.5rem);
  line-height: 1.1;
}
```

## 6.6 Consistent global box sizing

``` css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

------------------------------------------------------------------------

# 7. Common Mistakes to Avoid

1.  **Confusing `opacity` with alpha color.** `opacity` fades the entire
    element and its children; alpha in `background-color` only makes
    that color translucent.
2.  **Forgetting `box-sizing`.** A `width: 300px` element with padding
    and border becomes wider than 300px under `content-box`.
3.  **Using `height: 100%` without a definite parent height.** The
    percentage may not resolve as expected.
4.  **Using `100vh` blindly on mobile.** Browser UI can affect the
    visible viewport; consider `100dvh`.
5.  **Using `cover` when the entire image must remain visible.** Use
    `contain` when showing the complete image matters.
6.  **Expecting `background-image` to set an element's dimensions.**
    Backgrounds paint inside an element; they do not give it content
    dimensions.
7.  **Using padding with negative values.** Negative padding is invalid;
    negative margins are allowed.
8.  **Forgetting that background layers are ordered front to back.** The
    first listed image is painted on top.
9.  **Using a decorative background for meaningful content.** Use an
    `<img>` with appropriate alternative text when the image conveys
    information.
10. **Memorizing every unit before building anything.** Learn `px`, `%`,
    `em`, `rem`, `vw`, `vh`, `fr` (for Grid tracks), and `clamp()`
    first; look up the others when needed.

------------------------------------------------------------------------

# 8. Active Recall Checklist

Try to answer these without looking at the notes.

## Colors

-   [ ] What is the difference between HEX, RGB, and HSL?
-   [ ] What does the alpha channel control?
-   [ ] What is the difference between `opacity: 0.5` and a background
    color with 50% alpha?
-   [ ] What does `currentColor` do?

## Units

-   [ ] What is the difference between `px`, `%`, `em`, and `rem`?
-   [ ] What does `100vw` mean?
-   [ ] When would you use `clamp()`?
-   [ ] What is the difference between `vh` and `dvh`?

## Box model

-   [ ] Name the four parts of the box model.
-   [ ] What is the difference between `content-box` and `border-box`?
-   [ ] What is the difference between padding and margin?
-   [ ] What does `overflow: auto` do?

## Gradients

-   [ ] What is the difference between linear, radial, and conic
    gradients?
-   [ ] How do you change a linear gradient's direction?
-   [ ] How do you create a hard color transition?
-   [ ] How do you place a gradient overlay over a photograph?

## Backgrounds

-   [ ] What is the difference between `background-size: cover` and
    `contain`?
-   [ ] What does `background-position: center` do?
-   [ ] What is the difference between `background-origin` and
    `background-clip`?
-   [ ] How do multiple background layers work?

------------------------------------------------------------------------

# 9. Mini Practice Tasks

Build each one without copying the recipe. Look up a property only after
trying to recall it.

1.  Create a 300px-wide card with 24px padding, a 1px border, and
    rounded corners.
2.  Create a centered container that never grows beyond 1200px.
3.  Make a heading whose size scales between 32px and 72px depending on
    viewport width.
4.  Create a hero section with a background photo, a dark gradient
    overlay, and centered white text.
5.  Create a circular gradient decoration using a radial gradient.
6.  Make a button with a blue-to-purple linear gradient.
7.  Create a panel with a maximum height and vertical scrolling.
8.  Make a translucent background while keeping the text fully opaque.

------------------------------------------------------------------------

# 10. Reference and Further Reading

Use these references when you need exact syntax, browser compatibility,
or less common values:

-   MDN CSS reference:
    https://developer.mozilla.org/en-US/docs/Web/CSS/Reference
-   MDN CSS values and units:
    https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Values_and_units
-   MDN box model:
    https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Box_model
-   MDN background and borders:
    https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_backgrounds_and_borders
-   MDN CSS colors:
    https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_colors
-   MDN gradients:
    https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_images/Using_CSS_gradients

> **How to use this handbook:** Learn one section, build a tiny example,
> then add your own example and mistake to the notes. Do not try to
> memorize the entire reference in one sitting. The goal is to know what
> tool to reach for and where to look up its exact syntax.
