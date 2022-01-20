# ESLint

Run `npm i eslint -D`

1. `npm run eslint --init`

## Not Next.JS

1. `npm run eslint --init` to create an elint.json file
   - Select desired options
2. `npm run eslint` to lint
   - `npm run lint --fix` to attempt to fix anything

## Linting with prettier

1. `npm i prettier -D`
2. `touch .prettierrc`
3. yarn add -D eslint-config-prettier

```
// .eslintrc.json:
...
"extends": ["eslint:reccommended", "prettier"],
///
```
