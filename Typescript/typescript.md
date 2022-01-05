# Typescript Notes

Learning with [Traversy Media](https://www.youtube.com/watch?v=BCg4U1FzODs).

## Basic Information

- Typescript is a superset of Javascript. It uses **static typing**, meaning variable types are known before compile time.
  - Javascript uses **dynamic typing**.
- Uses `.ts` and `.tsx` extensions
- **Typescript must be compiled into Javascript**with the typescript compiler (TSC)!!
- **tsconfig.js** file is used to help configure how you want your typescript to work, what you want TS to report, where you want your files to compile to, etc.

## Types

Javascript (JS) vs Typescript (TS):

- `let id = 5` vs `let id: number = 5`
  - setting `id = "6"` will give an error in TS, but not in JS
- **You set types by stating the type upon declaration**
  - typescript will infer the type if I don't explicitly state it
- set an array of items using brackets `[]` with declaration. Example:
  - `let numberArray = number[] = [1,2,3,4]`
- A **Tuple** data type is an array where you explicitly **state the data types for each item at each index**. Example:
  - `let person : [number, string, boolean] = [1, "2", false]`
  - A tuple defined with brackets `let person : [number, string, boolean][] = [1, "2", false]`, is an array of arrays where the subarrays are the defined tuples
- Unions allow data points to be two different types. Example:
  - `let id: string | number = 22`

## Compiling

- `tsc index` compiles index
- `"watch": "tsc --watch index"` will watch your file and report errors (or tsc --watch if config file has been edited)

## tsconfig.json

- run `tsc --init`
- `"outDir": "./dist",` Specifies an output folder for all emitted files
- `"rootDir": "./src",` Specifies the root folder within your source files
-
