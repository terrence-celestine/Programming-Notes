### What is TypeScript?

1. A compiler than converts TS to JS.
1. Offers type checking before compilation.
1. Uses static types

### Advantages

1. TS adds Types
2. Gives us access to decorators.
3. Configuration Options
4. Non-JS Features like Interfaces or Generics
5. Modern Tooling

### Course Outline

1. TypeScript Basics
2. Compiler & Configuration
3. Working with Next-gen JS code
4. Classes & Interfaces
5. Advanced Types & TypeScript Features
6. Generics
7. Decorators
8. Time to Practice - Full Project
9. Working with Namespaces & Modules
10. Webpack & TypeScript
11. Third-Party Libraries & TypeScript
12. React + TypeScript & NodeJS + TypeScript

### Core Types

1. Number - 1, 1.4, -10

2. Boolean - true, false

3. String - 'Hi', "Hi", ` Hi `

   Variable types can passed when instantiating a varaible. Variables cannot be assigned to different types. e.g. If a variable has teh type number it can't be assigned to a string value.

4. Arrays - can have a specific type passed before the array declaration.

5. Tuples - An array with a fixed length and list which type of data it contains at each position.

6. Union Types - Let's an argument be different types, types can be switched by re-assigning variable.

7. Literal Types - 

8. Type alias/Custom Type - variables that point to core type, union types, or literal types. Can be useful for complex types and make code more readable.

   

```tsx
// basic type
const number1:number = 5;
// array
const favoriteHobbies: string[];
// tuple 
const roleIDPair = [number, string];

// union type
function combine(input1: number | string, input2: number | string){
  let result;
  if (typeof input1 === "number" && typeof input2 === "number"){
    result = input1 + input2;
  } else {
    result = input1.toString() + input2.toString();
  }
  return result;
}

// type alias
type User = { name: string, age: number};
const u1: User = { name: "terrence", age: 32};

function greet(user: User){
  console.log("Hi, i am " + user.name);
}
```

### Return Type: Void

1. A function is void when it returns nothing.

#### Function Type

1. Is a type that describes how the function assigned to a variable's parameters and the return value type's should look.

```ts
let combineValues = (a:number, b: number) => number;
```

### Unknown Type

1. A typesafe version of any. Any value is assignable to it. 
2. No operations are permitted on an unknown value without doing an if check.

### The Compiler

1. Use the --watch command in the CLI to auto refres whenever a change is made.
2. Use tsc --init will tell the compiler that the entire folder should be managed by typescript.
   1. Then use tsc --watch to watch any changes in the folder, and reload.
   2. This will create a tsconfig.json file.

### TS Config File

1. "exclude" - takes an array of file names that should be ignored.

   1. ```js
      "exclude": ["filename.ts", "folder_name"]
      ```

2. By default some folders are ignored such as "node_modules"

3. "include" - takes an array of file names that should be compiled.

4. "files" - takes an array of files that should be compiled.

5. target - tells typescript which version of JS it should compile down to. e.g. es5, es2015, etc.

6. allowJS - 

7. checkJS - 

8. sourceMap - helps with debugging and development. Generates .js.map files

9. rootDir -  tells the compiler where the TS files are located.

10. outDir - tells the compiler where to put our compiled JS files, replicates subfolder structure.

11. removeComments - removes all comments from compiled files.

12. noEmit - 

13. noEmitOnError - tells the compiler to stop compiling all files if an error occurs during compilation in any file.

14. strict - Enables all strict type checking.

15. noImplicityAny - makes sure that the value's we use in TS are valid.

16. noUsedLocals - makes sure all variables are used.

17. noUnUsedParams - makes sure all params are used.

18. noImplicitReturns - makes sure functions return something in all cases.

    1. Used inside of if/else statements

### Classes

1. Classes are like blue prints for an object. 
2. Classes have a constructor function that takes the arguments passed to the function and sets them on the object instance.

```js
class Department {
  name: string
  constructor(n: string){
    this.name = n;
  }
  describe(){
    console.log("Department: " + this.name);
  }
}

const accounting = new Department('Accounting');

```

### This Keyword

1. The "this" keyword is used for referencing the class instance.
2. Use arrow functions to scope the "this" keyword.

```js
class Department{
  name: string
  constructor(n: string){
    this.name = n;
  }
  describe = () => {
    console.log("Department" + this.name)
  }
}
```

### Public/Private Keyword

1. Adding the private keyword infront of a class property makes it so the property can only be accessed by the class.
2. Use private keyword to protect any data that shouldn't be modified unless.
3. Properties and methods are considered "public" by default.

```js
class Department {
    public name: string;
    private employees: string[] = [];
    constructor(n:string){
        this.name = n;
    }
    describe = () => {
        console.log("Department: " + this.name)
    }
    addEmployee = (employee:string) => {
        this.employees.push(employee);
    }
    printEmployeeInformation = () => {
        console.log(this.employees.length);
        console.log(this.employees)
    }
}
```

### Read Only Keyword

1. Useful for when you create an object with immutable props.

```js
class Department {
  readonly name: string;
	private employees: string[] = [];
  constructor(n:string){
    this.name = n;
  }
  describe = () => {
    console.log("Department" + this.name);
  }
  addEmployee = (employee:string) => {
    this.employees.spush(employee);
  }
  printEmploeeInformation = () => {
    console.log(this.employees.length);
    console.log(this.employees);
  }
}
```

