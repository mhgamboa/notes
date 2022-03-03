# Notes

All the notes I take are here so I will always remember.

## [Backend](https://github.com/mhgamboa/notes/tree/main/Backend)

1. [Swagger UI](https://github.com/mhgamboa/notes/blob/main/Backend/swaggerUI.md)
2. [Heroku](https://github.com/mhgamboa/notes/blob/main/Backend/heroku.md)
3. [puppeteer](https://github.com/mhgamboa/notes/blob/main/Backend/puppeteer.md)
4. [concurrently](https://github.com/mhgamboa/notes/blob/main/Backend/concurrently.md)
5. [cheerio](https://github.com/mhgamboa/notes/blob/main/Backend/cheerio.md)
6. [cron](https://github.com/mhgamboa/notes/blob/main/Backend/cron.md)

## [Databases](https://github.com/mhgamboa/notes/tree/main/Databases)

1. [MongoDB](https://github.com/mhgamboa/notes/blob/main/DataBases/mongodb.md)
2. [Mongoose](https://github.com/mhgamboa/notes/blob/main/Databases/mongoose.md)
3. [PostgresQL](https://github.com/mhgamboa/notes/blob/main/Databases/postgresql.md)
4. [Entity Relationship Diagrams](https://github.com/mhgamboa/notes/blob/main/Databases/entity-relationship-diagrams.md)

## [Frontend](https://github.com/mhgamboa/notes/tree/main/Frontend)

1. [Nextjs](https://github.com/mhgamboa/notes/blob/main/Frontend/nextjs.md)
   1. [node-auth](https://www.youtube.com/playlist?list=PLzYM-WGWIJDQzw0bEJKHCyx_ZDdZ13IWt)
2. [Chart.js](https://github.com/mhgamboa/notes/blob/main/Frontend/chartjs.md)

## [Python](https://github.com/mhgamboa/notes/tree/main/Python)

1. [Basics](https://github.com/mhgamboa/notes/blob/main/Python/basics.md)

## [Javascript](https://github.com/mhgamboa/notes/tree/main/Javascript)

1. [Dates](https://github.com/mhgamboa/notes/blob/main/Javascript/dates.md)
2. [TypeScript](https://github.com/mhgamboa/notes/blob/main/Javascript/typescript.md)

### [NPM Packages](https://github.com/mhgamboa/notes/tree/main/Javascript)

1. [express-session](https://github.com/mhgamboa/notes/blob/main/Javascript/NPM%20Packages/express-session.md)
2. [connect-mongo](https://github.com/mhgamboa/notes/blob/main/Javascript/NPM%20Packages/connect-mongo.md)

## [Security](https://github.com/mhgamboa/notes/tree/main/Security)

1. [Authentication](https://github.com/mhgamboa/notes/blob/main/Security/authentication.md)
2. [PassportJS](https://github.com/mhgamboa/notes/blob/main/Security/passportjs.md)

## [Testing](https://github.com/mhgamboa/notes/tree/main/Testing)

1. [Basics](https://github.com/mhgamboa/notes/blob/main/Testing/basics.md)
2. [Jest](https://github.com/mhgamboa/notes/blob/main/Testing/jest.md)
3. [puppeteer](https://github.com/mhgamboa/notes/blob/main/Backend/puppeteer.md)

## [Other](https://github.com/mhgamboa/notes/tree/main/Other)

1. [ESLint](https://github.com/mhgamboa/notes/blob/main/Other/eslint.md)
1. [Husky](https://github.com/mhgamboa/notes/blob/main/Other/husky.md)

## Stuff I want to learn

1. Next.js (Learning Right Now)
   1. node-auth (Learning Right Now)
2. SQL (PostgresQL)
3. Docker
4. [Typescript](https://youtube.com/playlist?list=PLC3y8-rFHvwi1AXijGTKM0BKtHzVC-LSK)
5. [AWS](https://aws.amazon.com/pricing/) or [AWS Free Tiers](https://aws.amazon.com/pricing/)
   1. Compute
      1. EC2?
      2. Lambda? (Free)
   1. Storage
      1. s3?
      2. Cloudfront or Storage Gateway? (Free)
   1. Database
      1. Aurora? (No free trial)
      2. RDS? (No free trial)
      3. DynamoDB? (Free)
   1. Send Emails
      1. SES (Free)
   1. Other
      1. Amazon SQS (Queue structure)
      1. Elasticache (Redis caching)
6. Digital Ocean? (May be a stepping stone between Heroku and AWS)
   1. Compute
   2. Droplets
   3. S3
7. Security ([tutorial](https://www.youtube.com/watch?v=F-sFp_AvHc8)
   1. Session Authentication
      - [express-session](https://www.npmjs.com/package/express-session)
      - Cookie flags:
        - `httpOnly: true` -> don't let JS code access cookies
        - `secure: true` -> Only set cookies if website uses https
        - `ephemeral: true` -> destroy cookies when the browser closes
   2. OAuth
   3. Password
      - [Short Intro video](https://www.youtube.com/watch?v=--tnZMuoK3E)
      - bcrypt vs scrypt vs Argon2(i/d) (From what I can read, bcrypt was the standard, now Argon2 is becomin the standard)
   4. Cross Site Request Forgery (`npm i csurf`)
   5. `npm i helmet` -> Sets up a bunch of http headers that ensure security
   6. Passport library to help with implementing security ()
8. React Query ([tutorial](https://www.youtube.com/watch?v=VtWkSCZX0Ec))
9. NPM packages
   1. [validator](https://www.npmjs.com/package/validator)
   2. [ajv](https://www.npmjs.com/package/ajv)
   3. [multer](https://www.npmjs.com/package/multer)
   4. [passport](https://www.npmjs.com/package/passport) (See "Security" above)
10. [Python and SQL course](https://www.youtube.com/watch?v=0sOvCWFmrtA)
