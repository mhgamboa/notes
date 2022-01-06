# Puppeteer

Note: I just went through a lot of blogs, and apparently [puppeteer](https://pptr.dev/) is lot like [playwright](https://playwright.dev/), the biggest difference being that playwright supports more browsers than just Chrome. Apparently some of the core devs left puppeteer (Created by Google) to create playwright (created by Microsoft).

Run `npm i puppeteer` to get started.
`const puppeteer = require('puppeteer')`

## Accessing a web page

```
const start = async () => {
    const browser = await puppeter.launch(); // Launches the headless browser
    const page = await browser.newPage(); // Opens a new webpage in the browser
    await page.goto("https://website.com/path/name");
    ... // Do whatever you want (screenshot, scrape, etc.)
    await browser.close();
}

start();
```

## Screenshots

```
await page.screenshot({
    path: "filename.jpg",
    fullPage: true, // Options
})
```

## Scraping

```
const start = async () => {
    ... // See "Accessing" section above

    const foo = await page.evaluate(() => {
        // Write any Client Side Javascript. (queryselector, getElementById, etc)
        // Everything here happens in Browser land, not the NodeJS console

        return Array.from(document.querySelectorAll(".className")).map(item=> item.textContent);
    });

    await browser.close()
}
```

- `page.$$eval()` will automatically run `Array.from(document.querySelectorAll(selector)`. Example:
  - `const foot = await page.$$eval("Selector", (item)=> item.textContent)`

## Clicking Buttons

```
const main = async () => {
    ...
    await page.click("#idName"); // Or ".className"
    await page.waitForNavigation(); // If clicking button causes some sort of navigation

    await browser.close()
}
```

If the above gives you erros do the following instead:

```
const main = async () => {
    ...
    await Promise.all([page.click("#idName"), page.waitForNavigation()])

    await browser.close();
}
```

## Filling Forms

```
const main = async () => {
    ...
    await page.type(".cssSelector", "What to Type");
}
```
