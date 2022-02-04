# Next.js

1. `npx create-next-app` (or `npx create-next-app --example with-tailwindcss with-tailwindcss-app`)
2. Run `npm run dev` instead of `npm start` with create-react-app
3. There is no `src` folder. Only a `pages` folder out of the box
4. **Components** should have uppercase file names - **pages** should have lowercase file names

## Routing

1. The folder structure of the `my-app/pages` Represents your website path
   - `.../my-app/pages/api/hello.js` would create the route `http://localhost:3000/api/hello`
   - `.../my-app/pages/aboutMe/index.js` would create the route `http://localhost:3000/aboutMe`

### UseRouter Hook

**TODO: TAKE GOOD NOTES ON `UseRouter()` Hook**

- Shallow Routing

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

### Overview

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

### Fetching Data TL;DR

There are 3 methods to fetch external data:

1. **Static Site Generation (SSG)** -> Only used on **pages** (Also called page components), not **components**

   - **To statically generate sites, you must use `getStaticProps()`**, which fetches data and builds pages at build time
     - `getStaticPaths()` is exported in the same file as the page component
     - `getStaticProps()` accepts an argument`context` which allows you to access any dynamic url path that the client has requested
     - `getStaticProp()` must return an object. This object contains two important key/value pairs:
       - `props` (required) which contains the data that is passed to the page component as props
       - `revalidate` (optional) which allows Next.js to rebuild a page without rebuilding an entire website. See [Incremental Static Regeneration](https://github.com/mhgamboa/notes/blob/main/Frontend/nextjs.md#incremental-static-regeneration-isr) below.
   - `getStaticPaths(context)` can be used with `getStaticProps()` to allow for statically generated dynamic routing (`[fileName].js`)
     - Essentially all `getStaticPaths()` does is tell Next.js how many pages to generate at build time
     - `getStaticPaths()` must return an object with 2 important keys: `{patchs, fallback}`
       - `paths` (required) which is an array of objects relating to each each unique dynamic param/page to be generated
         - Each object must have the the following key/value pair: `params: { [filename]: fetchedDataId. }` to correctly assign routes
       - `fallback` (required)
         - A value of `false` says all unbuilt routes will generate a 404 error
         - A value of `true` Will do two things when an unbuilt route is requested:
           1. On the first request, a "fallback" version of the page will be served. This is defined in the page component
              - Example: `const router = useRouter(); if (router.isFallback) {return <div>Loading...</div>}` [See an example](https://nextjs.org/docs/api-reference/data-fetching/get-static-paths#fallback-pages)
           2. While the "fallback" is being served, Next.js will statically build the required HTML/JSON and serve it as soon as possible.
              - The built HTML/CSS will be served in future requests for the designated route

2. **Server Side Rendering (SSR)** -> Only used on **pages** (Also called page components), not **components**

### Static Site Generation (SSG)

- Static generation is when all data is fetched and all pages are created at build time. Good when data doesn't change a whole lot. **Next.js is staticly generated by default.**

#### SSG With Data `getStaticProps()`

- To Fetch Data at Build Time with SSG **you must use the `getStaticProp()` hook**.
- `getStaticProps()` is exported in the same file where a **page (not a component)** is exported.
- `getStaticProp()` is where data is fetched and parsed.
- `getStaticProp()` returns an object two important key/value pairs:
  - `props` which passes data to the page component as props
  - `revalidate` which allows Next.js to rebuild react pages without rebuilding an entire website. Read more about [Incremental Static Regeneration](https://github.com/mhgamboa/notes/blob/main/Frontend/nextjs.md#incremental-static-regeneration-isr) below.
- `getStaticProps(context)` takes one argument `context`.

EXAMPLE:

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
    },
    revalidate: 60, // Rebuilds the Articles List page every 60 seconds
  }
}
```

##### SSG with data in Dynamic routes

1. `getStaticPaths()` must receive an argument `getStaticPaths(context)`. `context` is an object with a key called params that allows you to acces the URL params.
2. You must use `getStaticPaths()` which tells Next.js all the possible paths of `[postId].js` that will exist.
   - `getStaticPaths()` must return an object with a `paths` key, which is an array of objects relating to each each unique dynamic param
     - Each object must have the the following key/value pair: `params: { [filename]: fetchResult. }` to correctly route the data
     - If the page name is `pages/posts/[postId]/[commentId]`, then params should contain postId and commentId.
   - The returned `getStaticPaths()` object must also have a `fallback` key
     - `fallback: false` -> Paths not returned by `getStaticPaths()` will result in the 404 page
     - `fallback: true` -> Paths not returned by `getStaticPaths()` don't result in a 404 page. It show a fallback version, which is a static HTML page fetched & created at runtime, NOT buildtime. You'll need to create a `useRouter.fallback` in the component. [Here's an example](https://nextjs.org/docs/api-reference/data-fetching/get-static-paths#fallback-pages)
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
3. You don't get access to the request object (Can't GET user specified content)

#### Incremental Static Regeneration (ISR)

**Incremental Static Regeneration (ISR)** overcomes these deficits by rebuilding pages when data changes, without rebuilding your entire app. This is done with a `revalidate` key within the `getStaticProps()` return object. Example:

```
export async getStaticProps = (context) => {
  {postId} = context.params;
  fetch(`url/${postID}`);

  return {
    props: {
      post: data
    },
    revalidate: 60 // Revalidate every 60 seconds
  };
}
```

- With the `revalidate` key, ISR will only generate a new page **after** a request has been made, and the old HTML static page has been served

### Server Side Rendering (SSR)

#### Pros and Cons

**Pros:**

**Cons:**

- SSR is slower than SSG. **Use SSR only when necessary.**

#### getServerSideProps()

1. `getServerSideProps()` is how you get these data from your database. You can write server-side code directly in `getServerSideProps()` (fs module, DB queries, etc.)
2. `getServerSideProps()` logic will never run on the client-side. The code won't even be bundled
3. **Can only run in a page not a component**
4. Should always return an object with a `props` key
5. Runs at reqeust time
   EXAMPLE:

```
const newsArticleList = ({ articles }) {
  return (
    {articles.map(article => (
      <div key={article.id}>
        <h2>
          {article.id} {article.title} | {article.category}
        </h2>
      </div>
    ))}
  )
}

