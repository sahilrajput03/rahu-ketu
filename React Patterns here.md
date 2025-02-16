### Read article on thinking in react: @ reactjs.org

[link](https://reactjs.org/docs/thinking-in-react.html)

***



***

# React.js: 5 awesome packages you need to try out

https://medium.com/javascript-in-plain-english/5-awesome-react-packages-you-need-to-try-out-20a156d3d73e

***

### When you use <> and </> to wrap some JSX element in react, when parsed in html it gets vanished, you won't these empty braces in html in browser or the code that is produced by that component.

```js
      <>
        <tr>
          <td>{props.text}</td>
          <td>{props.filter}</td>
        </tr>
      </>

```



***

## Another importantreact patterns : https://dev.to/codeartistryio/the-react-cheatsheet-for-2020-real-world-examples-4hgg << this one looks damm pretty.

Another article on read states: [Link](https://medium.com/hackernoon/a-different-way-to-manage-state-in-react-2d21dfb94482)

## Another important react patterns :- https://vasanthk.gitbooks.io/react-bits/

***

# Below content is served from https://reactpatterns.com/

***

## Element

[Elements](https://reactjs.org/docs/glossary.html#elements) are anything inside angle brackets.

```jsx
<div></div>
<Greeting />
```

[Components](https://reactpatterns.com/#component) return Elements.

***

## Component

Define a [Component](https://reactjs.org/docs/glossary.html#components) by declaring a function that returns a React [Element](https://reactpatterns.com/#element).

```jsx
function Greeting() {
  return <div>Hi there!</div>;
}
```

***

## Expressions

Use curly braces to [embed expressions](https://reactjs.org/docs/introducing-jsx.html#embedding-expressions-in-jsx) in [JSX](https://reactjs.org/docs/glossary.html#jsx).

```jsx
function Greeting() {
  let name = "chantastic";

  return <div>Hi {name}!</div>;
}
```

***

## Props

Take `props` as an argument to allow outside customizations of your Component.

```jsx
function Greeting(props) {
  return <div>Hi {props.name}!</div>;
}
```

***

## defaultProps

Specify default values for `props` with `defaultProps`.

```jsx
function Greeting(props) {
  return <div>Hi {props.name}!</div>;
}
Greeting.defaultProps = {
  name: "Guest"
};
```

***

## JSX spread attributes

Spread Attributes is a feature of [JSX](https://reactjs.org/docs/introducing-jsx.html).
It's a syntax for providing an object's properties as JSX attributes.

Following the example from [Destructuring props](https://reactpatterns.com/#destructuring-props),
We can **spread** `restProps` over our ``.

```jsx
function Greeting({ name, ...restProps }) {
  return <div {...restProps}>Hi {name}!</div>;
}
```

***

## Merge destructured props with other values

Components are abstractions.
Good abstractions allow for extension.

Consider this component that uses a `class` attribute for style a `button`.

```jsx
function MyButton(props) {
  return <button className="btn" {...props} />;
}
```

This works great until we try to extend it with another class.

```jsx
<MyButton className="delete-btn">Delete...</MyButton>
```

In this case, `delete-btn` replaces `btn`.

Order matters for [JSX spread attributes](https://reactpatterns.com/#jsx-spread-attributes).
The `props.className` being spread is overriding the `className` in our component.

***

We need to use destructuring assignment to get the incoming `className` and merge with the base `className`.
We can do this simply by adding all values to an array and joining them with a space.

```jsx
function MyButton({ className, ...props }) {
  let classNames = ["btn", className].join(" ");

  return <button className={classNames} {...props} />;
}
```

To guard from `undefined` showing up as a className,
Use [default values](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Default_values_2).

```jsx
function MyButton({ className = "", ...props }) {
  let classNames = ["btn", className].join(" ");

  return <button className={classNames} {...props} />;
}
```

***

## Conditional rendering

You can't use if/else statements inside a component declarations.
So [conditional (ternary) operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) and [short-circuit evaluation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_Operators#Short-circuit_evaluation) are your friends.

### `if`

```jsx
{
  condition && <span>Rendered when `truthy`</span>;
}
```

### `unless`

```jsx
{
  condition || <span>Rendered when `falsy`</span>;
}
```

### `if-else`

```jsx
{
  condition ? (
    <span>Rendered when `truthy`</span>
  ) : (
    <span>Rendered when `falsy`</span>
  );
}
```

****

***

***

# **unread**

## Children types

React can render `children` from most types.
In most cases it's either an `array` or a `string`.

### `String`

```jsx
<div>Hello World!</div>
```

### `Array`

```jsx
<div>{["Hello ", <span>World</span>, "!"]}</div>
```

## Array as children

Providing an array as `children` is a very common.
It's how lists are drawn in React.

We use `map()` to create an array of React Elements for every value in the array.

```jsx
<ul>
  {["first", "second"].map(item => (
    <li>{item}</li>
  ))}
</ul>
```

That's equivalent to providing a literal `array`.

```jsx
<ul>{[<li>first</li>, <li>second</li>]}</ul>
```

This pattern can be combined with destructuring, JSX Spread Attributes, and other components, for some serious terseness.

```jsx
<ul>
  {arrayOfMessageObjects.map(({ id, ...message }) => (
    <Message key={id} {...message} />
  ))}
</ul>
```

## Function as children

React components don't support functions as `children`. However, [render props](https://reactpatterns.com/#render-prop) is a pattern for creating components that take functions as children.

## Render prop

Here's a component that uses a render callback.
It's not useful, but it's an easy illustration to start with.

```jsx
const Width = ({ children }) => children(500);
```

The component calls `children` as a function, with some number of arguments. Here, it's the number `500`.

To use this component, we give it a [function as `children`](https://reactpatterns.com/#function-as-children).

```jsx
<Width>{width => <div>window is {width}</div>}</Width>
```

We get this output.

```jsx
<div>window is 500</div>
```

With this setup, we can use this `width` to make rendering decisions.

```jsx
<Width>
  {width => (width > 600 ? <div>min-width requirement met!</div> : null)}
</Width>
```

If we plan to use this condition a lot, we can define another components to encapsulate the reused logic.

```jsx
const MinWidth = ({ width: minWidth, children }) => (
  <Width>{width => (width > minWidth ? children : null)}</Width>
);
```

Obviously a static `Width` component isn't useful but one that watches the browser window is. Here's a sample implementation.

```jsx
class WindowWidth extends React.Component {
  constructor() {
    super();
    this.state = { width: 0 };
  }

  componentDidMount() {
    this.setState({ width: window.innerWidth }, () =>
      window.addEventListener("resize", ({ target }) =>
        this.setState({ width: target.innerWidth })
      )
    );
  }

  render() {
    return this.props.children(this.state.width);
  }
}
```

Many developers favor [Higher Order Components](https://reactpatterns.com/#higher-order-component) for this type of functionality. It's a matter of preference.

## Children pass-through

You might create a component designed to apply `context` and render its `children`.

```jsx
class SomeContextProvider extends React.Component {
  getChildContext() {
    return { some: "context" };
  }

  render() {
    // how best do we return `children`?
  }
}
```

You're faced with a decision. Wrap `children` in an extraneous `` or return `children` directly. The first options gives you extra markup (which can break some stylesheets). The second will result in unhelpful errors.

```jsx
// option 1: extra div
return <div>{children}</div>;

// option 2: unhelpful errors
return children;
```

It's best to treat `children` as an opaque data type. React provides `React.Children` for dealing with `children` appropriately.

```jsx
return React.Children.only(this.props.children);
```

## Proxy component

*(I'm not sure if this name makes sense)*

Buttons are everywhere in web apps. And every one of them must have the `type` attribute set to "button".

```jsx
<button type="button">
```

Writing this attribute hundreds of times is error prone. We can write a higher level component to proxy `props` to a lower-level `button` component.

```jsx
const Button = props =>
  <button type="button" {...props}>
```

We can use `Button` in place of `button` and ensure that the `type` attribute is consistently applied everywhere.

```jsx
<Button />
// <button type="button"><button>

<Button className="CTA">Send Money</Button>
// <button type="button" class="CTA">Send Money</button>
```

## Style component

This is a [Proxy component](https://reactpatterns.com/#proxy-component) applied to the practices of style.

Say we have a button. It uses classes to be styled as a "primary" button.

```jsx
<button type="button" className="btn btn-primary">
```

We can generate this output using a couple single-purpose components.

```jsx
import classnames from "classnames";

const PrimaryBtn = props => <Btn {...props} primary />;

const Btn = ({ className, primary, ...props }) => (
  <button
    type="button"
    className={classnames("btn", primary && "btn-primary", className)}
    {...props}
  />
);
```

It can help to visualize this.

```jsx
PrimaryBtn()
  ↳ Btn({primary: true})
    ↳ Button({className: "btn btn-primary"}, type: "button"})
      ↳ '<button type="button" class="btn btn-primary"></button>'
```

Using these components, all of these result in the same output.

```jsx
<PrimaryBtn />
<Btn primary />
<button type="button" className="btn btn-primary" />
```

This can be a huge boon to style maintenance. It isolates all concerns of style to a single component.

## Event switch

When writing event handlers it's common to adopt the `handle{eventName}` naming convention.

```jsx
handleClick(e) { /* do something */ }
```

For components that handle several event types, these function names can be repetitive. The names themselves might not provide much value, as they simply proxy to other actions/functions.

```jsx
handleClick() { require("./actions/doStuff")(/* action stuff */) }
handleMouseEnter() { this.setState({ hovered: true }) }
handleMouseLeave() { this.setState({ hovered: false }) }
```

Consider writing a single event handler for your component and switching on `event.type`.

```jsx
handleEvent({type}) {
  switch(type) {
    case "click":
      return require("./actions/doStuff")(/* action dates */)
    case "mouseenter":
      return this.setState({ hovered: true })
    case "mouseleave":
      return this.setState({ hovered: false })
    default:
      return console.warn(`No case for event type "${type}"`)
  }
}
```

Alternatively, for simple components, you can call imported actions/functions directly from components, using arrow functions.

```jsx
<div onClick={() => someImportedAction({ action: "DO_STUFF" })}
```

Don't fret about performance optimizations until you have problems. Seriously don't.

## Layout component

Layout components result in some form of static DOM element. It might not need to update frequently, if ever.

Consider a component that renders two `children` side-by-side.

```jsx
<HorizontalSplit
  startSide={<SomeSmartComponent />}
  endSide={<AnotherSmartComponent />}
/>
```

We can aggressively optimize this component.

While `HorizontalSplit` will be `parent` to both components, it will never be their `owner`. We can tell it to update never, without interrupting the lifecycle of the components inside.

```jsx
class HorizontalSplit extends React.Component {
  shouldComponentUpdate() {
    return false;
  }

  render() {
    return (
      <FlexContainer>
        <div>{this.props.startSide}</div>
        <div>{this.props.endSide}</div>
      </FlexContainer>
    );
  }
}
```

## Container component

"A container does data fetching and then renders its corresponding sub-component. That’s it."—[Jason Bonta](https://twitter.com/jasonbonta)

Given this reusable `CommentList` component.

```jsx
const CommentList = ({ comments }) => (
  <ul>
    {comments.map(comment => (
      <li>
        {comment.body}-{comment.author}
      </li>
    ))}
  </ul>
);
```

We can create a new component responsible for fetching data and rendering the `CommentList` function component.

```jsx
class CommentListContainer extends React.Component {
  constructor() {
    super()
    this.state = { comments: [] }
  }

  componentDidMount() {
    $.ajax({
      url: "/my-comments.json",
      dataType: 'json',
      success: comments =>
        this.setState({comments: comments});
    })
  }

  render() {
    return <CommentList comments={this.state.comments} />
  }
}
```

We can write different containers for different application contexts.

## Higher-order component

A [higher-order function](https://en.wikipedia.org/wiki/Higher-order_function) is a function that takes and/or returns a function. It's not more complicated than that. So, what's a higher-order component?

If you're already using [container components](https://reactpatterns.com/#container-component), these are just generic containers, wrapped up in a function.

Let's start with our `Greeting` component.

```jsx
const Greeting = ({ name }) => {
  if (!name) {
    return <div>Connecting...</div>;
  }

  return <div>Hi {name}!</div>;
};
```

If it gets `props.name`, it's gonna render that data. Otherwise it'll say that it's "Connecting...". Now for the the higher-order bit.

```jsx
const Connect = ComposedComponent =>
  class extends React.Component {
    constructor() {
      super();
      this.state = { name: "" };
    }

    componentDidMount() {
      // this would fetch or connect to a store
      this.setState({ name: "Michael" });
    }

    render() {
      return <ComposedComponent {...this.props} name={this.state.name} />;
    }
  };
```

This is just a function that returns component that renders the component we passed as an argument.

Last step, we need to wrap our `Greeting` component in `Connect`.

```jsx
const ConnectedMyComponent = Connect(Greeting);
```

This is a powerful pattern for providing fetching and providing data to any number of [function components](https://reactpatterns.com/#function-component).

## State hoisting

[function-component](https://reactpatterns.com/#function-component) don't hold state (as the name implies).

Events are changes in state. Their data needs to be passed to stateful [container components](https://reactpatterns.com/#container-component) parents.

This is called "state hoisting". It's accomplished by passing a callback from a container component to a child component.

```jsx
class NameContainer extends React.Component {
  render() {
    return <Name onChange={newName => alert(newName)} />;
  }
}

const Name = ({ onChange }) => (
  <input onChange={e => onChange(e.target.value)} />
);
```

`Name` receives an `onChange` callback from `NameContainer` and calls on events.

The `alert` above makes for a terse demo but it's not changing state. Let's change the internal state of `NameContainer`.

```jsx
class NameContainer extends React.Component {
  constructor() {
    super();
    this.state = { name: "" };
  }

  render() {
    return <Name onChange={newName => this.setState({ name: newName })} />;
  }
}
```

The state is *hoisted* to the container, by the provided callback, where it's used to update local state. This sets a nice clear boundary and maximizes the re-usability of function component.

This pattern isn't limited to function components. Because function components don't have lifecycle events, you'll use this pattern with component classes as well.

*[Controlled input](https://reactpatterns.com/#controlled-input) is an important pattern to know for use with state hoisting*

*(It's best to process the event object on the stateful component)*

## Controlled input

It's hard to talk about controlled inputs in the abstract. Let's start with an uncontrolled (normal) input and go from there.

```jsx
<input type="text" />
```

When you fiddle with this input in the browser, you see your changes. This is normal.

A controlled input disallows the DOM mutations that make this possible. You set the `value` of the input in component-land and it doesn't change in DOM-land.

```jsx
<input type="text" value="This won't change. Try it." />
```

Obviously static inputs aren't very useful to your users. So, we derive a `value` from state.

```jsx
class ControlledNameInput extends React.Component {
  constructor() {
    super();
    this.state = { name: "" };
  }

  render() {
    return <input type="text" value={this.state.name} />;
  }
}
```

Then, changing the input is a matter of changing component state.

```jsx
return (
  <input
    value={this.state.name}
    onChange={e => this.setState({ name: e.target.value })}
  />
);
```

This is a controlled input. It only updates the DOM when state has changed in our component. This is invaluable when creating consistent UIs.

*If you're using [function components](https://reactpatterns.com/#function-component) for form elements, read about using [state hoisting](https://reactpatterns.com/#state-hoisting) to move new state up the component tree.*