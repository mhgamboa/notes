# MongoDB

- The [MongoClient](https://mongodb.github.io/node-mongodb-native/4.1/classes/MongoClient.html) is a class instantiated from `mongodb.MongoClient`. It is what you use to intereact with your Mongo Database in anyway
- You can connect your MongeClient to your cluster with `MongoClient.connect()`, which returns a promise. [Connect](https://mongodb.github.io/node-mongodb-native/4.1/classes/MongoClient.html#connect) simply connects your app to your cluster. Example:

```
const client = new MongoClient(process.env.ATLAS_URI, { useNewUrlParser: true, useUnifiedTopology: true });

client.connect((err) => {
  const database = client.db("dbName");
  const collection = database.collection("collectionName");

  client.close();
});
```

- When your done with your connection you'll want to run `client.close()` to close the connection
- `app.use(express.json())` Parses the data sent in a post request

**MAKING REQUESTS FROM FRONTEND TO BACKEND**

- In `client`folder run `npm i axios`
- create a file `src/http-common.js`. Put the following code in this file:

```
import axios from "axios";

export default axios.create({
  baseURL: "http://localhost:8000", //Put whatever your server port number is
  header: {
    "Content-type": "application/json",
  },
});
```

- create a file `src/services/toDo.js`. Put the following code in this file:

```
import http from "../http-common.js";

class toDoDataService {
  getItems() {
    return http.get("/get/Route");
  }

  delteItems() {
    return http.delete("/delete/Route");
  }
}

export default new toDoDataService();
```

- In app.js put in the following code:

```
import ToDoService from "./services/toDo";
...
const [list, updateList] = useState([]);

const retrieveToDos = () => {
  ToDoService.getItems().then((response) => {
    updateList(response.data);
  }).catch(err => console.error(err));
};
```
