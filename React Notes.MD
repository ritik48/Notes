#### What is React ?

React is a Javascript library for building user interfaces. Its is

-   extremely popular
-   Component based
    -   components are the building blocks of user interfaces in React
    -   we build complex uis in react by building and combining different components
    -   A React component can be anything in your web application like a Button, Text, Label, or Grid.
-   Declarative

    -   We describe how components look like and how they work using a declarative syntax called JSX
    -   Declarative: telling react what a component should look like, based on current data/state.
    -   React is abstraction away from DOM: we never touch DOM

    -   JSX: a syntax that combines Html, Css, Javascript as well as referencing other components.

-   State-driven
    -   State is basically data and based on this data react renders the UI
    -   when state/data changes then we manually update the state in app and React automatically re-render the updated ui
    -   React **REACTS** TO STATE CHANGES BY RE-RENDERING THE UI

> #### React is a libraray Beacause React is only the view layer. We still need to pick multiple external libraries to build a complete application.

**While complete frameworks built on top of React are**

-   **NextJs**
-   **Remix**

-   Created in 2011 by Facebook

##### Two ways for setting up a React Project

-   ###### Create-React-App

    -   Complete **'starter kit'** for React application
    -   Everything is already configured: ESLint, Prettier, Jest, e.t.c.
    -   Uses **slow and outdated** technologies (i.e. webpack)

    -   though we can use it for creating projects for practice
        but should **not be used for real-world app** as it's slow and uses outdate technologu on the back

-   ##### Vite

    -   **Modern build tool** that contains a template for setting up React applications.
    -   But need to **manually set up ESLint (and others)**

    -   Used **for modern real-world apps**
    -   **Extremely fast** hot module replacement (HMR) and bundling.

#### Using create-react-app to create a project

`npx create-react-app project_name`

> React started code index.js

```
import React from "react";
import ReactDom from "react-dom/client";

function App() {
  return <h1>Hello React !!!</h1>;
}

const root = ReactDom.createRoot(document.getElementById("root"));
// root.render(<App />);


// StrictMode is a React Developer Tool primarily used for
// highlighting possible problems in a web application.
// In order to identify and detect any problems within the application and
// show warning messages, StrictMode renders every component inside the application twice.

root.render(
    <React.StrictMode>
        <App />
    </React.StrictMode>
)
```

### Components

-   Basic structure :

    -   Data
        -   Props
        -   State
    -   Logic
    -   Appearance

-   React applicatios are entirely made out of components
-   Building blocks of user interfaced in React
-   Piece of UI that has it's own data, logic, and appearance
-   We building coomplex UIs by building multiple components and combining them.
-   Components can be reused, nested inside each other, and pass data between them.

### JSX

-   **Declarative syntax** to **describe** what components **look like** and **how they work**.

-   **Components must return** a block of **JSX**
-   **Extension of JavaScipt that allows us to embed JavaScript, CSS, and React components into HTML.**

-   JSX is converted to javascript using a tool called Babel. it is automatically included in project when creating a project.

    Each JSX element is **converted** to a React.createElement function call.

e,g.

```
<header>
  <h1 style="color: red">Hello React !!!</h1>
</header>

converted to ->

React.createElement(
  'header',
  null,
  React.createElement(
    'h1',
    { style: { color: 'red' } },
    'Hello React!'
  )
);

```

This conversion is needed as browers don't understand JSX syntax.

means we **could use React without JSX**, but this would **mean to write a lot of code.**

### JSX is Declarative

First let's see what **_imperative_** means.

-   Imperative (How to do things)

    -   Manual DOM element selection and DOM traversing
    -   step-by-step DOM mautation until we reach the desired UI

-   Declarative (What we want)
    -   Decribe what UI should look like using JSX, based on current data
    -   React is an abstraction away from DOM: we never touch the DOM.

### Props

-   **Way to pass data** between components specifically from a parent component to a child component.

-   With props, **parent components control** how **child components** look and work

-   **Anything** can be passed as props: single values, arrays, objects, functions, even other components

e.g, let's pass data from _menu_ component to _pizza_ component.

```
function Menu() {
    return (
        <div className="menu">
            <h2>Menu</h2>
            <Pizza />
            <Pizza />
            <Pizza />
            <Pizza />
        </div>
    )
}


function Pizza() {
  return (
    <div>
      <img src="pizzas/spinaci.jpg" alt="pizza spinaci"></img>
      <h3>Pizza Spinaci</h3>
      <p>Tomato, mozarella, spinach, and ricotta cheese</p>
    </div>
  );
}
```

