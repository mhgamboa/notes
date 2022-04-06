# React With Typescript

I'm following [this](https://youtube.com/playlist?list=PLC3y8-rFHvwi1AXijGTKM0BKtHzVC-LSK) tutorial. Thank you Codevolution for your awesome education materials!

1. Use `.ts` and `.tsx` files instead of `.js` and `.jsx` files

## Typing Props

1. When you pass props to a child component, the props' data types must be predefined within the child component. Example:

```
Haha lol.
```

## Components

You can type React Functional Components (RFC) with `const functionName: React.FC = () => {}`.

```
const Header = () => {
  return <header>{T`Harry's header`}</header>
}
```

**VS**

```
const Header: React.FC = () => {
  return <header>{T`Harry's header`}</header>
}
// OR
function Header(): React.ReactElement {}
```
