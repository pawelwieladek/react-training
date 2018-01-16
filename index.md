title: React Trainging
author:
  name: Pawel Wieladek
  twitter: pawelwieladek
  url: http://pawelwieladek.com
style: public/style.css
output: public/index.html
controls: false
progress: true

---

# React Training

<img src="images/react-logo.png" width="200" />

---

# Part 1. ES6

<img src="images/js-logo.png" width="200" />

---

### Variables

```js
// hoisted
var a;
```
```js
// block-scoped
let b;
```
```js
// block-scoped
const b;
```

---

# Part 2. React

<img src="images/react-logo.png" width="200" />

---

### Hello, World!

```html
<div id="root"></div>
```

```js
import ReactDOM from 'react-dom';

ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

---

### JSX syntax

```js
const element = <h1 className="greeting">Hello, world!</h1>;
```

```js
const element = React.createElement(
  'h1',
  { className: 'greeting' },
  'Hello, world!'
);
```

#### Separation of conerns
Separate logic units instead of technologies.

#### JSX is a function call
```js
const element = <h1>Hello, world!</h1>;
```

```js
// preact example
const element = h('h1', 'Hello, world!');
```

#### Attributes

##### string

```html
<div className="container"><div>
```

##### boolean

```html
<button disabled></button>
// default true
<button disabled={false}></button>
```

##### number

```html
<textarea cols={6}></textarea>
```

##### function

```html
<input onChange={this.handleChange} />
```

##### any JS expression

```html
<input value={2+2} />
```

##### self-closing tag

```html
<input type="text" />
```

##### children

```html
<div>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</div>
```

##### spread operator

```js
const props = {
  title,
  onClick
};

const foo1 = <Component {...props} />
// is equal to
const foo2 = <Component title={title} onClick={onClick} />
```

#### JSX Prevents Injection Attacks
By default, React DOM escapes any values embedded in JSX before rendering them.

```js
<Header greeting={'<3'} />
// is equal to
<Header greeting="&lt;3" />
```

#### Expression as children

```jsx
<ul>
  {items.map((item) => <li key={item}>{item}</li>)}
</ul>
```

#### Function as children

```jsx
<ul>
  <Repeat times={3}>
    {(index) => <li key={index}>{index}</li>}
  </Repeat>
</ul>
```

#### Booleans, Null, and Undefined Are Ignored

```js
<div />

<div></div>

<div>{false}</div>

<div>{null}</div>

<div>{undefined}</div>

<div>{true}</div>
```

---

### Props

- ```props``` are **input data**.

- ```props``` are **read-only**.

- Components are **independent, reusable pieces**.

#### Component as a class

```js
class Header extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}
```

#### Component as a function

```js
function Header(props) {
  return <h1>Hello, {props.name}!</h1>;
}
```

#### Extraction

Split large piece of functionality into smaller components.

@todo: obrazek

---

### State

- Component must act like a **pure function**.

- UI is dynamic and **changes over time**.

- Component can have its own **state** to reflect the changes.

- Component **change state only by** ```setState()``` function

```js
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.increment = this.increment.bind(this);
    this.state = {
        count: 5
    };
  }

  increment() {
    this.setState({
        count: this.state.count + 1
    });
  }

  render() {
    return (
        <button onClick={this.increment}>{this.state.count}</button>
    );
  }
});
```

#### Binding

```js
this.increment = this.increment.bind(this);
```

#### The smallest representaion possible

```js
// bad practice
this.setState({
  firstName: firstName,
  lastName: lastName,
  fullName: `${firstName} ${lastName}`
});

// instead modify in render
render() {
  let fullName = `${firstName} ${lastName}`;
  return (
    <div>{fullName}</div>
  );
}
```

---

### Events

// komponent rodzic i dziecko

#### SyntheticEvent

---

### Data flow

- Components contains components forming a **tree**.
- Components pass data from the **top to the bottom**.

@todo: obrazek

---

### Virtual DOM

- Instead of manipulating DOM tree, React implements it's own tree of components
- React compares two component trees, finds changes and apply them on the DOM.

@todo: obrazek

---

### Lifecycle

```js
class Header extends React.Component {
  constructor(props) {
    super(props);
    // initial state
    this.state = {};
  }

  static defaultProps = {
    greeting: 'Hello'
  }

  static displayName = 'Header';

  render() {
    return <h1>Hello, world!</h1>;
  }
}
```

> Tip: [transform-class-properties](https://babeljs.io/docs/plugins/transform-class-properties/) lets you use ```static``` keyword.

#### Mounting

1. ```constructor()```
2. ```componentWillMount()```
3. ```render()```
4. ```componentDidMount()```

#### Updating

1. ```componentWillReceiveProps()```
2. ```shouldComponentUpdate()```
3. ```componentWillUpdate()```
4. ```render()```
5. ```componentDidUpdate()```

#### Unmounting

1. ```componentWillUnmount()```

---

### Conditional Rendering

```js
class Header extends React.Component {
  constructor(props) {
    super(props);
    this.showLine = this.showLine.bind(this);
    this.state = {
      lineVisible: false
    };
  }

