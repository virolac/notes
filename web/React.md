# React

## General
- A **JavaScript** library for building user interfaces
- Maintained by **Facebook**
- Declarative and composable
- Uses a **Virtual DOM** representation
- A **React** application is composed of independent, reusable pieces called components
- **React** components can be defined as classes or functions, with function components being more popular

## What is the Virtual DOM?
The **V**irtual **DOM** (**VDOM**) is a programming concept where an ideal, or "virtual", representation of a UI is kept
in memory and synced with the "real" **DOM** by a library such as **ReactDOM**.

This approach enables the declarative **API** of **React**: We tell **React** what state we want the UI to be in, and it
makes sure the **DOM** matches that state. This abstracts out the *attribute manipulation*, *event handling*, and manual
**DOM** updating that we would otherwise have to use to build our app.

## Elements
**React** elements are the building blocks of **React** applications. They are immutable and describe what we want to
see on the screen.

We can create them using the `React.createElement(type, props, ...children)` function. The `type` argument can be either
a tag name string (such as `div` or `span`), a **React** component type (a [class](#class-components) or a
[function](#function-components)), or a **React** fragment type (`<>...</>`).

Typically, elements are not used directly, but get returned from [components](#components).

## JSX
Consider this variable declaration:

```javascript
const element = <h1>Hello, world!</h1>;
```

The funny tag syntax is neither a string nor **HTML**. It is called **JSX**, and it is a *syntax extension* to **JavaScript**.

**JSX** produces [*"elements"*](#elements). It is *syntactic sugar* for the `React.createElement(type, props, ...children)` function.

We can embed any valid **Javascript** expression in **JSX** by putting it inside curly braces:

```javascript
const name = "Josh Perez";
const element = <h1>Hello, {name}</h1>;
```

Since **JSX** is closer to **JavaScript** than to **HTML**, **React DOM** uses *camelCase* property naming convention
instead of **HTML** attribute names. For example, `class` becomes `className` in **JSX**, and `tabindex` becomes
`tabIndex`.

## Components
Components let us split the UI into independent, reusable pieces, and think about each piece in isolation.

Conceptually, components are like **JavaScript** functions. They accept arbitrary inputs (called *"props"*) and return
[React elements](#elements) describing what should appear on the screen.

### Function components
The simplest way to define a component is to write a **JavaScript** function:

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

This function is a valid **React** component because it accepts a single *"props"* (which stands for properties) object
argument with data and returns a **React** element. We call such components *"function components"* because they are
literally **JavaScript** functions.

We can then use **JSX** to create a **React** element out of the component and render it, just like we would for regular
**DOM** tags:

```javascript
import { createRoot } from "react-dom/client";

const element = <Welcome name="Sara" />;

const root = createRoot(document.getElementById("root");
root.render(element);
```

When **React** sees an element representing a user-defined component, it passes **JSX** attributes and children to this
component as a single object. We call this object *"props"*.

We can pass an entire object in `props` to avoid passing many individual attributes. The **JavaScript** *spread
operator* (`...`) can be used as well.

The `children` property is available in every component and gives access to the component's children, i.e. the
elements/components between the opening and closing tag of the component.

One important thing to keep in mind is that we should **always start component names with a capital letter**. That's
because **React** treats components starting with lowercase letters as **DOM** tags.

If we want to register an event handler inside a component we can do so by using the relevant `on*` property:

```html
<button onClick={handleClick}>Submit</button>
```

#### Rendering arrays
We will often want to display multiple similar components from a collection of data. We can use the [**JavaScript array
methods**](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array#) to manipulate an array of
data and render lists of components from them.

For example, say that we have a list of people:

```html
<ul>
  <li>Creola Katherine Johnson: mathematician</li>
  <li>Mario José Molina-Pasquel Henríquez: chemist</li>
  <li>Mohammad Abdus Salam: physicist</li>
  <li>Percy Lavon Julian: chemist</li>
  <li>Subrahmanyan Chandrasekhar: astrophysicist</li>
</ul>
```

We can generate this list in **React** from an array of names like this:

```javascript
const people = [
  { id: "1", desc: "Creola Katherine Johnson: mathematician" },
  { id: "2", desc: "Mario José Molina-Pasquel Henríquez: chemist" },
  { id: "3", desc: "Mohammad Abdus Salam: physicist" },
  { id: "4", desc: "Percy Lavon Julian: chemist" },
  { id: "5", desc: "Subrahmanyan Chandrasekhar: astrophysicist" }
];

export default function List() {
  const listItems = people.map(person =>
    <li key={person.id}>{person.desc}</li>
  );

  return <ul>{listItems}</ul>;
}
```

The `key` attribute defined for each `li` item above helps **React** identify which items have changed, are added, or
are removed. Keys should be provided for all elements inside the array to give them a stable identity. If we omit them,
**React** will give us a warning.

#### Hooks
Hooks are a new addition in **React 16.8**. They let us use state and other **React** features without writing a
[class](#class-components).

- ##### `useState`
  Props are supposed to be *immutable* and used only to configure a component (e.g. static text to display, color, etc.).

  Even if we declare a variable inside our component and display its value with **React**, changes to that variable won't
  be tracked by **React** so the component won't get re-rendered.

  For dynamic components **React** has the concept of (*mutable*) state with the `useState` hook. It accepts the initial
  state value and returns an array with the current state value as the first element and a function for updating the state
  as the second element.

  For example:

  ```javascript
  import { useState } from "react";

  const [count, setCount] = useState(0);
  ```

  We can also pass a function to `useState` that returns the initial value for the state, effectively performing *lazy
  initialization*.

  The state is only created the first time a component renders. During the next renders `useState` gives us the current
  state. When the state changes, **React** re-renders the component as well as any child components that depend on that
  state.

  The update function returned by `useState` takes either the new value for the state (useful for resetting it) or a
  function to run. That function accepts the old state value as a parameter, and returns an updated value:

  ```javascript
  function Counter({ initialCount }) {
    const [count, setCount] = useState(initialCount);

    return (
      <>
        Count: {count}
        <button onClick={() => setCount(initialCount)}>Reset</button>
        <button onClick={() => setCount(prevCount => prevCount - 1)}>-</button>
        <button onClick={() => setCount(prevCount => prevCount + 1)}>+</button>
      </>
    );
  }
  ```

- ##### `useEffect`
  A *side effect* is any code that **React** is not in charge of handling, like code that has some kind of effect on an
  outside system (e.g. local storage, **API** websockets, two states we want to keep in sync with each other, etc.).

  **React** provides us with the `useEffect` hook for handling those types of *side effects*.

  `useEffect` accepts a function that contains *imperative*, possibly *effectful* code and by default runs it after every
  completed render. This means that, if that function sets the component's state, the component will keep getting
  re-rendered infinitely!

  We can pass a second parameter to `useEffect`, called the *dependencies array*, which controls if the effect will run
  after a render. Specifically, the effect will only run if any value in the *dependencies array* changes. Passing an
  empty array will run the effect only after the first render.

  The function we pass as the first parameter to `useEffect` can optionally return another function that will run after
  the component [unmounts](#lifecycle-methods) to perform any clean up needed by the effect. Because of this optional
  return value, the function passed to `useEffect` can't be asynchronous, since that would implicitly convert its return
  value to a `Promise`. It can, however, call an asynchronous function.

  **Example:**

  ```javascript
  useEffect(() => {
    const subscription = props.source.subscribe();

    return () => {
      // Clean up the subscription
      subscription.unsubscribe();
    };
  }
  ```

- ##### `useRef`
  The `useRef` hook lets us reference a value that's not needed for rendering. It is similar to `useState` but it **DOES
  NOT** trigger a re-render. It is usually used to target **DOM** nodes/elements.

  `useRef` returns an object with a single property, `current`. If we pass the ref object to **React** as a `ref`
  attribute to a **JSX** node, **React** will set its `current` property.

  The argument we pass to `useRef` is the value we want the ref object's `current` property to be initially. It can be a
  value of any type and is ignored after the initial render.

  **Example:**

  ```javascript
  // create a ref object with its `current` property set to `null`
  const divContainer = useRef(null);

  // make `divContainer.current` reference the `div` element
  <div ref={divContainer}>Hello</div>

  // set the text content of the `div` element through the ref object
  divContainer.current.textContent = "Hello";
  ```

- ##### `useReducer`
  For more complex state management **React** offers the `useReducer` hook. Its terminology and usage is similar to the
  [**Redux library**](https://redux.js.org/).

  `useReducer` accepts a *reducer* and an initial state as arguments and returns the current state and a *dispatch*
  function for changing that state in response to some interaction.

  The `reducer` specifies how the state gets updated. It must be pure, should take the state and action as arguments, and
  should return the next state. State and action can be of any types.

  The `dispatch` function returned by `useReducer` lets us update the state to a different value and trigger a re-render.
  We need to pass the action as the only argument to the `dispatch` function.

  **React** will set the next state to the result of calling the `reducer` function with the current `state` and the
  action we've passed to `dispatch`.

  **Example:**

  ```javascript
  const reducer = (state, action) => {
    if (action.type === "UPDATE_AGE") {
      return {
        age: action.payload
      };
    }

    throw new Error("Unknown action type!");
  };

  const [state, dispatch] = useReducer(reducer, { age: 42 });
  // ...
  dispatch({ type: 'UPDATE_AGE', payload: 43 });
  ```

- ##### `useContext`
  **React** provides us with the `useContext` hook for cases where we want to avoid passing data deeply into the component
  tree (also called *prop drilling*). It makes use of a **React** component called `Context`.

  `Context` lets a parent component make some information available to any component in the tree below it--no matter how
  deep--without passing it explicitly through props.

  We can create a context with the `React.createContext` function. Its only argument is the *default* value for the
  context:

  ```javascript
  import { createContext } from "react";

  const ThemeContext = createContext("light"); // "light" is the default value

  export default ThemeContext;
  ```

  To make the context available to the children of a component, we need to wrap them with a context provider:

  ```javascript
  import ThemeContext from "../contexts/ThemeContext";

  export default function Section({ children }) {
    return (
      <section className="section">
        <ThemeContext.Provider value="dark">
          {children}
        </ThemeContext.Provider>
      </section>
    );
  }
  ```

  This tells **React**: *"if any component inside this `<Section>` asks for `ThemeContext`, give them the value `dark`"*.
  The component will use the value of the nearest `<ThemeContext.Provider>` in the UI tree above it.

  Then, `useContext` can be called with the context as a parameter and will return its value:

  ```javascript
  import { useContext } from "react";
  import ThemeContext from "../contexts/ThemeContext";
  // ...
  const themeVariant = useContext(ThemeContext); // `themeVariant` is set to "dark"
  ```

- ##### `useMemo` & `memo`
  `useMemo` is a hook that lets us *cache* the result of a calculation between re-renders.

  It accepts the function calculating the value we want to cache and a list of all *reactive values* referenced inside
  that function. Reactive values include `props`, `state`, and all the variables and functions declared directly inside
  our component body.

  On the initial render, `useMemo` returns the result of calling the function with no arguments.

  During subsequent renders, it will either return an already stored value from the last render (if the dependencies
  haven't changed), or call the function again, and return the result that it has returned.

  **Example:**

  ```javascript
  import { useMemo } from "react";

  function TodoList({ todos, tab }) {
    const visibleTodos = useMemo(
      () => filterTodos(todos, tab),
      [todos, tab]
    );
    // ...
  }
  ```

  There is also a `memo` function which can be used to wrap a component to create a *memoized* version of it:

  ```javascript
  import { memo } from "react";

  const SomeComponent = memo(function SomeComponent(props) {
    // ...
  });
  ```

  This memoized version of the component will usually not be re-rendered when its parent component is re-rendered as long
  as its props have not changed. But **React** may still re-render it: memoization is only a performance optimization, not
  a guarantee.

- ##### `useCallback`
  `useCallback` is a hook that lets us *cache* a function definition between re-renders:

  ```javascript
  import { useCallback } from "react";

  export default function ProductPage({ productId, referrer, theme }) {
    const handleSubmit = useCallback((orderDetails) => {
      post("/product/" + productId + "/buy", {
        referrer,
        orderDetails,
      });
    }, [productId, referrer]);
    // ...
  }
  ```

  `useCallback(fn, deps)` is equivalent to `useMemo(() => fn, deps)`;

  `useCallback` runs during rendering; [`useEffect`](#useeffect) should be used for side effects instead!

  **React** is fast by default so these performance optimizations should be used very rarely, since they add their own
  cost. When they are used, we should not rely on them as a semantic guarantee: code should still work without them!

**React** also allows us to create our own custom hooks, as long as the name starts with the word `use` and uses
*camelCase*.

### Class components
We can also use an **ES6 class** to define a component:

```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

Class components must extend `React.Component` and are required to at least define the `render` method.

Every class that extends `React.Component` has access to the component properties via `this.props` so there is no need
to have a `props` parameter like with [function components](#function-components).

State must be an object, called `state`, and every piece of data that we want to save must be a property on that object:

```javascript
class App extends React.Component {
  state = {
    goOut: "Yes"
  };

  render() {
    return <h1>{this.state.goOut}</h1>;
  }
}
```

We can also define the state inside the constructor:

```javascript
constructor() {
  super();

  this.state = {
    goOut: "Yes"
  };
}
```

We can update a component's state by calling its `setState` method. It has the same signature as the function returned
by [`useState`](#usestate). When calling `setState` to update an object, we don't need to use the *spread operator*
(`...`) because **React** automatically calls `Object.assign` under the hood.

Class components aren't used as much anymore but a lot of older code still exists that uses them so it can be useful to
at least be familiar with them.

### Lifecycle methods
A **React** component's lifecycle has three phases:

- **Mount** (construction and render)
- **Update** (render)
- **Unmount** (removal from the **DOM**)

Each phase has a corresponding lifecycle method: `componentDidMount`, `componentDidUpdate` and `componentWillUnmount`.

The `render` method is called both after the component is first mounted and every time the props or state of the
component change. It can also be forcibly called with `forceUpdate`.

When a component is created, the following methods are called in order:

- `constructor()`
- `static getDerivedStateFromProps()`
- `render()`
- `componentDidMount()`

When a component gets re-rendered, the following methods are called in order:

- `static getDerivedStateFromProps()`
- `shouldComponentUpdate()`
- `render()`
- `getSnapshotBeforeUpdate()`
- `componentDidUpdate()`

The `componentDidUpdate` method can optionally accept the previous props and the previous state as parameters. This is
useful, for example, in order to avoid infinite loops when updating state, similar to the *dependency array* in
[`useEffect`](#useeffect).

### Controlled components
In **HTML**, form elements such as `<input>`, `<textarea>`, and `<select>` typically maintain their own state and update
it based on user input. In **React**, mutable state is typically kept in the `state` property of components, and only
updated with `setState()`.

We can combine the two by making the **React** state be the *"single source of truth"*. Then the **React** component
that renders a form also controls what happens in that form on subsequent user input. An input form element whose value
is controlled by **React** in this way is called a **"controlled component"**.

For example:

```javascript
const [firstName, setFirstName] = useState("");

return <input type="text" name="firstName" value={firstName} />;
```

**Remarks:**

- In **React**, `<select>` and `<textarea>` elements also have a `value` property for uniform handling of form elements
- Checkboxes, however, are handled differently because we care about their `checked`, not their `value` property

## Typechecking with PropTypes
We can use the `prop-types` package to run typechecking on the props for a component:

```javascript
import PropTypes from "prop-types";
import defaultImage from "assets/default-image.jpeg";

export default function Product({ image, name, price }) {
  return (
    <article className="product">
      <img src={image.url} alt={name} />
      <h4>{name}</h4>
      <p>${price}</p>
    </article>
  );
}

Product.propTypes = {
  image: PropTypes.object.isRequired,
  name: PropTypes.string.isRequired,
  price: PropTypes.number.isRequired,
};

Product.defaultProps = {
  name: "default name",
  price: 3.99,
  image: defaultImage,
};
```

## React Router
The [**React Router**](https://reactrouter.com/) library adds *client-side routing* capabilities to a **React**
application.

We can install it with `npm install react-router-dom`.

**Usage:**

```javascript
import { BrowserRouter as Router, Route, Switch } from "react-router-dom";

import Home from "./Home";
import About from "./About";
import User from "./User";
import Error from "./Error";

export default function App() {
  return (
    <Router>
      <Switch>
        <Route exact path="/">
          <Home />
        </Route>
        <Route path="/about">
          <About />
        </Route>
        <Route path="/user/:id" children={<User />}></Route>
        <Route path="*">
          <Error />
        </Route>
      </Switch>
    </Router>
  );
}
```

**React Router** defines the `useParams` hook that returns an object of key/value pairs of the dynamic params from the
current **URL**, that were matched by `<Route path>`:

```javascript
// for the `path="/user/:id"` shown above
const { id } = useParams();
```

Child routes inherit all `params` from their parent routes.

When creating a link to a route, we should use the `Link` component of **React Router**:

```javascript
import { Link } from 'react-router-dom';
// ...
<Link to="/">Home</Link>
```

Otherwise, the browser will do a full document request for the **URL** and refresh the page, instead of using **React
Router** to perform *client-side routing*.
