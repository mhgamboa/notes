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

#### Nested Dynaimc Routes

- **Nested Routes** can be handled in two different ways: folder structure and catch all routes.

1. Folder Structure
   - Essentially just nest bracketed folders within each other. Example: `app/pages/products/[folderName]/reviews/[reviewId].js`
     - In `[reviewId].js` use the following code: `import { userRouter } from 'next/router; const { productId, reviewId } = router.query;`
     - This works but gets too complex with big apps. If you had 20 folders, you'd still have to create 20 `[reviewId].js` files
2. Nested Dynamic routing (Catch All routes)
   - Create a file called `[...filname].js` wherever you want the dynamic routing to start. (`[...params].js` is the conventional name)
     - **Essentially `[...fileName].js` just catches all possible routes that will be sent to the current URL. 404 pages will not be automatically generated since, all routes are technically valid, and will be caught.**
     - Example: instead of `app/pages/products/[folderName]/reviews/[reviewId].js` use `app/pages/[...params].js`
   - To access the url parameters, use the following code: `import { userRouter } from 'next/router; const { params } = router.query;`
     - `params` will return an array of url routes that was accessed

See also [Accessing Parameters](https://github.com/mhgamboa/notes/blob/main/Backend/nextjs.md#acessing-params).

### Accessing Params (UseRouter Hook)

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

### In-site Navigation

#### Links

- When navigating within your site you nest all `<a>` tags within `<Link>`, which must be imported. `href` attributes will go in the `<Link>` tag. (Not the `<a>` tag)
  - Without `<Link>` a new server request will be made, thus removing all client state
  - Navigating outside of your page can just use the regular `<a>` tag with no `<Link>`
  - **If the child is not an an `<a>` tag** you must us the `passHref` attribute in the `<Link>` tag

```
import Link from 'next/link'

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
import Link from 'next/link'

const home = ({productId = 100 }) => {
  return (
    <div>
      <Link href="/products/${productId}" replace> // Replace the current history state instead of adding a new url into the stack
        <a>product {ProductId}</a>
      </Link>
    </div>
  )
}
```

#### Editing the URL

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
     - **Essentially all `getStaticProps()` does is fetch the necessary data, and deliver it to the page component**
     - `getStaticProps()` is exported in the same file as the page component
     - `getStaticProps()` accepts an argument`context` which allows you to access any dynamic url path that the client has requested
     - `getStaticProps()` must return an object. This object contains two important key/value pairs:
       - `props` (required) which contains the data that is passed to the page component as props
       - `revalidate` (optional) which allows Next.js to rebuild a page without rebuilding an entire website. See [Incremental Static Regeneration below](https://github.com/mhgamboa/notes/blob/main/Frontend/nextjs.md#incremental-static-regeneration-isr)
   - `getStaticPaths(context)` can be used with `getStaticProps()` to allow for statically generated dynamic routing (`[fileName].js`)
     - **Essentially all `getStaticPaths()` does is tell Next.js how many pages to generate at build time**
     - `getStaticPaths()` must return an object with 2 important keys: `{patchs, fallback}`
       - `paths` (required) which has a value that is an array of objects relating to each unique dynamic param/page to be generated
         - Each object must have the the following key/value pair: `params: { [filename]: fetchedDataId }` to correctly assign routes to the generated pages
       - `fallback` (required)
         - A value of `false` says all unbuilt routes will generate a 404 error
         - A value of `true` Will do two things when an unbuilt route is requested:
           1. On the first request, a "fallback" version of the page will be served. This is defined in the page component Example:
              - `import { useRouter } from 'next/router'`
              - `const router = useRouter();`
              - `if (router.isFallback) {return <div>Loading...</div>}`
              - See another example [here](https://nextjs.org/docs/api-reference/data-fetching/get-static-paths#fallback-pages)
           2. While the "fallback" is being shown to the client, Next.js will statically build the required HTML/JSON and serve it as soon as possible
              - The built HTML/CSS will be served in future requests for the designated route
              - `fallback` reduces the originaly build time, by subsequently building out pages as needed

2. **Server Side Rendering (SSR)** -> Only used on **pages** (Also called page components), not **components**

   - **To perform Server Side Rendering, you must use `getServerSideProps()`**
     - `getServerSideProps()` fetches data and provides said data to the page components at request time
     - `getServerSideProps()` must be exported in the same file as the page componenet
     - `getServerSideProps()` logic will never run on the client-side. The code won't even be bundled
     - You can write server-side code directly in `getServerSideProps()` (fs module, DB queries, etc.)
     - To pass the fetched data to the page component `getServerSideProps()` should return an object with a `props` key
       - The page component will then take `props` as a parameter to receive the fetched data
     - SSR with Dynamic pages
       - `getServerSideProps(context)` also accepts the argument `context`, which has access to the url params
       - **`context` which is an object with 3 keys: params, req, res**

3. **Client Side Rendering (CSR)** -> Only used on **components**, not **pages** (Also called page components)
   - Instead of fetching data within the `useEffect()` hook, Next.js allows you to use external hook libraries for data fetching
     - Two of the most popular are swr (which was created by Vercel) and react-query
     - **Notes about querying are out of the scope of these notes. Check the [Frontend Folder](https://github.com/mhgamboa/notes/tree/main/Frontend) to find any applicable notes**

### Static Site Generation (SSG)

- Static generation is when all data is fetched and all pages are created at build time. Good when data doesn't change a whole lot. **Next.js is staticly generated by default.**

#### SSG With Data `getStaticProps()`

- To Fetch Data at Build Time with SSG **you must use the `getStaticProps()` hook**.
- `getStaticProps()` is exported in the same file where a **page (not a component)** is exported.
- `getStaticProps()` is where data is fetched and parsed.
- `getStaticProps()` returns an object two important key/value pairs:
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

1. Data will always be up to date
2. SEO as HTML is build before being sent to the client

**Cons:**

1.  SSR is slower than SSG. **Use SSR only when necessary.**

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

## API Routing

1. Create a folder called `api` inside of your `pages` folder - folder Routing works just the same
2. When you `export default` within api routes, you conventionally name functions "handler" Example: `export default function handler() {}`
   - handler() takes 2 parameters: `handler(req, res){}`
     - res works just like it does in express: `res.status(200).json({})`
3. Code written within the `api` folder is never bundled. So it will never slow down the client
4. CRUD requests:
   - **GET:** GET requests are the default. Just use `const res = await fetch("/api/route")` to have Next.js "Query" itself
   - **POST:** POST are defined in the 2nd parameter of `fetch()`. See example below.
   - **DELETE:** POST are defined in the 2nd parameter of `fetch()`. See example below.
   - **PATCH:** POST are defined in the 2nd parameter of `fetch()`. See example below.
   - In `api` `handler()` function use `if (req.method === "GET") {} else if (req.method ==="POST") {} else if...`
5. **You shouldn't request your own api in getStaticProps()!** You should instead just copy the logic performed in your API into `getStaticProps()` to save time (Or export a function to be used by both)

Example:

```
const res = await fetch("/api/", {
  method: "POST", //"POST", "DELETE", "PATCH", etc.
  body: JSON.stringify(),
  headers: {
    "content-Type": "application/json"
  }
})

const data = await res.json();
```

### API Dynamic Routing

There are two ways to create Dynamic routes:

1. create the folder `/api/v1/[itemId].js` **use the req object**
   - To acces the route parameters of `/api/v1/:itemId`
     - `export default function handler(req, res) {const {commentID} = req.query`
2. Nested Dynamic routing (Catch All routes)
   - Create a file called `[...filname].js` wherever you want the dynamic routing to start. (`[...params].js` is the conventional name)
     - **Essentially `[...fileName].js` just catches all possible routes that will be sent to the current URL. 404 pages will not be automatically generated since, all routes are technically valid, and will be caught.**
     - Example: instead of `api/[folderName]/reviews/[reviewId].js` use `api/[...params].js`
   - To access the url parameters, use `req.query`
     - `params` will return an array of url routes that was accessed
   - **Optional Cactch All Routes** can act as an `index.js` of a folder as well.
     - To use Optional catch all routes you ust use two brackets instead of one: `[[...filename]].js`

## Components

1. Components should live in `my-app/components` folder
2. Group components within their own sub directory. Example:

```
.../my-app/components/Navbar/Navbar.js
.../my-app/components/cards/Card.js
```

## Styling

### Global Styles

1. Global style sheet - automatically exists in `app/styles/globals.css`. This file applies styles across the entire app.
   - The style sheet found within `app/styles` should be imported into `_app.js`
2. All global css libraries (Tailwindcss/Bootstrap/etc.) should be imported into `_app.js` as well

### Component Styles

1. To implement component level styling you must utilize **CSS Modules**
2. These modules are saved as `pageOrComponentName.module.css`
3. These CSS module files should be saved in the `my-app/styles` folder
4. To implement styles into pages/components:
   - `import styles from ../styles/pageName.module.css`
   - `<div className={style.className}></div>`
5. Using different CSS modules allows you to use the same classNames accross different components
   - Next.js Adds a random string to classNames when the project is built to ensure that they don't conflict with one another
6. Styles defined within CSS modules override the globally defined styles

### Sass/SCSS Styling

I didn't care to watch [the video](https://www.youtube.com/watch?v=_14sPRuHcYw&list=PLC3y8-rFHvwgC9mj0qv972IO5DmD-H0ZH&index=52)...

### CSS-in-JS styling

Honestly, this section goes beyond the scope of these notes and what I want to study.

Just know that there are packages called `styled-components` and `styled-jsx` that you can use to style your app.

## Miscellaneous

### App Layout

#### Seamlessly use Navbars/Headers/Footers/etc in multiple pages of your app

1. In the `components` folder Create `Layout.js`

```
const Layout = ({children}) => {
  return (
    <div>
      <header>
        <nav></nav>
      </header>

      <div>{children}</div>

      <footer></footer>
    </div>
  )
}
```

2. In \_app.js

```
import '../styles/globals.css'
import '../styles/layout.css' //If applicable
import Layout from '../components/Layout'

function MyApp({ Component, pageProps }) {
  return (
  <Layout>
    <Component {...pageProps} />
  </Layout>
  )
}

export default MyApp
```

#### To Remove Layout style from certain pages:

```
// pages/register.js
import Footer from '../components/Footer.js'

const Register = () => {
  return <h1>Register Page</h1>
}

export default Register

Register.getLayout = function PageLayout(page) { // page === Register Page
  <>
    {page}
    <Footer />
  </>
}
```

```
// pages/_app.js

import Header from '../components/Header'
import Footer from '../components/Footer'

function MyApp({ Component, pageProps }) {
  if (component.getLayout) {
    return Component.getLayout(<Component {...pageProps} />); //Renders Page + Footer (Not Header)
  }

  return (
  <>
    <Header />
    <Component {...pageProps} />
    <Footer />
  </>
  )
}
```

### Head Component

- The **Head component** helps you dynamically manage a components `head` section

1. `import Head from 'next/head'`
2. In your page/component use `<Head></Head>`. EXAMPLE:

```
import Head from 'next/head'

export default function Home() {
  return (
    <>
      <Head>
        <title>Welcome to My App</title> {/* Shows up on Tab Header */}
        <meta name="description" content="Quick summary of page"/> {/* For SEO */}
      </Head>
      <Main></Main>
    </>
  )
}
```

3. You can also apply the head in `pages/_app.js` which will become a default `Head` for all pages
   - If `Head` is defined in a `page`, it will be used over the default

### Image Component

1. Images should be placed in the `public` folder
2. `import Image from 'next/image'`
3. use `<Image>` instead of `<img>`. This helps in a number of ways:
   - Lowers image resolution for faster downloading
   - Automatically implements lazy loading
   - If image is not mapped you can add `placholder='blur'` attribute to enhance lazy loading experience
     - use `blurDataURL=""` if you want to map

### Absolute Imports & Module lPaths

- Relative imports are normal imports that you've always seen. Example: `import Header from '../components/Header.js`
- Absolute imports pretty much just makes imports easier to look at

1. Create `jsconfig.json` in the root
2. In `jsconfig.json`:

```
{
  "compilerOptions": {
    "baseUrl": ".",
  }
}
```

- `"baseURL"` will let you use the following syntax:
  - `import Header from 'components/Header.js`
  - Instead of `import Header from '../components/Header.js`

```
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/layout/*": ["components/layout/*"]
    }
  }
}
```

- `"baseURL"` will let you use the following syntax:
  - `import Header from 'components/layout/Header.js`
  - Instead of `import Header from '../components/layout/Header.js`
- `"paths"` with `"baseUrl` will let you use the following syntax:
  - `import Header from '@/layout/Header.js`
  - Instead of `import Header from Header/components/Header.js`

### TypeScript Support

To Add TypScript support:

1. In the root create a file called `tsconfig.json`
2. `npm run dev`
3. You'll get an error prompring you to run `npm add --dev typescript @types/react`
4. `npm run dev`
   - This will create a new `tsconfig.json` and `next-env.d.ts` file
   - `tsconfig.json` sets up settings for TS
   - `tsconfig.json` makes sure TS types are picked up by the TS compiler
5. Copy `compilerOptions` from `jsconfig.json` into `compilerOptions` from `tsconfig.json`