  showLine() {
    this.setState({
      lineVisible: true
    });
  }

  renderLine() {
    if (this.state.formEnabled) {
      return <hr />;
    }
  }

  render() {
    let line = this.renderLine();
    return (
      <div>
        <button onClick={this.showLine}>Show line</button>
        {line}
      </div>
    );
  }
}
```

---

### Lists

Recall:

- JSX tag is a function call.

```js
class Header extends React.Component {
  render() {
    return (
      <ul>
        {this.props.items.map(item => {
          return <li key={item.id}>{item.text}</li>;
        })}
      </ul>
    );
  }
}
```

#### Keys
- Keys help React identify which items have changed
- Key has to be unique for the item among its siblings

```js
// bad practice
<ul>
  {items.map((item, index) =>
    <li key={index}>{item.text}</li>
  )}
</ul>
```

---

### Composition

```js
class Input extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(event) {
    // encapsulate <input> specific data structure
    this.props.onChange(event.target.value);
  }

  render() {
    return (
      <input
        value={this.props.value}
        onChange={this.handleChange}
      />
    );
  }
}
```

```js
class Button extends React.Component {
  render() {
    const largeClassName = this.props.size === 'large' ? 'btn-large' : null;
    const buttonClassNames = classNames('btn', largeClassName);
    return (
      <button className={buttonClassNames}>{this.props.children}</button>
    );
  }
}

class LargeButton extends React.Component {
  render() {
    return (
      <Button {...props} size="large" />
    );
  }
}
```

---

### Prop Types

Define component's API and validate input data.

```js
import PropTypes from 'prop-types';

class Header extends React.Component {
  static propTypes = {
    name: PropTypes.string.isRequired,
    onClick: PropTypes.func
  }

  static defaultProps = {
    return {
      onClick: () => {}
    };
  }

  render() {
    return (
      <div>
        <h1>Hello, {this.props.name}!</h1>
        <button onClick={this.props.onClick}>Click me</button>
      </div>
    );
  }
}
```

---

### Refs

- Typically to modify a child, you **re-render it with new props**. 
- **refs** gives an access to a DOM node.

```js
class Input extends React.Component {
  componentDidMount() {
    // once component is mounted, DOM node is present
    const domNode = ReactDOM.findDOMNode(this.refs.input);
    $.fancyPlugin(domNode);
  }

  render() {
    return (
      <input type="text" ref="input" />
    );
  }
}
```

---

### Style and classes

```js
class Header extends React.Component {
  render() {
    return (
      <button className="btn btn-primary" style={{
        color: 'red',
        padding: 20
      }}>
        Hello, world!
      </button>
    );
  }
}
```

---

### Fragments

- A common pattern in React is for a component to **return multiple elements**.
- Fragments let you **group a list of children without adding extra nodes** to the DOM.

```js
function ParametersList(props) {
  return (
    <dl>
      {props.parameters.map(parameter => (
        <React.Fragment key={parameter.id}>
          <dt>{parameter.name}</dt>
          <dd>{parameter.value}</dd>
        </React.Fragment>
      ))}
    </dl>
  );
}
```

---

### Context

- Context lets the data pass through the component tree **implicitly**

#### Passing data without context

```js
class A extends React.Component {
  render() {
    return (
      <button style={{ background: this.props.background }}>
        A
      </button>
    );
  }
}

class B extends React.Component {
  render() {
    return (
      <A background={this.props.background} />
    );
  }
}

class C extends React.Component {
  render() {
    return (
      <B background={this.props.background} />
    );
  }
}

class D extends React.Component {
  render() {
    return (
      <C background="f00" />
    );
  }
}
```

#### Passing data with context

```js
class A extends React.Component {
  static contextTypes = {
    background: PropTypes.string
  };

  render() {
    return (
      <button style={{ background: this.props.background }}>
        A
      </button>
    );
  }
}

class B extends React.Component {
  render() {
    return (
      <A />
    );
  }
}

class C extends React.Component {
  render() {
    return (
      <B />
    );
  }
}

class D extends React.Component {
  static childContextTypes = {
    background: PropTypes.string
  };

  getChildContext() {
    return { background: "f00" };
  }

  render() {
    return (
      <C />
    );
  }
}
```

---

### Higher-Order Components

- Higher-order component is a &&function that takes a component and returns a new component**.

```js
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

Example

```js
function withLoader(WrappedComponent) {
  class WithLoader extends React.Component {
    render() {
      // pass all props with spread operator
      return this.props.loaded ? (
        <WrappedComponent {...this.props} />
      ) : <div className="loader" />;
    }
  }
  // override displayName to indicate HoC usage
  const displayName = WrappedComponent.displayName || WrappedComponent.name || 'Component';
  WithLoader.displayName = `WithLoader(${displayName})`;
  // return new component
  return WithLoader;
}

@withLoader
class Welcome extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}!</h1>
    );
  }
}

class App extends React.Component {
  render() {
    return (
      <div>
        <Welcome loaded={false} name="Pawel" />
        <Welcome loaded={true} name="Pawel" />
      </div>
    );
  }
}
```

---

### Server-side Rendering

---

### Testing
