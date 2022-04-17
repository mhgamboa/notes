# Typescript Notes

Learning with [Traversy Media](https://www.youtube.com/watch?v=BCg4U1FzODs).

## Basic Information

- Typescript is a superset of Javascript. It uses **static typing**, meaning variable types are known before compile time.
  - Javascript uses **dynamic typing**
- Uses `.ts` and `.tsx` extensions
- **Typescript must be compiled into Javascript** with the typescript compiler (TSC)!!
- **tsconfig.js** file is used to help configure how you want your typescript to work, what you want TS to report, where you want your files to compile to, etc.

## Types

Javascript (JS) vs Typescript (TS):

### Basic Types

- Declarations
  - **You set types by stating the type upon declaration**
  - `let id = 5` vs `let id: number = 5` (or just `let id: number;`)
    - setting `id = "6"` will give an error in TS, but not in JS
  - typescript will infer the type if I don't explicitly state it
- **Unions** allow data points to be two different types. Example:
  - `let id: string | number = 22` is valid
  - `let id: string | number = "twenty two"` is also valid
- You can set up variables to have **any** data type with the `any` and `unknown` keywords. Example: `let id: any;`
  - Differences between `any` and `unknown`:
    - You can't assign a variable with type `unknown` to another variable
    - You can access properties of `any`, you can't access properties of `unknown`. This is to prevent errors
    - **`any` is saying I don't care. `unknown` is saying I don't know**

### Arrays

- set an array of items using brackets `[]` with declaration. Example:
  - `let numArray: number[] = [1,2,3,4]`
    - `numArray` can only contain numbers
  - `let stringArray: string[];`
    - `stringArray` can only contain strings
  - For an array of types: `let personArray: person[];`
    - `stringArray` can only contain the `type` of person
- A **Tuple** data type is an array where you explicitly **state the data types for each item at each index**. Example:
  - `let personTuple : [number, string, boolean] = [1, "2", false]`
  - To define an array where each array item is a tuple: `let subArrayTuple : [number, string, boolean][] = [1, "2", false]`

### Objects

- Technically you can define an object as `let person: object;`, but this doesn't let you add typing to each key.
- A better way to define objects (AKA classes) is as a **type** keyword, or as an **interface**
- It is perfectly ok for types to extend interfaces, and vice versa (see below on how to extend)

#### Types

1. Defined as a capital letter
2. to make a property optional add a question mark in its definition

```
// Definition
type Person = {
  name: string;
  age: number;
  hobbies?: string[];
}

// Creation
let me:Person = {
  name: "Marcus",
  age: 28
}
```

- Types can also be extended:

```
// Definition of X
type X = {
  a: string;
  b: number;
}

// Creation of Y, which includes definition of X
type Y = x & {
  a: string;
  b: number;
}
```

#### Interfaces

```
// Definition
interface Person {
  name: string;
  age: number;
  hobbies?: string[];
}

// Creation
let me: Person = {
  name: "Marcus",
  age: 28
}
```

- Interfaces can also be extended:

```
// Definition of X
interface Person {
  name: string;
  age: number;
}

// Creation of Guy, which includes definition Person
interface Guy extends Person {
  facialHair?: boolean;
}
```

## Functions

1. **Function parameters must be typed** or Typescript will give you an error
   - `const printName = (name) => console.log(name);` -> not valid
   - `const printName = (name: string) => console.log(name);` -> is valid
2. Functions can be typed as well
   - Technically you can define a function as `let functionName: Function;`, but this doesn't let you type the parameters, or the type of data returned
   - A better way to define functions as `functionName: (variable: type) => typeReturned`
     - Example: `printName: (name: string) => void` (We use void since nothing is returned)
     - Example2: `const minus = (a: number, b: number): number;` (Must take numbers as parameters and return a number)
     - We use `void` when a function returns undefined
     - We use `never` when a function doesn't return anything (throwing an error, or if defined return types have become impossible to return)

## Compiling

- `tsc index` compiles index
- `"watch": "tsc --watch index"` will watch your file and report errors (or tsc --watch if config file has been edited)

## tsconfig.json

- run `tsc --init`
- `"outDir": "./dist",` Specifies an output folder for all emitted files
- `"rootDir": "./src",` Specifies the root folder within your source files
-
