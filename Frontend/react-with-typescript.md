# React With Typescript

I'm following [this](https://youtube.com/playlist?list=PLC3y8-rFHvwi1AXijGTKM0BKtHzVC-LSK) tutorial. Thank you Codevolution for your awesome education materials!

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
