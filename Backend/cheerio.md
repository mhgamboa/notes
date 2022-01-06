# Cheerio

Tbh, Cheerio is pretty useless on its own. you'll want to use cheerio with ~~axios~~ [node-fetch](https://www.npmjs.com/package/node-fetch) run `npm i node-fetch cheerio` and optionally `npm i pretty` if you want the html to be more legiblec

Then require the packages:

```
import fetch from "node-fetch";
import * as cheerio from "cheerio";
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

Cheerio likes looking like jQuery. So the first thing you'll doo with cheerio is load the html into a dollar sign ($). Example:

```
const $ = cheerio.load(body);
```

Then you can access the DOM like you would jQuery. Examples:

```
const $ = cheerio.load(
  <ul id="fruits">
    <li class="apple">Apple</li>
    <li class="orange">Orange</li>
    <li class="pear">Pear</li>
  </ul>
)

$('.apple', '#fruits').text(); // => Apple
$('ul .pear').attr('class'); // => pear
$('li[class=orange]').html(); // => Orange
const entireHTML = $.html()

```

**So Putting it all together:**

```
const getHTML = async (url) => {
    const response = await fetch(url);
    const body = await response.text();
    const $ = cheerio.load(body);


    return body;
}
```
