# Vue.js

## General
- Uses a **Virtual DOM** representation, similar to **React**
- Interface is divided into components, which include a template for markup, logic including any state/data/methods as
  well as the styling for that component
- [**Vuex**](https://vuex.vuejs.org/) is a *state manager* for global state in larger applications (similar to [**Redux**](https://redux.js.org/) for **React**)
- **Vue 3** introduced the [**Composition API**](https://vuejs.org/guide/extras/composition-api-faq.html), which aims to
  address code re-usability and readability, especially in larger applications
- **Vue** offers the [**Vue CLI**](#vue-cli), a command line interface for creating **Vue** apps

## Vue CLI
- `ui` - launches a **Graphical User Interface** to manage **Vue** projects
- `create` - accepts a project name and then asks a series of questions to configure the project before creating it

## Components
- Components are usually defined in a dedicated file using the `.vue` extension - known as a [**Single-File Component**](https://vuejs.org/guide/scaling-up/sfc.html)
  (**SFC** for short)
- A component is a **JavaScript** object with special properties and methods
- We can also pass "props" into a component:
  ```html
  <Header title="Task Tracker" />
  ```
- Components can have their own state which can determine how a specific component behaves and what data is displayed
- Some state may be local to a specific component and some may be "global"- or "app"-level state that needs to be shared
  with multiple components
- When defining styles for the component, use the `scoped` attribute to apply the styles to the current component only

### Properties
- #### `name`
  Explicitly declare a display name for the component.

- #### `template`
  Contains the markup for the component as a string.

  **Example:**

  ```javascript
  export default {
    template: "<h1>Hello, world!</h1>"
  };
  ```

- #### `data`
  A function that returns the initial reactive state for the component instance. The function is expected to return a
  plain **JavaScript** object, which will be made reactive by **Vue**.

  **Example:**

  ```javascript
  export default {
    data() {
      return { a: 1 }
    },
    created() {
      console.log(this.a); // => 1
      console.log(this.$data); // => { a: 1 }
    }
  };
  ```

- #### `methods`
  Methods to be mixed into the component instance. Declared methods can be directly accessed on the component instance,
  or used in template expressions. All methods have their `this` context automatically bound to the component instance,
  even when passed around.

  **Example:**

  ```javascript
  export default {
    data() {
      return { a: 1 };
    },
    methods: {
      plus() {
        this.a++;
      }
    },
    created() {
      this.plus();
      console.log(this.a); // => 2
    }
  };

  ```

- #### `props`
  A list/hash of attributes that are exposed to accept data from the parent component. It has a simple `Array`-based
  syntax and an alternative `Object`-based syntax that allows advanced configurations such as *type checking*, *custom
  validation* and *default values*.

  **Example:**

  ```javascript
  // simple syntax
  export default {
    props: ["size", "myMessage"]
  };

  // object declaration with validations
  export default {
    props: {
      // type check
      height: Number,
      // type check plus other validations
      age: {
        type: Number,
        default: 0,
        required: true,
        validator: (value) => {
          return value >= 0;
        }
      }
    }
  };
  ```

- #### `components`
  An object that registers components to be made available to the component instance.

- #### `emits`
  Declare the custom events emitted by the component.

  **Example:**

  ```javascript
  // array syntax
  export default {
    emits: ['check'],
    created() {
      this.$emit('check');
    }
  };

  // object syntax
  export default {
    emits: {
      // no validation
      click: null,

      // with validation
      submit: (payload) => {
        if (payload.email && payload.password) {
          return true;
        } else {
          console.warn(`Invalid submit event payload!`);
          return false;
        }
      }
    }
  };
  ```

- #### `computed`
  Declare computed properties to be exposed on the component instance.

  **Example:**

  ```javascript
  export default {
    data() {
      return { a: 1 };
    },
    computed: {
      // readonly
      aDouble() {
        return this.a * 2;
      },
      // writable
      aPlus: {
        get() {
          return this.a + 1;
        },
        set(v) {
          this.a = v - 1;
        }
      }
    },
    created() {
      console.log(this.aDouble); // => 2
      console.log(this.aPlus); // => 2

      this.aPlus = 3;
      console.log(this.a); // => 2
      console.log(this.aDouble); // => 4
    }
  };
  ```

## Basic SFC layout
```html
<template>
  <header>
    <h1>{{title}}</h1>
  </header>
</template>

<script>
export default {
  props: {
    title: String
  }
}
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
```

## Creating a Vue application
```javascript
import { createApp } from "vue";
import App from "./App.vue";

const app = createApp(App);

app.mount("#root"); // argument can be an actual DOM element or a selector string
```

## Built-in directives
A directive is some special token in the markup that tells the library to do something to a **DOM element**

### # v-text
Update the element's text content.

- **Details**  
`v-text` works by setting the element's `textContent` property, so it will overwrite any existing content inside the
element. If we need to update part of `textContent`, we should use [**mustache interpolations**](https://vuejs.org/guide/essentials/template-syntax.html#text-interpolation) instead.


- **Example:**
  ```html
  <span v-text="msg"></span>
  <!-- same as -->
  <span>{{msg}}</span>
  ```

### # v-bind
Dynamically bind one or more attributes, or a component prop to an expression.

- **Shorthand:** `:` or `.` (when using `.prop` modifier)

- **Example:**  
  ```html
  <!-- bind an attribute -->
  <img v-bind:src="imageSrc" />

  <!-- shorthand -->
  <img :src="imageSrc" />
  ```

### # v-if
Conditionally render an element or a template fragment based on the truthy-ness of the expression value.

- **Details**  
  When a `v-if` element is toggled, the element and its contained directives / components are destroyed and
  re-constructed. If the initial condition is *falsy*, then the inner content won't be rendered at all.

### # v-show
Toggle the element's visibility based on the truthy-ness of the expression value.

- **Details**  
  `v-show` works by setting the `display` **CSS** property via inline styles, and will try to respect the initial `display`
  value when the element is visible. It also triggers transitions when its condition changes.

### # v-for
Render the element or template block multiple times based on the source data.

- **Details**  
  The directive's value must use the special syntax `alias in expression` to provide an alias for the current element
  being iterated on:

  ```html
  <div v-for="item in items">
    {{ item.text }}
  </div>
  ```

  Alternatively, we can also specify an alias for the index (or the key if used on an `Object`):

  ```html
  <div v-for="(item, index) in items"></div>
  <div v-for="(value, key) in object"></div>
  <div v-for="(value, name, index) in object"></div>
  ```

  The default behavior of `v-for` will try to patch the elements in-place without moving them. To force it to reorder
  elements, we should provide an ordering hint with the `key` special attribute:

  ```html
  <div v-for="item in items" :key="item.id">
    {{ item.text }}
  </div>
  ```

### # v-on
Attach an event listener to the element.

- **Shorthand:** `@`

- **Argument:** event

- **Details**  
  The event type is denoted by the argument. The expression can be a *method name*, an *inline statement*, or omitted if
  there are modifiers present.

- **Example:**
  ```html
  <button v-on:click="doThis"></button>

  <!-- shorthand -->
  <button @click="doThis"></button>
  ```

### # v-model
Create a two-way binding on a form input element or a component.

- **Limited to:**
  - `<input>`
  - `<select>`
  - `<textarea>`
  - components

<br>

- **Details**  
  Under the hood, the template compiler expands `<input v-model="searchText" />` to this more verbose equivalent:

  ```html
  <input
    :value="searchText"
    @input="searchText = $event.target.value"
  />
  ```

  When used on a component, `v-model` instead expands to this:

  ```html
  <CustomInput
    :modelValue="searchText"
    @update:modelValue="newValue => searchText = newValue"
  />
  ```

  For this to actually work with a custom input component, it must do two things:

  1. Bind the `value` attribute of a native `<input>` element to the `modelValue` prop
  2. When a native input event is triggered, emit an `update:modelValue` custom event with the new value

  <br>

  ```html
  <!-- CustomInput.vue -->
  <script>
  export default {
    props: ['modelValue'],
    emits: ['update:modelValue']
  }
  </script>

  <template>
    <input
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
    />
  </template>
  ```

  Now `v-model` should work perfectly with this component:

  ```html
  <CustomInput v-model="searchText" />
  ```

## Lifecycle hooks
- `created` - called after the instance has finished processing all state-related options.

## Dynamically binding classes
We can bind `:class` to an array to apply a list of classes:

```javascript
data() {
  return {
    activeClass: 'active',
    errorClass: 'text-danger'
  };
}
```

```html
<div :class="[activeClass, errorClass]"></div>
```

Which will render:

```html
<div class="active text-danger"></div>
```

## vue.config.js
`vue.config.js` is an optional config file that will be automatically loaded by `@vue/cli-service` if it's present in
the project root (next to `package.json`).

The file should export an object containing options:

```javascript
module.exports = {
  devServer: {
    proxy: "http://localhost:4000"
  }
}
```

This will tell the *development server* to **proxy** any unknown requests (requests that did not match a static file to
http://locahost:4000.

## Vue Router
### HTML
```html
<script src="https://unpkg.com/vue@3"></script>
<script src="https://unpkg.com/vue-router@4"></script>

<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!-- use the router-link component for navigation. -->
    <!-- specify the link by passing the `to` prop. -->
    <!-- `<router-link>` will render an `<a>` tag with the correct `href` attribute -->
    <router-link to="/">Go to Home</router-link>
    <router-link to="/about">Go to About</router-link>
  </p>
  <!-- route outlet -->
  <!-- component matched by the route will render here -->
  <router-view></router-view>
</div>
```

- **router-link**  
  Note how instead of using regular `a` tags, we use a custom component `router-link` to create links. This allows
  **Vue Router** to change the **URL** without reloading the page, handle **URL** generation, as well as its encoding.

- **router-view**  
  `router-view` will display the component that corresponds to the **URL**. We can put it anywhere to adapt it to our
  layout.

### JavaScript
```javascript
// 1. Define route components.
// These can be imported from other files
const Home = { template: "<div>Home</div>" };
const About = { template: "<div>About</div>" };

// 2. Define some routes
// Each route should map to a component.
const routes = [
  { path: "/", component: Home },
  { path: "/about", component: About }
]

// 3. Create the router instance and pass the `routes` option
const router = VueRouter.createRouter({
  // 4. Provide the history implementation to use.
  history: VueRouter.createWebHashHistory(),
  routes // short for `routes: routes`
})

// 5. Create and mount the root instance.
const app = Vue.createApp({});
// Make sure to use the router instance to make the
// whole app router-aware.
app.use(router);
app.mount("#app");

// Now the app has started!
```

By calling `app.use(router)`, we get access to it as `this.$router` as well as the current route as `this.$route` inside
of any component.
