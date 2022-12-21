# Prisma

## Table of Contents

1. Setup/Installation

## Setup/Installation

1. To install the Prisma CLI run `npm install prisma -D`
   - The [Prisma CLI](https://www.prisma.io/docs/concepts/components/prisma-cli) is the primary way to interact with your Prisma project from the command line.
2. Initialize prisma in your project with `npx prisma init`
   - This creates `prisma/schema.prisma` in the root of your directory
3. Install the prisma client with `npm install @prisma/client`

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
3. Field **Attributes** can modify the field:
   - `id String @id @default(cuid())` OR `id Int @id @default(autoincrement())`
     - `@id` specifies this is the tables primary key
     - `@default` states what the default will be if not manually defined
     - `cuid()` is the value that will be defaulted
       - Use uuid, if you need globally unique identifiers and don't mind the added overhead of larger keys and slower performance
       - Use uuid, if you need faster performance and don't need globally unique identifier (larger chance to overlap with multiple databases)
   - `@updatedAt`
   - `@default(now())`
