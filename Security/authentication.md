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
