# Notes

All the notes I take are here so I will always remember.

## [Backend](https://github.com/mhgamboa/notes/tree/main/Backend)

1. [Swagger UI](https://github.com/mhgamboa/notes/blob/main/Backend/swaggerUI.md)
2. [Heroku](https://github.com/mhgamboa/notes/blob/main/Backend/heroku.md)
3. [Mongoose](https://github.com/mhgamboa/notes/blob/main/Backend/mongoose.md)
4. [puppeteer](https://github.com/mhgamboa/notes/blob/main/Backend/puppeteer.md)
5. [concurrently](https://github.com/mhgamboa/notes/blob/main/Backend/concurrently.md)
6. [cheerio](https://github.com/mhgamboa/notes/blob/main/Backend/cheerio.md)
7. [cron](https://github.com/mhgamboa/notes/blob/main/Backend/cron.md)
8. [MongoDB](https://github.com/mhgamboa/notes/blob/main/Backend/mongodb.md)

## [Frontend](https://github.com/mhgamboa/notes/tree/main/Frontend)

1. [Nextjs](https://github.com/mhgamboa/notes/blob/main/Frontend/nextjs.md)
1. [Chart.js](https://github.com/mhgamboa/notes/blob/main/Frontend/chartjs.md)

## [Javascript](https://github.com/mhgamboa/notes/tree/main/Javascript)

1. [Dates](https://github.com/mhgamboa/notes/blob/main/Javascript/dates.md)
2. [TypeScript](https://github.com/mhgamboa/notes/blob/main/Javascript/typescript.md)

## [Testing](https://github.com/mhgamboa/notes/tree/main/Testing)

1. [Jest](https://github.com/mhgamboa/notes/blob/main/testing/jest.md)
2. [Jest](https://github.com/mhgamboa/notes/blob/main/testing/basics.md)
3. [Puppeteer](https://github.com/mhgamboa/notes/blob/main/testing/basics.md)
4. [puppeteer](https://github.com/mhgamboa/notes/blob/main/Backend/puppeteer.md)

## [Other](https://github.com/mhgamboa/notes/tree/main/Other)

1. [ESLint](https://github.com/mhgamboa/notes/blob/main/Other/eslint.md)
1. [Husky](https://github.com/mhgamboa/notes/blob/main/Other/husky.md)

## Stuff I want to learn

1. Next.js (Learning Right Now)
2. SQL (PostgresQL)
3. [AWS](https://aws.amazon.com/pricing/) or [AWS Free Tiers](https://aws.amazon.com/pricing/)
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
4. Digital Ocean? (May be a stepping stone between Heroku and AWS)
   1. Compute
   2. Droplets
   3. S3
5. Security ([tutorial](https://www.youtube.com/watch?v=F-sFp_AvHc8&t=1007s&disableadblock=1))
   1. Session Authentication
      - Cookie flags:
        - `httpOnly: true` -> don't let JS code access cookies
        - `secure: true` -> Only set cookies if website uses https
        - `ephemeral: true` -> destroy cookies when the browser closes
   2. OAuth
   3. Password Hashing
      - bcrypt vs scrypt vs Argon2(i/d) (From what I can read, bcrypt was the standard, now Argon2 is becomin the standard)
   4. Cross Site Request Forgery (`npm i csurf`)
   5. `npm i helmet` -> Sets up a bunch of http headers that ensure security
   6. Passport library to help with implementing security ()
6. React Query ([tutorial](https://www.youtube.com/watch?v=VtWkSCZX0Ec))