_passing data_

```
function Menu() {
    return (
        <div className="menu">
            <h2>Menu</h2>
            <Pizza
            photoName="photo/piza.jpg"
            name="Pizza Spinaci"
            ingredients="Tomato. spinach"
            price="10"
            />
        </div>
    )
}

function Pizza(props) {

  console.log(props)
  // Javascript object
  {
    photoName: 'photo/piza.jpg',
    name: 'Pizza Spinaci', ingredients: 'Tomato. spinach',
    price: '10'
  }

  return (
    <div>
      <img src="{props.photoName}" alt="{props.name}"></img>
      <h3>{props.name}</h3>
      <p>{props.ingredients}</p>
    </div>
  );
}
```

#### Props are READ-ONLY !

-   Components Basic structure :
    -   Data
        -   Props
        -   State
    -   Logic
    -   Appearance

Props is data coming from the **outside**, and can **only be updated** by the **parent element**

State is internal data that can be updated by the component's logic

-   Props are read-only, they are **immutable!** This is one of React's strict rules.

-   If you need to mutate props, you actually **need state**

##### Why ?

-   As props are actually JavaScript objects, Mutating props would affect parent, creating **side effects**

-   Components have to be **pure functions** in terms of props and state.
    i.e, component should not manipulate external data
    lie:

    ```
    const x = 7;
    function Fun() {
      x=6; // -> violates components being pure function

      return <h1>hello</h1>
    }
    ```

    -   This allows React to optimize apps, avoid bugs, make apps predictible.

#### ONE-WAY data flow

-   React follows one-way data flow.
-   data can only be passed from parent to child components which is done by props.

-   makes applications more predictable and esier to understand

-   makes applications, easier to debug, as we have more control over the data

-   is more performant.

### RULES OF JSX

-   JSX works essentially like HTML, but we can enter **"JavaScript mode""** by using {} _(for text or attribute)_

-   We can place \*\*JavaScript expressions inside {}.
    Examples: reference variables, create arrays or objects, [].map(), ternary operator

-   Statements are **not allowed** (if/else, for, switch)

-   JSX produces a **JavaScript expression**
    const el = <h1>Hello React !!!</h1>

    _converts to_

    const el = React.craeteElement("h1", null, "Hello React !!!");

    _and vice versa_

    -   We can place **other pieces of JSX** inside {}
    -   We can write JSX **anywhere** inside a component (in if/else, assign to variables, pass it into functions)

-   A piece of JSX can only have **one root element.** If you need more, use <React.fragment> (or the short <>)

#### Difference Between JSX and HTML

-   _className_ instead of HTML's _class_

-   _htmlFor_ instead of HTML's _for_

-   Every tag needs to be closed. Examples: <img /> or <br />

-   All event handlers and other properties need to be **camelCased**. e.g, _onClick_ or _onMouseOver_

-   **Exception**: aria-_ and data-_ are written with dashes like in HTML

-   CSS inline styles are written like this: **{{style}}**
    (to reference a variable, and then an object)

    e.g., `{{color:"red"}}`

-   CSS property names are also **camelCased**

-   Comments need to be in {} (because they are JS)

---

Note:

inside {} we caanot write if/else r forEach because these two don't prodcuces value. Therefore only those javascript are written that propuces a value.

like: array.map(x => x.name) // this returns a modified array therefore is valid.

like wise instead of if/else, we can use:

-   conditionals (&& or ||)
-   ternary

---

### React Fragment

```
{pizzaLength > 0 ? ( -> line 'A'
  <p>
    Authentic Italian cuisine. 6 creative dishes to choose from. All from
    our stone oven, all organic, all delicious
  </p>

  <ul className="pizzas">
    {pizzaData.map((pizza) => (
      <Pizza data={pizza} key={pizza.name} />
    ))}
  </ul>
) : (
  <p>We're still working on our menu. Please come back later :)</p>
    )}
```

In the above line 'A' , the JSX is returning two elements, while we know that a JSX can have only one root element.
S, above will give error.

So, to return more than one root element, we use fragment.

we enclose the elements we want to return in **<> </>**

e.g.,

