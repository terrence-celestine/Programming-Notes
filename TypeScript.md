## Types

### Basic Types

- **Number** - A number just like in JavaScript including decimals.
- **String** - A string using single, double quotes or backticks.
- **Boolean** - Either true or false.
- **Date** - The normal date object like in JavaScript. 

```tsx
interface User {
    name: string;
    age: number;
    isActive: boolean;
}

const userName: string = "Terrence";
const userAge: number = 33;
const isActiveOnGithub: boolean = true;

const Person: User = {
    name: userName;
    age: userAge;
    isActive: isActiveOnGithub;
}

console.log(Person); // returns {name: 'Terrence', age: 33, isActive: true}

```

### Interfaces 

- Describe the types for an object.

```tsx
interface Person {
    name: string;
    age: number;
}

const Terrence: Person = {
    name: "Terrence",
    age: 26
}

console.log(Terrence); // returns {name: "Terrence", age: 26}
```

### Composing Types

- New types can be create by combining simpler ones.

#### Union Types

- Sometimes a type can be one of many types.

```tsx
type isActive = true | false;
```

#### Function return types

- When a function has a return type defined it tells Typescript what value should be returned when it's ran. It helps with strictness.

```tsx
interface User {
  name: string;
  age: number;
  isActive: true | false;
}

const userName: string = "Terrence";
const userAge: number = 33;
const isActiveOnGithub: boolean = true;

const Terrence: User = {
  name: userName,
  age: userAge,
  isActive: isActiveOnGithub,
};

const getUserName = (user: User): string => {
  return user.name;
};

console.log(getUserName(Terrence)); // returns Terrence
```

#### Function Parameters

- A functions parameters should be described with either an interface or each item should be broken down.
- Parameters can be option by passing the "?"

```tsx
const getUserName = (user: {
  name: string;
  age: number;
  isActive: boolean;
}): string => {
  return user.name;
};

const getUserName = (user: {
	name: string;
    age?: number;
	isActive?: boolean;
}): string => {
    return user.name;
}
```

- Parameters can be a Union Type, which let's the parameter be one of multiple types.
  - You can tell the function what to do based on the type

```tsx
const printID = (id: string | number) => {
    if (typeof id === "string"){
        console.log(id.toUpperCase());
    } else {
        console.log(id);
    }
}

const greetPeople = (user: string[] | string) => {
    if (Array.isArray(x)){
        console.log("Hello, " + user.join(" and "));
    } else {
        console.log("Welcome " + user);
    }
}

greetPeople(["Eric","Kevin","John"]); // returns HEllo, Eric and Kevin and John
greetPeople("Terrence"); // returns Welcome Terrence
printAge("abc123"); // returns ABC123
printAge(123); // returns 123
```

