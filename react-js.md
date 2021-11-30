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







