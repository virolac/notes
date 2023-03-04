# TailwindCSS

## Table of Contents
- [General](#general)
- [Installation](#installation)
- [Configuration](#configuration)
  - [Content](#content)
  - [Theme](#theme)
  - [Plugins](#plugins)
  - [Important](#important)
- [Utility classes](#utility-classes)
  - [Container](#container)
  - [Position](#position)
  - [Top / Right / Bottom / Left](#top-%2F-right-%2F-bottom-%2F-left)
  - [Width](#width)
  - [Height](#height)
  - [Min-Width](#min-width)
  - [Max-Width](#max-width)
  - [Min-Height](#min-height)
  - [Max-Height](#max-height)
  - [Margin](#margin)
  - [Padding](#padding)
  - [Space Between](#space-between)
  - [Display](#display)
  - [Flex Direction](#flex-direction)
  - [Justify Content](#justify-content)
  - [Align Items](#align-items)
  - [Align Self](#align-self)
  - [Text Align](#text-align)
  - [Font Weight](#font-weight)
  - [Line Height](#line-height)
  - [Background Color](#background-color)
  - [Border Radius](#border-radius)
  - [Box Shadow](#box-shadow)
  - [Drop Shadow](#drop-shadow)
- [Conditional styles](#conditional-styles)
- [Styling based on parent state](#styling-based-on-parent-state)
  - [Differentiating nested groups](#differentiating-nested-groups)
- [Custom styles](#custom-styles)

## General
- A utility-first **CSS** framework for rapidly building responsive custom user interfaces
- Unlike **Bootstrap**, it doesn't offer components like *buttons* and *navbars* but is more easily customizable
- The compiler performs dead-code elimination so any styles that are not used in a project get removed at build time
- Often criticized for resulting in very long `class` lists

## Installation
1. Install the `tailwindcss` package:
   ```sh
   npm install -D tailwindcss
   ```
2. Create the configuration file (`tailwind.config.js`):
   ```sh
   npx tailwindcss init
   ```

3. Customize the [configuration](#configuration):
   ```javascript
   module.exports = {
     content: ["./src/**/*.{html,js}"],
     ...
   };
   ```

4. Add the `@tailwind` directives for each of **Tailwind**'s layers to the input **CSS** file:
   ```css
   @tailwind base;
   @tailwind components;
   @tailwind utilities;
   ```

5. Run the **CLI** tool to scan the template files for **Tailwind** classes and build the output **CSS**:
   ```sh
   npx tailwindcss -i ./src/input.css -o ./dist/output.css --watch
   ```

## Configuration
By default **Tailwind** will look for an optional `tailwind.config.js` file at the root of a project where we can define
any customizations.

### Content
The `content` section is where we configure the paths to all of our **HTML** templates, **JavaScript** components, and
any other files that contain **Tailwind** class names.

```javascript
module.exports = {
  content: [
    "./pages/**/*.{html,js}",
    "./components/**/*.{html,js}"
  }
  // ...
};
```

### Theme
The `theme` section is where we define the color palette, fonts, type scale, border sizes, breakpoints -- anything
related to the visual design of our application.

```javascript
module.exports = {
  // ...
  theme: {
    colors: {
      "blue": "#1fb6ff",
      "purple": "#7e5bef",
      // ...
    },
    fontFamily: {
      sans: ["Graphik", "sans-serif"],
      serif: ["Merriweather", "serif"]
    },
    screens: {
      sm: "480px",
      md: "768px",
      lg: "972px",
      xl: "1440px"
    },
    extend: {
      spacing: {
        "8xl": "96rem",
        "9xl": "128rem"
      },
      borderRadius: {
        "4xl": "2rem"
      }
    }
  }
};
```

### Plugins
The `plugins` section allows us to register plugins with **Tailwind** that can be used to generate extra utilities,
components, base styles, or custom variants.

```javascript
module.exports = {
  // ...
  plugins: [
    require("@tailwindcss/forms"),
    require("@tailwindcss/aspect-ratio"),
    require("@tailwindcss/typography"),
    require("tailwindcss-children")
  ]
};
```

### Important
The `important` option lets us control whether or not **Tailwind**'s utilities should be marked with `!important`. This
can be really useful when using **Tailwind** with existing **CSS** that has high specificity selectors.

```javascript
module.exports = {
  important: true
};
```

## Utility classes
### [**Container**](https://tailwindcss.com/docs/container)
A component for fixing an element's width to the current breakpoint.

|   Breakpoint   |      Properties      |
|:--------------:|:--------------------:|
|     *None*     |    `width: 100%;`    |
| *sm (640px)*   | `max-width: 640px;`  |
| *md (768px)*   | `max-width: 768px;`  |
| *lg (1024px)*  | `max-width: 1024px;` |
| *xl (1280px)*  | `max-width: 1280px;` |
| *2xl (1536px)* | `max-width: 1536px;` |

### [**Position**](https://tailwindcss.com/docs/position)
Utilities for controlling how an element is positioned in the **DOM**.

- `static`
  ```css
  position: static;
  ```

- `fixed`
  ```css
  position: fixed;
  ```

- `absolute`
  ```css
  position: absolute;
  ```

- `relative`
  ```css
  position: relative;
  ```

- `sticky`
  ```css
  position: sticky;
  ```

### [**Top / Right / Bottom / Left**](https://tailwindcss.com/docs/top-right-bottom-left)
Utilities for controlling the placement of positioned elements.

- `inset-0`
  ```css
  top: 0px;
  right: 0px;
  bottom: 0px;
  left: 0px;
  ```

- `inset-x-0`
  ```css
  left: 0px;
  right: 0px;
  ```

- `inset-y-0`
  ```css
  top: 0px;
  bottom: 0px;
  ```

- `top-0`
  ```css
  top: 0px;
  ```

- `right-0`
  ```css
  right: 0px;
  ```

- `bottom-0`
  ```css
  bottom: 0px;
  ```

- `left-0`
  ```css
  left: 0px;
  ```

### [**Width**](https://tailwindcss.com/docs/width)
Utilities for setting the width of an element.

- `w-0`
  ```css
  width: 0px;
  ```

- `w-{amount}`
  ```css
  width: {amount * 0.25}rem; /* {amount * 4}px */
  ```

- `w-{fraction}`
  ```css
  width: {fraction * 100}%;
  ```

- `w-auto`
  ```css
  width: auto;
  ```

- `w-full`
  ```css
  width: 100%;
  ```

- `w-screen`
  ```css
  width: 100vw;
  ```

- `w-min`
  ```css
  width: min-content;
  ```

- `w-max`
  ```css
  width: max-content;
  ```

- `w-fit`
  ```css
  width: fit-content;
  ```

### [**Height**](https://tailwindcss.com/docs/height)
Utilities for setting the height of an element.

- `h-0`
  ```css
  height: 0px;
  ```

- `h-{amount}`
  ```css
  height: {amount * 0.25}rem; /* {amount * 4}px */
  ```

- `h-{fraction}`
  ```css
  height: {fraction * 100}%;
  ```

- `h-auto`
  ```css
  height: auto;
  ```

- `h-full`
  ```css
  height: 100%;
  ```

- `h-screen`
  ```css
  height: 100vw;
  ```

- `h-min`
  ```css
  height: min-content;
  ```

- `h-max`
  ```css
  height: max-content;
  ```

- `h-fit`
  ```css
  height: fit-content;
  ```

### [**Min-Width**](https://tailwindcss.com/docs/min-width)
Utilities for setting the minimum width of an element.

- `min-w-0`
  ```css
  min-width: 0px;
  ```

- `min-w-full`
  ```css
  min-width: 100%;
  ```

- `min-w-min`
  ```css
  min-width: min-content;
  ```

- `min-w-max`
  ```css
  min-width: max-content;
  ```

- `min-w-fit`
  ```css
  min-width: fit-content;
  ```

### [**Max-Width**](https://tailwindcss.com/docs/max-width)
Utilities for setting the maximum width of an element.

- `max-w-0`
  ```css
  max-width: 0rem; /* 0px */
  ```

- `max-w-none`
  ```css
  max-width: none;
  ```

- `max-w-xs`
   ```css
   max-width: 20rem; /* 320px */
   ```

- `max-w-sm`
   ```css
   max-width: 24rem; /* 384px */
   ```

- `max-w-md`
   ```css
   max-width: 28rem; /* 448px */
   ```

- `max-w-lg`
   ```css
   max-width: 32rem; /* 512px */
   ```

- `max-w-xl`
   ```css
   max-width: 36rem; /* 576px */
   ```

- `max-w-full`
  ```css
  max-width: 100%;
  ```

- `max-w-min`
  ```css
  max-width: min-content;
  ```

- `max-w-max`
  ```css
  max-width: max-content;
  ```

- `max-w-fit`
  ```css
  max-width: fit-content;
  ```

- `max-w-prose`
  ```css
  max-width: 65ch;
  ```

- `max-w-screen-sm`
  ```css
  max-width: 640px;
  ```

- `max-w-screen-md`
  ```css
  max-width: 768px;
  ```

- `max-w-screen-lg`
  ```css
  max-width: 1024;
  ```

- `max-w-screen-xl`
  ```css
  max-width: 1280px;
  ```

### [**Min-Height**](https://tailwindcss.com/docs/min-height)
Utilities for setting the minimum height of an element.

- `min-h-0`
  ```css
  min-height: 0px;
  ```

- `min-h-full`
  ```css
  min-height: 100%;
  ```

- `min-h-screen`
  ```css
  min-height: 100vh;
  ```

- `min-h-min`
  ```css
  min-height: min-content;
  ```

- `min-h-max`
  ```css
  min-height: max-content;
  ```

- `min-h-fit`
  ```css
  min-height: fit-content;
  ```

### [**Max-Height**](https://tailwindcss.com/docs/max-height)
Utilities for setting the maximum height of an element.

- `max-h-0`
  ```css
  max-height: 0rem; /* 0px */
  ```

- `max-h-none`
  ```css
  max-height: none;
  ```

- `max-h-xs`
   ```css
   max-height: 20rem; /* 320px */
   ```

- `max-h-sm`
   ```css
   max-height: 24rem; /* 384px */
   ```

- `max-h-md`
   ```css
   max-height: 28rem; /* 448px */
   ```

- `max-h-lg`
   ```css
   max-height: 32rem; /* 512px */
   ```

- `max-h-xl`
   ```css
   max-height: 36rem; /* 576px */
   ```

- `max-h-full`
  ```css
  max-height: 100%;
  ```

- `max-h-min`
  ```css
  max-height: min-content;
  ```

- `max-h-max`
  ```css
  max-height: max-content;
  ```

- `max-h-fit`
  ```css
  max-height: fit-content;
  ```

- `max-h-prose`
  ```css
  max-height: 65ch;
  ```

- `max-h-screen-sm`
  ```css
  max-height: 640px;
  ```

- `max-h-screen-md`
  ```css
  max-height: 768px;
  ```

- `max-h-screen-lg`
  ```css
  max-height: 1024;
  ```

- `max-h-screen-xl`
  ```css
  max-height: 1280px;
  ```

### [**Margin**](https://tailwindcss.com/docs/margin)
Utilities for controlling an element's margin.

- `m-0`
  ```css
  margin: 0px;
  ```

- `mx-0`
  ```css
  margin-left: 0px;
  margin-right: 0px;
  ```

- `my-0`
  ```css
  margin-top: 0px;
  margin-bottom: 0px;
  ```

- `mt-0`
  ```css
  margin-top: 0px;
  ```

- `mr-0`
  ```css
  margin-right: 0px;
  ```

- `mb-0`
  ```css
  margin-bottom: 0px;
  ```

- `ml-0`
  ```css
  margin-left: 0px;
  ```

- `m-{amount}`
  ```css
  margin: {amount * 0.25}rem; /* {amount * 4}px */
  ```

- `-m-{amount}`
  ```css
  margin: -{amount * 0.25}rem; /* -{amount * 4}px */
  ```

- `m-auto`
  ```css
  margin: auto;
  ```

### [**Padding**](https://tailwindcss.com/docs/padding)
Utilities for controlling an element's padding.

- `p-0`
  ```css
  padding: 0px;
  ```

- `px-0`
  ```css
  padding-left: 0px;
  padding-right: 0px;
  ```

- `py-0`
  ```css
  padding-top: 0px;
  padding-bottom: 0px;
  ```

- `pt-0`
  ```css
  padding-top: 0px;
  ```

- `pr-0`
  ```css
  padding-right: 0px;
  ```

- `pb-0`
  ```css
  padding-bottom: 0px;
  ```

- `pl-0`
  ```css
  padding-left: 0px;
  ```

- `p-{amount}`
  ```css
  padding: {amount * 0.25}rem; /* {amount * 4}px */
  ```

### [**Space Between**](https://tailwindcss.com/docs/space)
Utilities for controlling the space between child elements.

- `space-x-0`
  ```css
  margin-left: 0px;
  ```

- `space-y-0`
  ```css
  margin-top: 0px;
  ```

- `space-x-{amount}`
  ```css
  margin-left: {amount * 0.25}rem; /* {amount * 4}px */
  ```
- `space-y-{amount}`
  ```css
  margin-top: {amount * 0.25}rem; /* {amount * 4}px */
  ```

### [**Display**](https://tailwindcss.com/docs/display)
Utilities for controlling the display box type of an element.

- `hidden`
  ```css
  display: none;
  ```

- `inline`
  ```css
  display: inline;
  ```

- `block`
  ```css
  display: block;
  ```

- `inline-block`
  ```css
  display: inline-block;
  ```

- `flex`
  ```css
  display: flex;
  ```

- `inline-flex`
  ```css
  display: inline-flex;
  ```

- `grid`
  ```css
  display: grid;
  ```

- `inline-grid`
  ```css
  display: inline-grid;
  ```

### [**Flex Direction**](https://tailwindcss.com/docs/flex-direction)
Utilities for controlling the direction of flex items.

- `flex-row`
  ```css
  flex-direction: row;
  ```

- `flex-row-reverse`
  ```css
  flex-direction: row-reverse;
  ```

- `flex-col`
  ```css
  flex-direction: column;
  ```

- `flex-col-reverse`
  ```css
  flex-direction: column-reverse;
  ```

### [**Justify Content**](https://tailwindcss.com/docs/justify-content)
Utilities for controlling how flex and grid items are positioned along a container's main axis.

- `justify-start`
  ```css
  justify-content: flex-start;
  ```

- `justify-end`
  ```css
  justify-content: flex-end;
  ```

- `justify-center`
  ```css
  justify-content: center;
  ```

- `justify-between`
  ```css
  justify-content: space-between;
  ```

- `justify-around`
  ```css
  justify-content: space-around;
  ```

- `justify-evenly`
  ```css
  justify-content: space-evenly;
  ```

### [**Align Items**](https://tailwindcss.com/docs/align-items)
Utilities for controlling how flex and grid items are positioned along a container's cross axis.

- `items-start`
  ```css
  align-items: flex-start;
  ```

- `items-end`
  ```css
  align-items: flex-end;
  ```

- `items-center`
  ```css
  align-items: center;
  ```

- `items-baseline`
  ```css
  align-items: baseline;
  ```

- `items-stretch`
  ```css
  align-items: stretch;
  ```

### [**Align Self**](https://tailwindcss.com/docs/align-self)
Utilities for controlling how an individual flex or grid item is positioned along its container's cross axis.

- `self-auto`
  ```css
  align-self: auto;
  ```

- `self-start`
  ```css
  align-self: flex-start;
  ```

- `self-end`
  ```css
  align-self: flex-end;
  ```

- `self-center`
  ```css
  align-self: center;
  ```

- `self-stretch`
  ```css
  align-self: stretch;
  ```

- `self-baseline`
  ```css
  align-self: baseline;
  ```

### [**Text Align**](https://tailwindcss.com/docs/text-align)
Utilities for controlling the alignment of text.

- `text-left`
  ```css
  text-align: left;
  ```

- `text-center`
  ```css
  text-align: center;
  ```

- `text-right`
  ```css
  text-align: right;
  ```

- `text-justify`
  ```css
  text-align: justify;
  ```

- `text-start`
  ```css
  text-align: start;
  ```

- `text-end`
  ```css
  text-align: end;
  ```

### [**Font Weight**](https://tailwindcss.com/docs/font-weight)
Utilities for controlling the font weight of an element.

- `font-thin`
  ```css
  font-weight: 100;
  ```

- `font-extralight`
  ```css
  font-weight: 200;
  ```

- `font-light`
  ```css
  font-weight: 300;
  ```

- `font-normal`
  ```css
  font-weight: 400;
  ```

- `font-medium`
  ```css
  font-weight: 500;
  ```

- `font-semibold`
  ```css
  font-weight: 600;
  ```

- `font-bold`
  ```css
  font-weight: 700;
  ```

- `font-extrabold`
  ```css
  font-weight: 800;
  ```

- `font-black`
  ```css
  font-weight: 900;
  ```

### [**Line Height**](https://tailwindcss.com/docs/line-height)
Utilities for controlling the leading (line height) of an element.

- `leading-none`
  ```css
  line-height: 1;
  ```

- `leading-tight`
  ```css
  line-height: 1.25;
  ```

- `leading-snug`
  ```css
  line-height: 1.375;
  ```

- `leading-normal`
  ```css
  line-height: 1.5;
  ```

- `leading-relaxed`
  ```css
  line-height: 1.625;
  ```

- `leading-loose`
  ```css
  line-height: 2;
  ```

### [**Background Color**](https://tailwindcss.com/docs/background-color)
Utilities for controlling an element's background color.

- `bg-inherit`
  ```css
  background-color: inherit;
  ```

- `bg-current`
  ```css
  background-color: currentColor;
  ```

- `bg-transparent`
  ```css
  background-color: transparent;
  ```

- `bg-black`
  ```css
  background-color: rgb(0 0 0);
  ```

- `bg-white`
  ```css
  background-color: rgb(255 255 255);
  ```

- `bg-gray-500`
  ```css
  background-color: rgb(107 114 128);;
  ```

### [**Border Radius**](https://tailwindcss.com/docs/border-radius)
Utilities for controlling the border radius of an element.

- `rounded-none`
  ```css
  border-radius: 0px;
  ```

- `rounded-sm`
  ```css
  border-radius: 0.125rem; /* 2px */
  ```

- `rounded`
  ```css
  border-radius: 0.25rem; /* 4px */
  ```

- `rounded-md`
  ```css
  border-radius: 0.375rem; /* 6px */
  ```

- `rounded-lg`
  ```css
  border-radius: 0.5rem; /* 8px */
  ```

- `rounded-xl`
  ```css
  border-radius: 0.75rem; /* 12px */
  ```

- `rounded-2xl`
  ```css
  border-radius: 1rem; /* 16px */
  ```

- `rounded-full`
  ```css
  border-radius: 9999px;
  ```

### [**Box Shadow**](https://tailwindcss.com/docs/box-shadow)
Utilities for controlling the box shadow of an element.

- `shadow`
  ```css
  box-shadow: 0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1);
  ```

- `shadow-inner`
  ```css
  box-shadow: inset 0 2px 4px 0 rgb(0 0 0 / 0.05);
  ```

- `shadow-none`
  ```css
  box-shadow: 0 0 #0000;
  ```

### [**Drop Shadow**](https://tailwindcss.com/docs/drop-shadow)
Utilities for applying drop-shadow filters to an element.

- `drop-shadow`
  ```css
  filter: drop-shadow(0 1px 2px rgb(0 0 0 / 0.1)) drop-shadow(0 1px 1px rgb(0 0 0 / 0.06));
  ```

- `drop-shadow-none`
  ```css
  filter: drop-shadow(0 0 #0000);
  ```

## Conditional styles
Every utility class in **Tailwind** can be applied *conditionally* by adding a modifier to the beginning of the class
name that describes the condition we want to target.

For example, to apply the `bg-sky-700` class on hover, we use the `hover:bg-sky-700` class:

```html
<button class="bg-sky-500 hover:bg-sky-700 ...">
  Save changes
</button>
```

## Styling based on parent state
When we need to style an element based on the state of some *parent* element, we mark the parent with the `group`
class, and use `group-*` modifiers like `group-hover` to style the target element:

```html
<a href="#" class="group block max-w-xs mx-auto rounded-lg p-6 bg-white ring-1 ring-slate-900/5 shadow-lg space-y-3 hover:bg-sky-500 hover:ring-sky-500">
  <div class="flex items-center space-x-3">
    <svg class="h-6 w-6 stroke-sky-500 group-hover:stroke-white" fill="none" viewBox="0 0 24 24"><!-- ... --></svg>
    <h3 class="text-slate-900 group-hover:text-white text-sm font-semibold">New project</h3>
  </div>
  <p class="text-slate-500 group-hover:text-white text-sm">Create a new project from a variety of starting templates.</p>
</a>
```

This pattern works with every pseudo-class modifier, for example `group-focus`, `group-active`, or even `group-odd`.

### Differentiating nested groups
When nesting groups, we can style something based on the state of a *specific* parent group by giving that parent a
unique group name using a `group/{name}` class, and including that name in modifiers using classes like
`group-hover/{name}`:

```html
<ul role="list">
  {#each people as person}
    <li class="group/item hover:bg-slate-100 ...">
      <img src="{person.imageUrl}" alt="" />
      <div>
        <a href="{person.url}">{person.name}</a>
        <p>{person.title}</p>
      </div>
      <a class="group/edit invisible hover:bg-slate-200 group-hover/item:visible ..." href="tel:{person.phone}">
        <span class="group-hover/edit:text-gray-700 ...">Call</span>
        <svg class="group-hover/edit:translate-x-0.5 group-hover/edit:text-slate-500 ...">
          <!-- ... -->
        </svg>
      </a>
    </li>
  {/each}
</ul>
```

## Custom styles
We can use the `@layer` directive to tell **Tailwind** which "bucket" a set of custom styles belongs to. Valid layers
are `base`, `components`, and `utilities`.

We can combine it with the `@apply` directive to inline any existing utility classes into our own custom **CSS**.

Wrapping any custom **CSS** in a `@layer` directive also makes it possible to use modifiers with those rules, like
`hover:` and `focus:` or responsive modifiers like `md:` and `lg:`.

**Example:**

```css
@layer components {
  .sidebar-icon {
    @apply relative flex items-center justify-center 
           h-12 w-12 mt-2 mb-2 mx-auto shadow-lg
           bg-gray-800 text-green-500
           hover:bg-green-600 hover:text-white
           rounded-3x1 hover:rounded-x1
           transition-all duration-300 ease-linear
           cursor-pointer;
  }
}
```
