# dotenv

Notes for the [dotenv](https://www.npmjs.com/package/dotenv) package.

1. `npm i dotenv`
2. As early as possible in your application, require and configure dotenv. `require('dotenv').config()` **OR for ES6:**

```
import dotenv from "dotenv";
dotenv.config();
```

**OR**

```
import { config } from "dotenv";
config();
```

3.
