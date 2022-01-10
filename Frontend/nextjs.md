# Next.js

1. `npx create-next-app` (or `npx create-next-app --example with-tailwindcss with-tailwindcss-app`)
2. Run `npm run dev` instead of `npm start` with create-react-app
3. There is no `src` folder. Only a `pages` folder out of the box
4. **Components** should have uppercase file names - **pages** should have lowercase file names

## Routing

1. The folder structure of the `my-app/pages` Represents your website path
   - `.../my-app/pages/api/hello.js` would create the route `http://localhost:3000/api/hello`
   - `.../my-app/pages/aboutMe/index.js` would create the route `http://localhost:3000/aboutMe`

### Dynamic Routing

1. Using Brackets `[]` signifies that the route will be dynamic
   - `.../my-app/pages/products/[productId].js` would create the route `http://localhost:3000/products/:productId`
2. Acess the query params (`:productId` in this example) using the `useRouter` hook. Example:

```
// http://localhost:3000/products/:productId
import {useRouter} from "next/router";

const productId = () => {
  const router = useRouter();
  const { productId } = router.query;
  return <h1> This is product Id number is {productId}</h1>
}

export default productId;

```

3. If there is a conflict between a dynamic route and a predefined route, Next.js will always match the predefined route first

#### Nested Routes

- **Nested Routes** can be handled in two different ways:
  1. Via the folder structure:`.../my-app/pages/products/[productId]/reviews/[reviewId].js` would create the route `http://localhost:3000/products/:productId/reviews/:reviewId`
  2. Via `[...params].js`
     -Placing a `[...params].js` in ``.../my-app/pages/products/[...params].js` would handle all undefined routes that start with `http://localhost:3000/products/`
     -You could have `http://localhost:3000/products/:productId/reviews/:reviewId/foo/bar/baz` and the `[...params].js` would still handle it
  3. **`router.query.params` would become an array with each item being a different level of nesting.** Example:

```

import {useRouter} from 'next/router';

// Located in ../my-app/pages/products
// For http://localhost:3000/products/:productId/reviews/:reviewId
const Doc = () => {
  const router = userouter();
  const {params = []} = router.query; // Sets default value, since params is undefined on initial render

  console.log(params); // logs [:productId, review, :reviewId]

  return()
}
```

See also [Accessing Parameters](https://github.com/mhgamboa/notes/blob/main/Backend/nextjs.md#acessing-params).

## Accessing Params

1. You can access all parameters using the `useRouter` hook. Example:

```
// .../my-app/products/[productId].js

// http://localhost:3000/products/100?foo=bar&fat=cat
import {useRouter} from "next/router";

const productId = () => {
  const router = useRouter();

  const productId = router.query.productId; // equals 100
  const foo = router.query.productId; // equals bar
  const bar = router.query.productId; // equals cat
  ...
  return ()
}

export default productId;
```

## Client-Side Navigation (Links)

- When navigating within your site you nest all `<a>` tags within `<Link>`, which must be imported. `href` attributes will go in the `<Link>` tag
  - Navigating outside of your page can just use the regular `<a>` tag with no `<Link>`

```
import Link from 'next/link

const home = () => {
  return (
    <div>
      <Link href="/about-me">
        <a>About Me</a>
      </Link>
    </div>
  )
}
```

- To **navigate within dynamic routes** you'll have to use string interpolation. You'll probably have to pass down props as well. Example:

```
import Link from 'next/link

const home = ({productId = 100 }) => {
  return (
    <div>
      <Link href="/products/${productId}" replace> //replace attribute refreshes to the base url
        <a>product {ProductId}</a>
      </Link>
    </div>
  )
}
```

## Components

1. Components should live in `my-app/components` folder
2. Group components within their own sub directory. Example:

```
.../my-app/components/Navbar/Navbar.js
.../my-app/components/cards/Card.js
```

## Styling

You can styling Next.js Projects in 3 ways:

1. Gobal style sheets - automatically exists in `my-app/styles/globals.css`. Applies styles across the entire app.
   - Styles are imported into `_app.js`
2. CSS Modules - Allows you to write unique styles for each page/component.
   - These CSS files are saved as `pageName.module.css` in the `my-app/styles` folder
   - `import styles from ../styles/pageName.module.css`
   - Implement the styles with `<p className={styles.className}></p>`
3. style JSX

I've watched two different tutorials, and they both only use the global style sheets and the CSS Modules.

## API Endpoints

- API endpoints live in `my-app/pages/api`
- API Endpoints must use the following syntax:

```
export default function handler(req, res) {
  res.status(200).json({ name: 'John Doe' })
}
```

- Routing works the same as they do for pages
  - `.../my-app/pages/api/hello/index.js` would create the route `http://localhost:3000/api/hello`
- **Path parameters** can be achieved with the following folder structure:
  - `.../my-app/pages/api/hello/[id].js` would create the route `http://localhost:3000/api/hello`
- Using Brackets `[]` signifies that the route will be dynamic. Here's how you implement the function for a dynamic route:

```
// In .../my-app/api/v1/item/[id].js

export default function handler(req, res) {
   const { id } = req.query;
  res.status(200).json({id}); // sends the json { "id": id }
}
```
