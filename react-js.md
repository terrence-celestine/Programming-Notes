# What is JSX?

1. JSX is a mixture of javascript and html.
2. Elements in JS can be wrapped in curly braces inside of HTML.
   1. The HTML inside of the curly braces will be the value of the JS variable.

```jsx
const name = "Terrence";
const user = {
  firstName: "terrence",
  lastName: "celestine"
};
const formatUser = (user) => user.firstName + " " + user.lastName;
const element = <h1>Hello, {name}</h1>;
const newElement = <h2>Hello, {() => formatUser(user)}</h2>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

3. JSX can use curly braces within the HTML to pass variables to attributes.

```jsx
const element = <img src={user.avatarUrl}></img>
```

4. If a tag is empty you can immediately close it with a self closing tag.

```jsx	
const element = <img src={user.avatarUrl} />;
```

5. JSX can have children elements.

```jsx
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

# Rendering Elements

### Rendering an Element into the DOM

1. In react applications have a single root element.
2. DOM elements are added to the root element by passing it to ReactDOM.render()
3. Elements in react are immutable and the only way to update the UI is to replace the element with a new one.
4. React only updates the DOM when necessary.

```jsx
<div id="root"></div>

const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

# Function and Class Components

1. Components can be either a function or a class.

```jsx
function Welcome(props){
  return <h1>Hello, {props.name}</h1>;
}

class Welcome extends React.Component {
  render(){
    return <h1>Hello, {this.props.name}</h1>
  }
}
```



### Composing Components

1. High level components can have multiple smaller components.

```jsx
function Welcome(props){
  return <h1>Hello, {props.name}</h1>
}

function App(){
  return (
  	<div>
    	<Welcome name="Sara"/>
      <Welcome name="Kyle"/>
      <Welcome name="Tommy"/>
    </div>
  )
}

ReactDOM.render(
  <App/>,
  document.getElementById('root')
)
```



### Extracting components

1. Components that are made up of a bunch of different HTML elements should be split into smaller components.
2. This lets you build seperate smaller components that are re-usable.

```jsx
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}

function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}

function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```

### Props are Read-Only

- When you make a component (function or class) it must never modify it's own props.

### Component State

- State can be added to a function component using hooks - useState, useEffect
- State can be added to a class component using the constructor

```jsx
class Clock extends React.Component {
  constructor(props){
    super(props);
    this.state = {data: new Date()}
  }
  render() {
    return (
      <div>
        <h1>Hello, World</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    )
  }
}
```

### Lifecycle Methods

- Helps to free up resources when components are destroyed.
- Used within class based components.
- componentDidMount - runs after the component renders to the DOM.
- componentWillUnmount - runs before the component is removed from the DOM.

```jsx
class Clock extends React.Component {
  constructor(props){
    super(props);
    this.state = {date: new Date()}
  }
  
  componentDidMount(){
    this.timerID = setInterval(() => this.tick(), 1000)
  }
  componentWillUnmount(){
    clearInterval(this.timerID)
  }
  
  tick() {
    this.setState({
      date: new Date()
    });
  }
  
  render(){
    return (
    	<div>
      	<h1>Hello, world</h1>
        <h2>Is is {this.state.date.toLocaleTimeString()}</h2>
      </div>
    )
  }
}
```

### State

- A components state should not be modified directly.
- Use the setState function to update the state.
- setState accepts a function that receives the previous state and the props.
- calling setState takes the current state and merges it with the new values passed to it, any values not updated will persist.

```jsx
this.setState({comment: "Hello"});

this.setState((state,props) => ({
  counter: state.counter + props.increment
}));

componentDidMount() {
  // only posts is updated in this call, comments stays the same
  fetchPosts().then(response => {
    this.setState({
      posts: response.posts 
    })
  });
  // only comments is updated, posts stay the same
  fetchPosts().then(response => {
    this.setStat({
      comments: response.comments
    });
  });
}
```

### Handling Events

- events are named in camelCase e.g.
  - class -> className
  - onclick -> onClick
  - to prevent the default behavior of a element you need to pass the event to the function and call preventDefault.
  - to pass arguments to an event handler 

```jsx
function Form(){
  function handleSubmit(e){
    e.preventDefault();
    console.log('you clicked submit');
  }
  return (
    <form onSubmit={handleSubmit}>
      <button type="submit">Submit</button>
    </form> 
  )
}
```

```jsx
class LoggingButton extends React.Component {
  handleClick = () => {
    console.log('clicked');
  }
  render(){
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    )
  }
}
```

```jsx
<button onClick={(e) => this.deleteRow(e,id)}>Delete Row</button>
```

### Conditional Rendering

- Depending on the current state components can render different child components.

```jsx
class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }

  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }

  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }

  render() {
    const isLoggedIn = this.state.isLoggedIn;
    let button;
    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}

ReactDOM.render(
  <LoginControl />,
  document.getElementById('root')
);
```

### Inline If-Else with Condition Operator

- Use a ternary operator to render elements or JSX differences

```jsx
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}

render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn
        ? <LogoutButton onClick={this.handleLogoutClick} />
        : <LoginButton onClick={this.handleLoginClick} />
      }
    </div>
  );
}
```

### Preventing Component from Rendering

- If you want to hide a child component from rendering you can return null.
- Returning null does not prevent lifecycle methods from firing.

```jsx
function WarningBanner(props) {
  if (!props.warn) {
    return null;
  }

  return (
    <div className="warning">
      Warning!
    </div>
  );
}

class Page extends React.Component {
  constructor(props) {
    super(props);
    this.state = {showWarning: true};
    this.handleToggleClick = this.handleToggleClick.bind(this);
  }

  handleToggleClick() {
    this.setState(state => ({
      showWarning: !state.showWarning
    }));
  }

  render() {
    return (
      <div>
        <WarningBanner warn={this.state.showWarning} />
        <button onClick={this.handleToggleClick}>
          {this.state.showWarning ? 'Hide' : 'Show'}
        </button>
      </div>
    );
  }
}
```

### Lists and Keys

- Lists are elements that are almost identical. e.g.
  - ul > li
  - ol > li
- List items should have a key, keys help react know which items in a list have changed, been added, or removed. Each key should be unique.

```jsx
const numbers = [1,2,3,4,5];

function NumberList(props){
    const numbers = props.numbers;
    const listItems = numbers.map((number) => <li key={number.toString()}>{number}</li>);
    return (
        <ul>{listItems}</ul>
    )    
    ReactDOM.render(
        <NumberList numbers={numbers}/>,
        document.getElementById('root')
    );
}
```

### Controlled Component

- Forms typically maintain their own state. 
- React components can control a form and it's inputs by maintaining the state.
- The values displayed in the form will be maintained by state.

```jsx
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

### Handling Multiple Inputs

- When handling multiple inputs, give each input a name attribute and use the name attribute as the key in setState.

```jsx
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```

### Lifting State Up

- Parent components should be the source of truth for child components.
- Parent components should also pass a function to child components so the parent's state can be updated on change.
