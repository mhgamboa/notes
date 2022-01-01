# Swagger UI

1. Within your desired postman collection, make sure all of the base urls are the same
2. Click the three dots on your desired collection and click "Export"
   1. Save this file as `docs.json`
3. Go to [apimatic.io](https://www.apimatic.io/)
4. Import `docs.json`. Igonore warnings and click proceed
5. Edit API
   1. Edit Name or add image if you want. Hit "Save Basic Settings"
6. Click on "Server Configuration" on left side pane
   1. Edit the url to your deployed heroku project. Example: `https://mgamboa-to-do.herokuapp.com/api/v1`
   2. Click **Save Config Settings**
7. Click on "Authentication" on left side pane
   1. Type should be "OAuth2.0 - Bearer Token
8. Click Endpoints. For Register and Login Route:
   1. Edit **Group** to be "Auth"
   2. **Skip authentication?** should be checked
   3. Click **Save Endpoint** after editing each endpoint
   4. You should see **a new Auth folder** under "Endpoints"
9. Group the rest of the endpoints accordingly by their basic routes. Don't click "Skip authentication?" if authentication is needed
10. Go back to main [dashboard](https://www.apimatic.io/dashboard) click three dots and click `Export API`
    1. Select **OpenAPI v3.0 (YAML)** Export Format and click `Export`
11. You should get a new page with your documentation in the swaggerUI syntax.
12. To make sure this renders correctly Copy and paste this code to [editor.swagger.io](https://editor.swagger.io/)
13. Edit the syntax as needed
    1. Delete any unnecessary tags at the bottom
    2. Edit any hard coded parameters like `{:id}`
    3. Test out your application through the swagger UI editor
14. `npm i yamljs` -> converts yaml into something Swagger UI can understand
15. `npm i swagger-ui-express` -> Adds Swagger UI to our application
16. Create a file in the root directory called `swagger.yaml`. Copy and paste the code from [editor.swagger.io](https://editor.swagger.io/) into this file
17. In root `app.js` add the following code:

```
const swaggerUI = require("swagger-ui-express");
const YAML = require("yamljs");
const swaggerDocument = YAML.load("./swagger.yaml");

...
//Somewhere near your other routes
app.use("/api-docs", swaggerUI.server, swaggerUI.setup(swaggerDocument));
```