```
{pizzaLength > 0 ? ( -> line 'A'
  <>
    <p>
      Authentic Italian cuisine. 6 creative dishes to choose from. All from
      our stone oven, all organic, all delicious
    </p>

    <ul className="pizzas">
      {pizzaData.map((pizza) => (
        <Pizza data={pizza} key={pizza.name} />
      ))}
    </ul>
  </>
) : (
  <p>We're still working on our menu. Please come back later :)</p>
    )}

```

Instead of enclosing within <> </>, we can also use <React.Fragment> </React.Fragment>

#### Conditionally setting classNames

`<li className={`pizza ${props.data.soldOut ? 'sold-out' : ''}`}>`

#### Event-listener in React

-   we use `onClick` i.e, inline html attribute

### What is State ?

-   Data that a component **can hold over time**, necessary for information that it needs to remenber throughout the app's life cycle.

-   Used to manage the data inside a component itself.
-   It is both read and write unlike props which is _read-only_

-   **Component's memory**

-   **Component state**: Single local component variable("Piece of state", "state variable")

-   Updating **component state** triggers React tp **re-render the component**

#### How to use State ?

-   Creating a state variable

    -   We use `useState()` function which is imported from React
    -   `const [step, setStep] = useState(1)`
        here 1, is basically the initial value of the state ariable `step` and `setStep` is the function that `useState` gives to update the state

-   To change the state variable `step` we will use `setStep` , like this -> `setStep(step + 1)`

> **useState()** is called **hook** in React.

> all the react function which start with `use` are `hooks`

> we can only call `hooks` like `useEffect()` on the top level of function not inside `if-statement` or inside other function or inside loop.

> We should `only update state variable using the setter function`, not manually.

> When a `state` changes , then react `re-renders` the components which will result in new `updated component view`, **but it preserves the `state` in the process**.

> 👉*React* is called "React" because...
> React **Reacts** to state changes by re-rendering the UI

-   It's better to use callback function when we want to change state based on current state

    e.g.,

    `const [step, setStep] = useState(1);`

    `setStep((s) => s+1);`

-   Each component has and manages `its own state`, no matter how many times we render the same component

-   Multiple instances of the same component are independent of each other

#### Practical Guidelines About State

-   Use a state variable for any data the component should keep track of ("remember") over time. **This is data that will change at some point**

-   Whenever you want something in the component to be **dynamic**, create a piece of state related to that "thing", and update the state when the "thing" should change.

-   When building a component, imagine its view as a **reflection of state changing over time.**

-   For data that should not trigger component re-renders, don't use state. Use a regular variable instead.

#### Global State vs Local State vs Derived State

-   Local State

    -   State needed **only by one or a few components**
    -   State that is defined in a component and **only that components and child components** have access to it(by passing via props)

-   Global State

    -   State that **many component s** might need
    -   **Shared** state that is accessible to **every component** in the entire application

    Achieved using:
    React Context API and
    Redux(external global state management library)

#### Lifting State up

Its possible that two components may require same state, and if there is a parent child relationship between the components then we can pass the state as props to the child.

But if the two components are siblings, then we need to lift the state up, meaning we have to put the state in the first common parent component.

### Controlled Elements

-   By default, the state of the input fields are owned by DOM so its difficult to get their values.

-   So we use controlled elements in react to control the states of input field

-   So controlled elements are basically the elements whose values are defined by the state and it also has an event handler which listens for the change and updates the state accordingly.

##### Derived State
  - State that is computed from an existing piece of state or from props.


### Children Prop

- When we use any component, then instead of closing it immediately, we can use it like any normal html tags like `div` which has both opening and closing tags `<div>` and `</div>`

Now in case of components, the values between the opening and closing are considered as, **children props** that can be passed. And this children props can be accessed in the component using special keyword `children`

```

<Button bgColor="white"
        color="red"
        onClick={onClick}
> 
<span>👉 Next </span> ------> line 1
</Button>

function Button({ bgColor, textColor, onClick, children }) {
    return (
        <button
            style={{ color: textColor, backgroundColor: bgColor }}
            onClick={onClick}
        >
            {children}
        </button>
    );
}

here, in children, line 1 will be passed
```

- The children prop **allow us to pass JSX into an element**
- Essential tool to make **reusable** and **configurable** components
- Really **useful for generic components** that don't know their content before being used.
- It's great to reduce number of props passed



