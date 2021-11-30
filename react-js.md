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

- 

