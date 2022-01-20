# Husky

[Official Documentation](https://www.npmjs.com/package/husky)

1. `npm i husky -D`
2. `npm set-script prepare "husky install"` // use even if your using yarn
3. `npm run prepare`
4. `npx husky add .husky/pre-commit "npm test"`
