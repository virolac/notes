# Svelte

## Table of Contents
- [What is Svelte?](#what-is-svelte%3F)
- [Why use Svelte?](#why-use-svelte%3F)
- [Setup a project](#setup-a-project)
- [Svelte components](#svelte-components)
- [Reactive declarations](#reactive-declarations)
- [Conditional rendering](#conditional-rendering)
- [Looping](#looping)
- [Events](#events)
  - [Listening to events](#listening-to-events)
  - [Event modifiers](#event-modifiers)
  - [Dispatching events](#dispatching-events)
  - [Event forwarding](#event-forwarding)
- [Transitions](#transitions)
- [Stores](#stores)

## What is Svelte?
- **Svelte** is a compiler that generates minimal and highly optimized **JavaScript** code
- **Svelte** works a bit differently than traditional frameworks, but does a lot of the same things. It is often called a framework
- **Svelte** compiles everything down to pure **JavaScript**

## Why use Svelte?
- Create dynamic frontend UIs
- Produces highly optimized **JavaScript**
- No **Virtual DOM**
- About *30%* faster than other frameworks
- Great out of the box animations/transitions
- Great ecosystem (**SvelteKit**, **SvelteNative**)
- Easy to use

## Setup a project
We can setup a new **Svelte** project by using `degit`, a project scaffolding tool.

In the terminal we can create a new project like this:

```sh
npx degit sveltejs/template my-svelte-project
cd my-svelte-project
# to use TypeScript run:
# node scripts/setupTypeScript.js
npm install
npm run dev
```

This will create a new project in the `my-svelte-project` directory, install its dependencies, and start a server on [http://localhost:5000](http://localhost:5000).

## Svelte components
- Reusable pieces of the UI including the output (**HTML**), styling (**CSS**) and logic (**JavaScript**) 
- They are defined in files with a `.svelte` extension
- They can be used like regular **HTML** elements, except the `/` is mandatory in self-closing tags  
  e.g. `<Todos />`
- They can accept **props** passed as attributes inside `{}`  
  e.g. `<Todos todos={todos} />`
- When an attribute has the same name as the variable, it can be omitted along with the `=`  
  e.g. `<Todos {todos} />`
- Inside a component, a **prop** is defined using the `export let <propName>` syntax  
  e.g. `export let todos;`
- Styles are scoped to the component they are defined in

**Example:**

```html
// Logic
<script>
  let numbers = [1, 2, 3, 4];

  function addNumber() {
    numbers.push(numbers.length + 1);
  }
 
  $: sum = numbers.reduce((acc, n) => acc + n, 0);
</script>

// Output
<p>{numbers.join(" + ") = {sum}</p>

<button on:click={addNumber}>
  Add a number
</button>

// Styles
<style>
  button {
    border-radius: 5px;
  }
</style>
```

## Reactive declarations 
Prefixing a statement with `$:` makes it reactive. This means that **Svelte** will re-run that statement whenever any of the referenced values change.

**Example:**

```html
<script>
  let count = 0;

  $: doubled = count * 2;
  $: console.log("the count is " + count);
</script>

<p>{count} doubled is {doubled}</p>
```

## Conditional rendering
To conditionally render some markup, we can wrap it in an `if` block:

```javascript
{#if x > 10}
  <p>{x} is greater than 10</p>
{:else if 5 > x}
  <p>{x} is less than 5</p>
{:else}
  <p>{x} is between 5 and 10</p>
{/if}
```

## Looping
To loop over lists of data, we can use an `each` block:

```javascript
let users = [
  {
    id: "1",
    name: "Alice",
  },
  {
    id: "2",
    name: "Bob",
  },
  {
    id: "3",
    name: "Jen",
  },
];

{#each users as user}
  <h3>{user.id}: {user.name}</h3>
{/each}
```

We can get the current *index* as a second argument, like so:

```javascript
{#each cats as cat, i}
  <li>
    <a target="_blank" href="https://www.youtube.com/watch?v={cat.id}" rel="noreferrer">
      {i + 1}: {cat.name}
    </a>
  </li>
{/each}
```

By default, modifying the value of an `each` block will add and remove items at the end of the block, and update any
values that have changed. To fix this behavior, we can pass a key expression - which must uniquely identify each item -
to the `each` block:

```javascript
{#each things as thing (thing.id)}
  <Thing name={thing.name} />
{/each}
```

## Events
### Listening to events
We can listen to any event on an element with the `on:` directive:

```html
<div on:mousemove={handleMouseMove}>
  The mouse position is {m.x} x {m.y}
</div>
```

### Event modifiers
**DOM** event handlers can have *modifiers* that alter their behaviour. 

For example, a handler with a `once` modifier will only run a single time:

```html
<script>
  function handleClick() {
    alert("no more alerts");
  }
</script>

<button on:click|once={handleClick}>
  Click me
</button>
```

The full list of modifiers:

- `preventDefault` — calls event.preventDefault() before running the handler
- `stopPropagation` — calls event.stopPropagation(), preventing the event reaching the next element
- `passive` — improves scrolling performance on touch/wheel events (**Svelte** will add it automatically where it's safe to do so)
- `nonpassive` — explicitly set `passive: false`
- `capture` — fires the handler during the capture phase instead of the bubbling phase
- `once` — remove the handler after the first time it runs
- `self` — only trigger handler if `event.target` is the element itself
- `trusted` — only trigger handler if `event.isTrusted` is `true`, i.e. if the event is triggered by a user action

We can chain modifiers together, e.g. `on:click|once|capture={...}`.

### Dispatching events
Components can also dispatch events. To do so, they must create an event dispatcher:

```javascript
import { createEventDispatcher } from "svelte";

const dispatch = createEventDispatcher();

const handleDelete = (itemId) => {
  dispatch("delete-feedback", itemId);
};
```

The data given as the second argument to the `dispatch` function can be accessed using the `detail` property of the `event` object.

### Event forwarding
Unlike **DOM** events, component events don't bubble. If we want to listen to an event on some deeply nested component,
the intermediate components must *forward* the event (works for **DOM** elements too). As a shorthand, we can do that by omitting the handler when
declaring the event:

```html
<Inner on:message />
```

## Transitions
We can make more appealing user interfaces by gracefully transitioning elements into and out of the **DOM**.

**Svelte** makes this very easy with the `transition` directive:

```javascript
import { fade, scale } from "svelte/transition";

<div transition:fade>
</div>

<div in:scale out:fade="{{ duration: 500 }}">
</div>
```

## Stores
When we have values that need to be accessed by multiple unrelated components we can use **stores**:

```javascript
import { writable } from "svelte/store";

export const UserStore = writable([
  {
    id: "1",
    name: "Alice",
  },
  {
    id: "2",
    name: "Bob",
  },
  {
    id: "3",
    name: "Jen",
  },
]);
```

A **store** is simply an object with a `subscribe` method that allows interested parties to be notified whenever the
store value changes. The `subscribe` method returns an `unsubscribe` function that we can use to avoid *memory leaks*.
We can call it in the `onDestroy` [lifecycle hook](https://svelte.dev/tutorial/ondestroy):

```html
<script>
  import { onDestroy } from "svelte";
  import { count } from "./stores.js";

  let countValue;

  const unsubscribe = count.subscribe(value => {
    countValue = value;
  });

  onDestroy(unsubscribe);
</script>

<h1>The count is {countValue}</h1>
```

We can auto-subscribe/unsubscribe from a **store** by prefixing its name with `$`:

```html
<script>
  import { count } from "./stores.js";
</script>

<h1>The count is {$count}</h1>
```

Stores can be **writable** or only **readable**.
