# [express-session](https://www.npmjs.com/package/express-session)

1. `npm i express-session`
2. `import session from 'express-session';`

By default `express-session` doesn't store data in a database - only within it's own state.

```
const MongoStore = require('connect-mongo')(session);

const sessionStore = new MongoStore({
    mongooseConnection: connection,
    collection: "sessions"
})

app.use(session({
    session: ".env secret key that signs the cookie",
    resave: false,
    saveUninitialized: true,
    store: sessionStore,
    cookie: {
        maxAge: 1000 * 60 * 60 * 24 * 1, // Millisec * Sec * Min * Hr = 1 Day
    }
}));
```

- options:
  - `resave`:
