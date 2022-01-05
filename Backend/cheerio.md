# Cheerio

Tbh, Cheerio is pretty useless on its own. you'll want to use cheerio with ~~axios~~ [node-fetch](https://www.npmjs.com/package/node-fetch) run `npm i node-fetch cheerio` and optionally `npm i pretty` if you want the html to be more legiblec

Then require the packages:

```
import fetch from "node-fetch";
import cheerio from "cheerio";
import pretty from "pretty";
```

You'll need to edit your package.json:

```
{
  "type": "module"
}
```

Alternatively you can require everything and import fetch uniquely:
`const fetch = (...args) => import('node-fetch').then(({default: fetch}) => fetch(...args));`

You'll want to get the data from Axios before using Cheerio. Pretty is used with cheerio if desired.

# Node Fetch

(Alternatively you can use [got](https://www.npmjs.com/package/got), or [axios](https://www.npmjs.com/package/axios))

- Pretty much just run the following:

```
const getHTML = async (url) => {
    const response = await fetch(url);
    const body = await response.text();

    return body;
}
```

# Cheerio
