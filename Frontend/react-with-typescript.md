# React With Typescript

I'm following [this](https://youtube.com/playlist?list=PLC3y8-rFHvwi1AXijGTKM0BKtHzVC-LSK) tutorial, **but for future reference take a look at the [Typescript cheatsheet](https://github.com/typescript-cheatsheets/react)**. Thank you Codevolution (and typescript-cheatsheets) for your awesome education materials!

1. Use `.ts` and `.tsx` files instead of `.js` and `.jsx` files

## Typing Props

1. When you pass props to a child component, the props' data types must be predefined within the child component. Example:

```
Haha lol.
```

## Components

- You can type React Functional Components (RFC) with in different ways:

```
const Header = () => {
  return <header>{`header content`}</header>
}
```

**VS**

```
const Header: React.FC = () => {return <header>{`header content`}</header>};
```

**VS**

`function Header(): React.ReactElement {}`

**VS**

`function Header(): JSX.Element {}`

There's a great discussion on declaring React functions [here](https://github.com/typescript-cheatsheets/react#function-components). Declarations can be done either normally, with `JSX.Element`, or as inline type declarations. But lots of people say to just avoid `React.FC` altogether

## State

- When you initialize state with `useState()` you can declare the types that State can be as well
  - If you don't initialize state type, then the type will be inferred (null if nothing is defined)
- Here's how you can initialize state:

```
import { useState } from react;

const index = () => {
  const [state, setState] = useState<string | number>(""); // state can only be of type string or number
}
```
