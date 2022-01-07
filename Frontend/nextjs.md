# Next.js

1. `npx create-next-app` (or `npx create-next-app --example with-tailwindcss with-tailwindcss-app`)
2. Run `npm run dev` instead of `npm start` with create-react-app
3. There is no `src` folder. Only a `pages` folder out of the box
4. **Components** should have uppercase file names - **pages** should have lowercase file names

## Routing

1. The folder structure of the `my-app/pages` Represents your website path
   - `.../my-app/pages/api/hello.js` would create the route `http://localhost:3000/api/hello`
   - `.../my-app/pages/aboutMe/index.js` would create the route `http://localhost:3000/aboutMe`
2. Using Brackets `[]` signifies that the route will be dynamic (See the [API Endpoints](<(https://github.com/mhgamboa/notes/blob/main/Backend/nextjs.md)#api-endpoints>) section for an example details)

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
