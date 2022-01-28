# Jest

run `npm i jest -D` or `yarn add jest --dev`.

In package.json add:

```
"scripts": {
    "test": "jest"
  }
```

npm run test `filename` without `.js` or `.test.js`. (Not specifying will run all test files)

## Basic Unit tests

First, create a sum.js file:

```
function sum(a, b) {
  return a + b;
}
module.exports = sum;
```

Then, create a file named sum.test.js. This will contain our actual test:

```
test('adds 1 + 2 to equal 3', () => {
});
```

Then run `npm run test` or `yarn test`

NOTE: `test()` is the same as `it()`. `toEqual()` is like `toBe()` but for comparing different objects.

Use `Describe()` to group tests together. Example:

```
descript("example tests", () => {
    test("should add 1 + 2 to equal 3", () => {
        const result = sum(1,2);
        expect(result).toBe(3);
    })

    test()
})
```

Can also compare with:

- `expect(foo).toBeFalsy()`
- `expect(foo).not.toBeTruthy()`
- `expect(value).toBeGreaterThan(3)`
- `expect(value).toBeLessThanOrEqual(3)`
- `expect(array).toContain(3)`
- `expect(sum(1, 2)).toThrow("Custom Error message")` //If an error is thrown, tests will pass
- `expect(sum(1, 2)).toThrow(Error)`

## Asynchronous functions

```
test('should return correct todo', async () => {
	const todo = await fetchData(1);
	expect(todo.id).toBe(1);
});
```

## Setup and Teardown

Setting things up before testing, and tearing it down after testing.

**Example:** You have an array you want to test with, but when you edit the array you mess up other tests. You would use the `beforeEach()` or `afterEach()` methods.

```
let animals = [
	'elephant',
	'zebra',
	'bear',
	'tiger',
];

beforeEach(() => {
	animals = [
		'elephant',
		'zebra',
		'bear',
		'tiger',
	];
});
```

`beforeAll()` will run once **BEFORE** any tests are run
`afterAll()` will run once **AFTER** any tests are run

You can scope all of these functions in different `describe()` instead of at the global level