export default newsArticleList

export async getServerSideProps = () => {
  const url = ""
  const res = await fetch(url)
  const data = await response.json()

  return {
    props: {
      article: data
    }
  }
}

```

#### SSR with Dynamic pages

1. `getServerSideProps(context)` also accepts the argument **`context` which is an object with 3 keys: params, req, res**. EXAMPLE:

```
export async function getServerSideProps(context) {
const { params, req, res } = context
}
```

### Client Side Rendering

**Client Side Fetching is good when:**

1. You are generating a page that is private
2. Doesn't need SEO
3. Is user Specific
4. When you want to fetch data from components
   - `getStaticProps()` and `getServerSideProps()` can only fetch data from pages

While you can use `useEffect()` to fetch data, **Next.js provides its own methods for client-side data fetching data called SWR** (Stale While Revalidate)

#### useEffect()

1. In `useEffect()` you can fetch data. Example:

```
import { useEffect } from 'react'

const Dashboard = () => {
  useEffect(() => {
    fetchDashboardData = async () => {
      const res = await fetch()
    }
    fetchDashboardData()
  })
}

export default Dashboard
```

#### SWR

- SWR is a React Hook library for data fetching

1. Run `npm i swr`
2. In your component use `const { data, error } = useSWR('dashboard', functionThatFetchesData)`
   - `data` is the data returned. `error` is any error that occurs during the fetching

```
import useSWR from 'swr

const fetcher = async () => {
  const url = '';
  const rest = await fetch (url);
  const data = await res.json();
  }

const DashboardSWR = () => {
  const { data, error } = useSWR('dashboard', fetcher);

  if (error) return "An error has occurred fetching DashboardSWR";
  if (data) return "Loading"; // Displayed in browser while data is loading

  return (
    <div>
      <h1>Dashboard</h1>
      <h2>Posts - {data.posts}</h2>
      <h2>Likes - {data.likes}</h2>
      <h2>Followers - {data.followers}</h2>
      <h2>Following - {data.following}</h2>
    </div>
  )
}

export default DashboardSWR;
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
