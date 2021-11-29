### What is JSX?

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
const newElement = <h2>Hello, {() => formatUser(user)}</h2>
```

3. JSX can use curly braces within the HTML to pass variables to attributes.

```jsx
const element = <img src={user.avatarUrl}></img>
```



