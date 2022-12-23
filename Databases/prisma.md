# Prisma

A huge thank you to Grégory D'Angelo for his free course [Build a Full-Stack App with Next.js, Supabase & Prisma](https://themodern.dev/courses/build-a-fullstack-app-with-nextjs-supabase-and-prisma-322389284337222224). I could not have learned all this without you.

NOTE: You can't use Prisma directly on the client-side. You must use getServerSideProps/getStaticProps(or whatever Next 13 is), or submit to the `pages/api` directory (or whatever the next 13 equivalent is)

## Table of Contents

1. [Setup/Installation](https://github.com/mhgamboa/notes/blob/main/Databases/prisma.md#setupinstallation)
2. [schema.prisma](https://github.com/mhgamboa/notes/blob/main/Databases/prisma.md#schemaprisma)
   - [Code generated in schema.prisma with `npx prisma init`](https://github.com/mhgamboa/notes/blob/main/Databases/prisma.md#code-generated-in-schemaprisma-with-npx-prisma-init)
   - [prisma.schema models](https://github.com/mhgamboa/notes/blob/main/Databases/prisma.md#prismaschema-models)
3. [Prisma Client](https://github.com/mhgamboa/notes/blob/main/Databases/prisma.md#prisma-client)
4. [Creating & Managing Tables](https://github.com/mhgamboa/notes/blob/main/Databases/prisma.md#creating--managing-tables)
5. [Prisma Studio](https://github.com/mhgamboa/notes/blob/main/Databases/prisma.md#prisma-studio)

## Setup/Installation

1. To install the Prisma CLI run `npm install prisma -D`
   - The [Prisma CLI](https://www.prisma.io/docs/concepts/components/prisma-cli) is the primary way to interact with your Prisma project from the command line.
2. Initialize prisma in your project with `npx prisma init`
   - This creates `prisma/schema.prisma` in the root of your directory
3. Install the prisma client with `npm install @prisma/client`
4. Edit schema
5. `npx prisma migrate dev`

## schema.prisma

### Code generated in schema.prisma with `npx prisma init`

```
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}
```

1. `generator client` is what your code will be generated into. `prisma-client-js` is the standard default for 99% of use cases. Really you'd only change this if you want to use graphQL

2. `datasource db` states where you're sourcing your data
   - `provider` is where your getting your data
   - `url` is your uri connection string

### prisma.schema models

```
model ModelName {
  id          String   @id @default(cuid())
  image       String?
  title       String
  description String
  price       Float
  guests      Int
  beds        Int
  baths       Int
  keywords    String[]
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}
```

1. Every model must have an `id` field
2. **Type Modifiers** can modify the field:
   - `image   String?` means the field is optional
   - `keywords  String[]` means a list of strings (if supported by db)
   - you can not combine type modifiers (At this time)
3. Field **Attributes** can modify the field:
   - `id String @id @default(cuid())` OR `id Int @id @default(autoincrement())`
     - `@id` specifies this is the tables primary key
     - `@default` states what the default will be if not manually defined
     - `cuid()` is the value that will be defaulted
       - Use `uuid()`, if you need globally unique identifiers and don't mind the added overhead of larger keys and slower performance
       - Use `autoincrement()`, if you need faster performance and don't need globally unique identifier (autoincremented integers have a larger chance to overlap with multiple databases)
   - `@updatedAt`
   - `@default(now())`
4. The term **Scalar Types** generally means "Primitive types" (string, int, bool, float, etc.)
   - These will convert to different types depending on your database.
     - Example: `title  String` would be a **varchar(191) in mySQL**, but a **text in PostgreSQL**
     - Example 2: `price  Float` would be a **DOUBLE in mySQL**, but a **double precision in PostgreSQL**
   - The data source connector defined in your schema.prisma file (postgresql in the example above) is the one responsible for determining what native database type each of Prisma scalar type maps to.
   - The Prisma JS client (the generator) determines what type in JavaScript each of these types map to.

## Prisma Client

1. The Prisma client is a tool that helps us interact with the databasefrom our Next.js (or node) by performing CRUD operations with records in our database.
   - It does this by providing JavaScript code that we can use to perform queries on our data
   - So when we "generate our Prisma Client" we're generating the unique JavaScript code that can perform CRUD operations on our unique data models.
2. Prisma Client is installed with `npm install @prisma/client`
3. Prisma Client is generated with `npm install @prisma/client`, but everytime you make changes to your schema.prisma you'll need to run **`npx prisma generate`**
   - This reads the `generator` block from `schema.prisma` to know which generator to use
   - If unspecified the output can be found in `./node_modules/.prisma/client`
   - This ensures that you only have to worry about your data, and not SQL queries
4. NOTE: You can't use Prisma on the client-side.

```
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();
```

## Creating & Managing Tables

1. To create a table you can use the following 2 commands: `npx prisma db push` and `npx prisma migrate dev`
   - `npx prisma db push`
     - Helpful for prototyping
     - Generates no migration history
     - if you run db push after adding a new required field **you will lose all your data**
   - `npx prisma migrate dev`
     - Syncs the schema with the database
     - Keeps track of the changes
     - Maintains existing data in the database

## Prisma Studio

1. Prisma Studio is a database client to explore and manipulate your data
2. Run `npx prisma studio` and navigate to [http://localhost:5555/](http://localhost:5555/) to view it

## Queries

First you must import the client:

```
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();
```

2. Generally operations follow the following format: `prisma.[tableName].[query]()`
