## Heroku

[Relevant article](https://devcenter.heroku.com/articles/getting-started-with-nodejs)

1. `cd my-app`
2. Use a [Procfile](https://devcenter.heroku.com/articles/procfile) to explicitly declare what command should be executed to start your app
   - It must exist in the root of your app
   - If no procfile the root package.json npm start becomes default
   - It has no file extension
   - Syntax is `<process type>: <command>`. Example: `web: npm start`
   - The `web` process type is how you recieve external HTTP traffic
3. `heroku create unique-app-name` -> creates heroku app
4. `git push heroku main` -> deploys heroku app
5. `heroku ps:scale web=1` -> Ensures at leasont one instance of app is running
6. `heroku open` -> opens deployed app in browser
7. `heroku logs --tail` -> view running stream of logs. `control+c` to stop streaming the logs
