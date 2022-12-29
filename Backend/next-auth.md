# Next Auth

Thanks to [this course](https://themodern.dev/courses/build-a-fullstack-app-with-nextjs-supabase-and-prisma-322389284337222224) by Grégory D'Angelo for helping me learn.

To install Next Auth run `npm i next-auth`

## Configuring Next.js, NextAuth, and Prisma to work together

1. **Next.js 12:** Within your `_app.js` file, import the `SessionProvider` from `next-auth/react`, wrap everything inside this provider, and set the `session` prop using `pageProps.session` to allow the session state to be shared between pages. Example:

```
// pages/_app.js
import { SessionProvider as AuthProvider } from 'next-auth/react';

function MyApp({ Component, pageProps }) {
  return (
    <AuthProvider session={session}>
        <Component {...pageProps} />
    </AuthProvider>
  );
}

export default MyApp;
```

2. Since next-auth.js v4, you also need to install `nodemailer` to send magic link emails . Run `npm i nodemailer`
3. Edit your `pages/api/auth/[...nextauth].js` and import/configure the different ways you want your users to login. In this example, your enabling users to login via magic link and Github:

```
import NextAuth from "next-auth"
import GithubProvider from "next-auth/providers/github"
import EmailProvider from "next-auth/providers/email";

import nodemailer from "nodemailer";

export const authOptions = {
  providers: [
    GithubProvider({
      clientId: process.env.GITHUB_ID,
      clientSecret: process.env.GITHUB_SECRET,
    }),
    EmailProvider({
      server: {
        host: process.env.EMAIL_SERVER_HOST,
        port: process.env.EMAIL_SERVER_PORT,
        auth: {
          user: process.env.EMAIL_SERVER_USER,
          pass: process.env.EMAIL_SERVER_PASSWORD,
        },
      },
      from: process.env.EMAIL_FROM,
      maxAge: 60 * 10, // Magic links are only valid for 10 min
    }),
    // ...add more providers here
  ],
}
export default NextAuth(authOptions)
```

### Magic Links

For gmail:

```
EMAIL_SERVER_HOST="smtp.gmail.com"
EMAIL_SERVER_PORT=587
EMAIL_SERVER_USER=[Your Email]
EMAIL_SERVER_PASSWORD=[Your Password]
EMAIL_FROM=[The Email you want to send from]
```

## Using Next.js dynamic API routes to handle authentication requests

## Using Prisma and the Prisma Adapter for next-auth to persist users, email sign in tokens, and sessions

## Configuring OAuth authentication to sign users in with Google
