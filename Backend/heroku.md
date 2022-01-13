# Heroku

## Create a Heroku App

[Relevant article](https://devcenter.heroku.com/articles/getting-started-with-nodejs)

1. `cd my-app`
2. Use a [Procfile](https://devcenter.heroku.com/articles/procfile) to explicitly declare what command should be executed to start your app
   - It must exist in the root of your app
   - If no procfile the root package.json npm start becomes default
   - It has no file extension
   - Syntax is `<process type>: <command>`. Example: `web: npm start`
   - The `web` process type is how you recieve external HTTP traffic
3. Add "engines" key-value pair in package.json pointing to your current version of node. Example:

```
  "engines": {
    "node": "16.x"
  }
```

4. Add a "heroku-postbuild" script. This is a hook that enables further builds in subdirectory Example:

```
"install-client": "cd client && npm install",
"build-client": "cd client && npm run build",
"heroku-postbuild": "npm run install-client && npm run build-client"
```

5. `heroku create unique-app-name` -> creates heroku app
6. `git push heroku main` -> deploys heroku app
   - `git push origin main` -> pushes changes to github
7. `heroku ps:scale web=1` -> Ensures at leasont one instance of app is running
8. `heroku open` -> opens deployed app in browser
9. `heroku logs --tail` -> view running stream of logs. `control+c` to stop streaming the logs

## cloning a heroku Project

1. `heroku git:clone -a heroku-app-name`
2. `cd heroku-app-name`
3. If you want to clone the Heroku Project:
   1. `git remote add origin git@github.com:mhgamboa/repository-name.git`
      - **Note:** the repository name will probably differ from Heroku app name
4. If you want to clone the Github Project:
   1. `git remote add heroku https://git.heroku.com/heroku-project-name.git`
      - **Note:** the repository name will probably differ from Heroku app name
5. `git push heroku main` -> deploys heroku app
6. `git push origin main` -> pushes changes to github

## Deploying in a Subdirectory
