# Authentication

There are 4 main options to choose from to authenticate users. From leaast to most complex they are:

1. Session
2. JSON Web Tokens (JWT)
3. OAuth
   1. In-House
   2. SAAS
4. Other/Ad-Hoc

## HTTP Headers

- In the Http headers we can create a key value pair for the key `set-cookie: "cookie1=value1; cookie2=value2"`
- `set-cookie` creates the cookies which will exist in every request made by the client
- When you set cookies you can also include an `expires` key-value pair

## Session Authentication

**Quick Summary:** When the user logs in, the server (ExpressJs) will create a `sessionID` in the database. This `sessionID` will then be saved as a cookie in the client's browser, with an expiry date. This cookie is attached to all requests made by the client to your domain. When the user makes CRUD requests to the server, the sessionID must be matched between the client's cookie and the server's database.

- Use `npm i express-sessions`?
- Difference between a session and a cookie:
  - **cookies**
    - Is stored in the browser. (ExpressJs)
    - Is attached to every request made by a browser to the designated domain
    - Can't be used to store user credentials or secret information
  - **Sessions**
    - Is stored on the server (Like ExpressJS)
    - Are used to store lots of data (as it's difficult/tedious to do so in cookies)
