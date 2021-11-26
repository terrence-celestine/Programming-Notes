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

4. Tuples - An array with a fixed length and list which type of data it contains at each position.

```tsx
// basic type
const number1:number = 5;
// array
const favoriteHobbies: string[];
// tuple 
const roleIDPair = [number, string];

```

