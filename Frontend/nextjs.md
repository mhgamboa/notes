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

- **Nested Routes** can be handled in two different ways: folder structure and catch all routes.

1. Create a folder structure as such:`.../my-app/pages/products/[productId]/reviews/[reviewId].js` would create the route `http://localhost:3000/products/:productId/reviews/:reviewId`

- This works but gets too complex with big apps

#### Nested Dynamic routing (Catch All routes)

1. Via `[...params].js`
   -Placing a `[...params].js` in ``.../my-app/pages/products/[...params].js` would handle all undefined routes that start with `http://localhost:3000/products/`
   -You could have `http://localhost:3000/products/:productId/reviews/:reviewId/foo/bar/baz` and the `[...params].js` would still handle it
2. **`router.query.params` would become an array with each item being a different level of nesting.** Example:

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

### Accessing Params

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

### Navigation

#### Links

- When navigating within your site you nest all `<a>` tags within `<Link>`, which must be imported. `href` attributes will go in the `<Link>` tag. (Not the `<a>` tag)
  - Without `<Link>` a new server request will be made, thus removing all client state
  - Navigating outside of your page can just use the regular `<a>` tag with no `<Link>`
  - **If the child is not an an `<a>` tag** you must us the `passHref` attribute in the `<Link>` tag

```
import Link from 'next/link

const home = () => {
  return (
    <div>
      <Link href="/about-me">
        <a>About Me</a>
      </Link>
      <Link href="/about-me" passhref>
        <h2>About You</>
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

#### Programatic Routing

- You never want to alter the URL directly since the browser will make a new request to the server and you will lose state
- Change the url with `router.push("/localPath")` instead of using `window.location.href`
  - You can also use `router.replace("/localPath")` if you don't want to add to the browser history stack (Think of the back button)

### API Endpoints

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

### 404

- Next will automatically 404 unknown routes, but you can customize it
- in `.../my-app/pages/404.js` you can create a page however you want

## Pre-Rendering & Data Fetching

Pre-rendering means the data is loaded before it is received by the browser. Create-React-App renders the web page after the document is received. Pros of pre-rendering:

1. Pages load faster, as less Javascript needs to be run
2. Pre-rendering helps with SEO

There are 2 types of pre-rendering:

1. Static Site Generation (SSG)

   1. Without data (default)
   2. With data (`getStaticProps()`)
   3. Inceremental Static Generation
   4. Dynamic parameters when fetching data

2. Server-side Rendering (SSR)
   1. Data fetching

These notes also cover **Client-side data fetching** & **Combining pre-rendering with client-side data-fetching**

### Static Site Generation (SSG)

- Static generation is when all pages are created at build time. Good when data doesn't change a whole lot. **Next.js is staticly generated by default.**

#### SSG With Data (Fetch Data at build time)

1. To Fetch Data at Build Time with SSG **you must use the `getStaticProp()` hook**. You make this when you export a **page (not a component)**, you also export a function called `getStaticProp()`, where data is fetched and parsed. Then return an object with the key/value pair `{props: {key1: parseddata.value1}}`. EXAMPLE:

```
const ArticlesList = (props) => {
  return (
    <div>
      {props.articles.map(foo=>{...})}
    </div>
  )
}

export default ArticlesList;

export async function getStaticProps() {
  const res = await fetch("url");
  const data = await res.json();

  return {
    props: {
      articles: data
    }
  }
}
```

##### SSG with data in Dynamic routes

1. `getStaticProps()` must receive an argument `getStaticProps(context)`. `context` is an object with a key called params that allows you to acces the URL params.
2. You must use `getStaticPaths()` which tells Next.js all the possible paths of `[postId].js` that will exist.
   - `getStaticPaths()` must return an object with a `paths` key, which is an array of objects relating to each each unique dynamic param
   - The returned `getStaticPaths()` object must also have a `fallback` key
     - `fallback: False` -> Paths not returned by `getStaticPaths()` will result in the 404 page
     - `fallback: True` -> Paths not returned by `getStaticPaths()` don't result in a 404 page. It show a fallback version, which is a static HTML page fetched & created at runtime, NOT buildtime. You'll need to create a useRouter.fallback in the component
       - **Use Case:** if you have thousands of pages, but only want to load them all at the same time, you can have `getStaticPaths()` only load a couple, and then statically render the rest when necessary
     - `fallback: "blocking"` -> Like `True`, but does it asynchronously, and will take time to load
       - **Use Case:** Preference. Vercel recommends True, unless you have issues

Example:

```
// In file called ./root-project/pages/posts/[postId].js
export default post = ({ post })=>{
  return (
    <div>
      <h1>post.id</h1>
      <p>{post.body}</p>
    </div>
  )
}

export async getStaticPaths = () => {
  const res = await fech(`url/posts`); //Fetch all the posts
  const data = await res.json(); //parse the data to json

  const paths = data.map(post => ({
    params: {
      postId: `${post.id}`
    }
  }));

  return {
    paths,
    fallback: false
    };
}

export async getStaticProps = (context) => {
  {postId} = context.params;
  fetch(`url/${postID}`);

  return {
    props: {
      post: data
    }
  };
}

```

#### Pros and Cons of SSG

**Pros:**

1. Can be build and cached on a CDN for super fast download speeds
2. Better for SEO
3. `getStaticPaths()` helps create dynamic data on static pages (best of both worlds)

**Cons:**

1. Build time can be horrendous
2. Data can grow stale

**Incremental Static Regeneration (ISR)** overcomes these deficits by rebuilding pages when data changes, without rebuilding your entire app. This is done with a `revalidate` key

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
