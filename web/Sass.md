# Sass

## General
- **Sass** stands for **S**yntactically **A**wesome **S**tyle**S**heets
- It is a **CSS** preprocessor
- Let's us use features that do not exist in **CSS**
- **Sass** files are compiled to regular **CSS**
- Has two syntaxes:
  - The **SCSS** syntax (`.scss`) is used most commonly and is a superset of **CSS**
  - The indented syntax (`.sass`) uses indentation rather than curly braces to nest statements, and newlines instead of
    semicolons to separate them
- We can install **Sass** as a project dependency with `npm install -D sass`
- Using the `sass` command we can compile **Sass** files to **CSS**:
  ```sh
  sass input.scss output.css
  ```
- Passing the `--watch` flag will automatically compile our files (or directories) everytime they change:
  ```sh
  # watch a single file
  sass --watch scss/style.scss css/style.css

  # watch a directory
  sass --watch scss:css
  ```

## Variables
Variables offer a way for storing information that we may want to reuse throughout our stylesheet. We can store things
like colors, font stacks, etc.

In **Sass** variables are prefixed with the `$` symbol:

```scss
$primary-color: #333;

body {
  color: $primary-color;
}
```

This makes them easier to read and write than **CSS** *custom properties*:

```css
:root {
  --primary-color;
}

body {
  color: var(--primary-color);
}
```

However, we can override the values of custom properties without re-defining rules for our components, which is not the
case with **Sass** variables.

### Lists
Lists contain a sequence of other values. In **Sass**, elements in lists can be separated by commas, spaces, or slashes,
as long as it's consistent within the list:

```scss
$font: (Helvetica, Arial, sans-serif);
$margin: (10px 15px 0 0);
```

Unlike most other languages, lists in **Sass** don't require special brackets; any expressions separated with spaces or
commas count as a list. However, we're allowed to write lists with square brackets (`[line1 line2]`), which is useful
when using `grid-template-columns`.

### Maps
Maps in **Sass** hold pairs of keys and values, and make it easy to look up a value by its corresponding key:

```scss
$font-weights: ("regular": 400, "medium": 500, "bold": 700);

@debug map.get($font-weights, "medium"); // 500
@debug map.get($font-weights, "extra-bold"); // null
```

The keys must be unique, but the values may be duplicated.

