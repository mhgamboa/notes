## Heroku

[Relevant article](https://devcenter.heroku.com/articles/getting-started-with-nodejs)

1. `cd my-app`
2. Use a [Procfile](https://devcenter.heroku.com/articles/procfile) to explicitly declare what command should be executed to start your app
   - It must exist in the root of your app
   - If no procfile the root package.json npm start becomes default
   - It has no file extension
   - Syntax is `<process type>: <command>`. Example: `web: npm start`
   - The `web` process type is how you recieve external HTTP traffic
3. Add a "heroku-postbuild" script. This is a hook that enables further builds in subdirectory Example:

```
"install-client": "cd client && npm install",
"build-client": "cd client && npm run build",
"heroku-postbuild": "npm run install-client && npm run build-client"
```

4. `heroku create unique-app-name` -> creates heroku app
5. `git push heroku main` -> deploys heroku app
   - `git push origin main` -> pushes changes to github
6. `heroku ps:scale web=1` -> Ensures at leasont one instance of app is running
7. `heroku open` -> opens deployed app in browser
8. `heroku logs --tail` -> view running stream of logs. `control+c` to stop streaming the logs

### cloning a heroku Project

1. `heroku git:clone -a heroku-app-name`
2. `cd heroku-app name`
3. `git add origin git@github.com:mhgamboa/repository-name.git`
   - **Note:** the repository name will probably differ from Heroku app name
4. `git push heroku main` -> deploys heroku app
5. `git push origin main` -> pushes changes to github
