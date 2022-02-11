# MongoDB

- The [MongoClient](https://mongodb.github.io/node-mongodb-native/4.1/classes/MongoClient.html) is a class instantiated from `mongodb.MongoClient`. It is what you use to intereact with your Mongo Database in anyway
- You can connect your MongeClient to your cluster with `MongoClient.connect()`, which returns a promise. [Connect](https://mongodb.github.io/node-mongodb-native/4.1/classes/MongoClient.html#connect) simply connects your app to your cluster. Example:

```
import { MongoClient } from "mongodb"

const options = {}
const client = new MongoClient(process.env.ATLAS_URI, options);

const main = async () => {
  try {
    await client.connect();
  } catch(err) {
    console.error(err)
  } finally {
    await client.close
  }
}

```

- When your done with your connection you'll want to run `client.close()` to close the connection
- `app.use(express.json())` Parses the data sent in a post request

## Other

- Here's a [semi-good example](https://github.com/hoangvvo/nextjs-mongodb-app) of combining Next.js with mongodb