Unlike [lists](#lists), maps **MUST** be written with parentheses around them. A map with no pairs is written `()`.

## Nesting
**Sass** lets us nest our **CSS** selectors in a way that follows the same visual hierarchy of our **HTML**:

<table>
<thead>
<tr>
<th>SCSS</th>
<th>CSS</th>
</tr>
<thead>
<tbody>
<tr>
<td>

```scss
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```

</td>
<td>

```css
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}
nav li {
  display: inline-block;
}
nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}
```

</td>
</tr>
</tbody>
</table>

## Parent selector
The parent selector, `&`, is a special selector invented by **Sass** that's used in [nested selectors](#nesting) to
refer to the outer selector. It makes it possible to re-use the outer selector in more complex ways, like adding a
*pseudo-class* or adding a selector *before* the parent:

<table>
<thead>
<tr>
<th>SCSS</th>
<th>CSS</th>
</tr>
<thead>
<tbody>
<tr>
<td>

```scss
.alert {
  // The parent selector can be used to add pseudo-classes to the outer selector
  &:hover {
    font-weight: bold;
  }

  // It can also be used to style the outer selector in a certain context, such
  // as a body set to use a right-to-left language.
  [dir=rtl] & {
    margin-left: 0;
    margin-right: 10px;
  }

  // We can even use it as an argument to pseudo-class selectors.
  :not(&) {
    opacity: 0.8;
  }
}
```

</td>
<td>

```css
.alert:hover {
  font-weight: bold;
}
[dir=rtl] .alert {
  margin-left: 0;
  margin-right: 10px;
}
:not(.alert) {
  opacity: 0.8;
}
```

</td>
</tr>
</tbody>
</table>

## Mixins
Some things in **CSS** are a bit tedious to write, especially with **CSS3** and the many vendor prefixes that exist. A
*mixin* lets us make groups of **CSS** declarations that we want to reuse throughout our site. It helps keep our
**Sass** very `DRY`. We can even pass in values to make our mixin more flexible.

To create a mixin we can use the `@mixin` directive and give it a name and (optionally) parameters. We can then use it
as a **CSS** declaration starting with `@include` followed by the name of the mixin:

<table>
<thead>
<tr>
<th>SCSS</th>
<th>CSS</th>
</tr>
<thead>
<tbody>
<tr>
<td>

```scss
@mixin transform($property) {
  -webkit-transform: $property;
  -ms-transform: $property;
  transform: $property;
}
.box { @include transform(rotate(30deg)); }
```

</td>
<td>

```css
.box {
  -webkit-transform: rotate(30deg);
  -ms-transform: rotate(30deg);
  transform: rotate(30deg);
}
```

</td>
</tr>
</tbody>
</table>

## Functions
Functions are similar to [mixins](#mixins) but they return values and don't need the `@include` *at-rule*:

```scss
// Set text color based on bg
@function set-text-color($color) {
  @if(lightness($color) > 70) {
    @return #333;
  } @else {
    @return #fff;
  }
}
// ...
color: set-text-color($primary-color);

// or even:
@mixin set-background($color) {
  background-color: $color;
  color: set-text-color($color);
}
// ...
@include set-background($primary-color);
```

### Color functions
**Sass** offers several color-related utility functions, which include:

- ```scss
  lighten($color, $amount) //=> color
  ```

  Makes `$color` lighter.

  The `$amount` must be a number between 0% and 100% (inclusive). Increases the **HSL** lightness of `$color` by that
  amount.

- ```scss
  darken($color, $amount) //=> color
  ```

  Makes `$color` darker.

  The `$amount` must be a number between 0% and 100% (inclusive). Decreases the **HSL** lightness of `$color` by that
  amount.

## Partials
We can create partial **Sass** files that contain little snippets of **CSS** that we can include in other **Sass**
files. This is a great way to modularize our **CSS** and help keep things easier to maintain.

A partial is a **Sass** file named with a leading underscore (e.g. `_partial.scss`). The underscore lets **Sass** know
that the file is only a partial file and that it should not be generated into a **CSS** file. **Sass** partials are used
with the `@use` rule:

<table>
<thead>
<tr>
<th>SCSS</th>
<th>CSS</th>
</tr>
<thead>
<tbody>
<tr>
<td>

```scss
// _base.scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

```scss
// styles.scss
@use "base";

.inverse {
  background-color: base.$primary-color;
  color: white;
}
```

</td>
<td>

```css
body {
  font: 100% Helvetica, sans-serif;
  color: #333;
}

.inverse {
  background-color: #333;
  color: white;
}
```

</td>
</tr>
</tbody>
</table>

## Imports
**Sass** extends **CSS's** `@import` rule with the ability to import **Sass** and **CSS** stylesheets, providing access
to [mixins](#mixins), [functions](#functions), and [variables](#variables) and combining multiple stylesheets' **CSS**
together.

Unlike plain **CSS** imports, which require the browser to make multiple **HTTP** requests as it renders our page,
**Sass** imports are handled entirely during compilation.

## Inheritance
Using `@extend` lets us share a set of **CSS** properties from one selector to another:

```scss
/* This CSS will print because %message-shared is extended. */
%message-shared {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

// This CSS won't print because %equal-heights is never extended.
%equal-heights {
  display: flex;
  flex-wrap: wrap;
}

.message {
  @extend %message-shared;
}

.success {
  @extend %message-shared;
  border-color: green;
}

.error {
  @extend %message-shared;
  border-color: red;
}

.warning {
  @extend %message-shared;
  border-color: yellow;
}
```

The `%message-shared` and `%equal-heights` selectors above are placeholder classes. A *placeholder class* is a special
type of class that only prints when it is extended, and can help keep our compiled **CSS** neat and clean. It is a
feature which goes hand in hand with inheritance.

## Operators
Doing math in our **CSS** is very helpful. **Sass** has a handful of standard math operators like `+`, `-`, `*`,
`math.div()`, and `%`:

<table>
<thead>
<tr>
<th>SCSS</th>
<th>CSS</th>
</tr>
<thead>
<tbody>
<tr>
<td>

```scss
.container {
  width: 100%;
}

article[role="main"] {
  float: left;
  width: 600px / 960px * 100%;
}

article[role="complementary"] {
  float: right;
  width: 300px / 960px * 100%;
}
```

</td>
<td>

```css
.container {
  width: 100%;
}

article[role="main"] {
  float: left;
  width: 62.5%;
}

article[role="complementary"] {
  float: right;
  width: 31.25%;
}
```

</td>
</tr>
</tbody>
</table>

## Interpolation
Interpolation can be used almost anywhere in a **Sass** stylesheet to embed the result of an expression into a chunk of
**CSS**. We just wrap an expression in `#{}`:

```scss
@mixin corner-icon($name, $top-or-bottom, $left-or-right) {
  .icon-#{$name} {
    background-image: url("/icons/#{$name}.svg");
    position: absolute;
    #{$top-or-bottom}: 0;
    #{$left-or-right}: 0;
  }
}

@include corner-icon("mail", top, left);
```

In most cases, interpolation injects the exact same text that would be used if the expression were used as a property
value. But there is one exception: the quotation marks around quoted strings are removed (even if those quoted strings
are in lists):

<table>
<thead>
<tr>
<th>SCSS</th>
<th>CSS</th>
</tr>
<thead>
<tbody>
<tr>
<td>

```scss
.example {
  unquoted: #{"string"};
}
```

</td>
<td>

```css
.example {
  unquoted: string;
}
```

</td>
</tr>
</tbody>
</table>

This makes it possible to write quoted strings that contain syntax that's not allowed in expressions
(like selectors) and interpolate them into style rules.

## Conditionals
**Sass** provides *at-rules* for controlling whether or not a block gets evaluated (including emitting any styles as
**CSS**):

<table>
<thead>
<tr>
<th>SCSS</th>
<th>CSS</th>
</tr>
<thead>
<tbody>
<tr>
<td>

```scss
@mixin triangle($size, $color, $direction) {
  height: 0;
  width: 0;

  border-color: transparent;
  border-style: solid;
  border-width: $size / 2;

  @if $direction == up {
    border-bottom-color: $color;
  } @else if $direction == right {
    border-left-color: $color;
  } @else if $direction == down {
    border-top-color: $color;
  } @else if $direction == left {
    border-right-color: $color;
  } @else {
    @error "Unknown direction #{direction}.";
  }
}

.next {
  @include triangle(5px, black, right);
}
```

</td>
<td>

```css
.next {
  height: 0;
  width: 0;
  border-color: transparent;
  border-style: solid;
  border-width: 2.5px;
  border-left-color: black;
}
```

</td>
</tr>
</tbody>
</table>

## Loops
The `@each` rule makes it easy to emit styles or evaluate code for each element of a [list](#lists) or each pair in a
[map](#maps). It's great for repetitive styles that only have a few variations between them:

```scss
$spaceamounts: (1, 2, 3, 4, 5);
$icons: ("eye": "\f112", "start": "\f12e", "stop": "\f12f");

@each $space in $spaceamounts {
  .m-#{$space} {
    margin: #{$space}rem;
  }
  .my-#{$space} {
    margin: #{$space}rem 0;
  }

  .p-#{$space} {
    padding: #{$space}rem;
  }
  .py-#{$space} {
    padding: #{$space}rem 0;
  }
}

@each $name, $glyph in $icons {
  .icon-#{$name}::before {
    display: inline-block;
    font-family: "Icon Font";
    content: $glyph;
  }
}
```
