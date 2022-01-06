# Concurrently

1. Run `npm i concurrently -D`
2. package.json scripts:

```
"server": "nodemon app.js",
"client": "cd client && npm start",
"dev": "concurrently --kill-others-on-fail -n 'server,client' -c 'yellow,green' \"npm run server\" \"npm run client\""
```
