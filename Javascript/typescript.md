# Typescript Notes

Learning with [Traversy Media](https://www.youtube.com/watch?v=BCg4U1FzODs).

## Basic Information

- Typescript is a superset of Javascript. It uses **static typing**, meaning variable types are known before compile time.
  - Javascript uses **dynamic typing**.
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
- Unions allow data points to be two different types. Example:
  - `let id: string | number = 22`

### Arrays

- set an array of items using brackets `[]` with declaration. Example:
  - `let numArray: number[] = [1,2,3,4]`
    - `numArray` can only contain numbers
  - `let stringArray: string[];`
    - `stringArray` can only contain strings
- A **Tuple** data type is an array where you explicitly **state the data types for each item at each index**. Example:
  - `let person : [number, string, boolean] = [1, "2", false]`
  - To define an array where each array item is a tuple: `let person : [number, string, boolean][] = [1, "2", false]`

### Objects

- Technically you can define an object as `let person: object;`, but this doesn't let you add typing to keys.
  Objects can be defined with the `type` keyword, or as an **interface**

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

#### Interfaces

## Compiling

- `tsc index` compiles index
- `"watch": "tsc --watch index"` will watch your file and report errors (or tsc --watch if config file has been edited)

## tsconfig.json

- run `tsc --init`
- `"outDir": "./dist",` Specifies an output folder for all emitted files
- `"rootDir": "./src",` Specifies the root folder within your source files
-
