# MongoDB

## Connecting to MongoDB

- The [MongoClient](https://mongodb.github.io/node-mongodb-native/4.1/classes/MongoClient.html) is a class instantiated from `mongodb.MongoClient`. It is what you use to intereact with your Mongo Database in anyway
- You can connect your MongeClient to your cluster with `MongoClient.connect()`, which returns a promise. [Connect](https://mongodb.github.io/node-mongodb-native/4.1/classes/MongoClient.html#connect) simply connects your app to your cluster. Example:

```
import { MongoClient } from "mongodb"

const options = {}

const main = async () => {
  try {
    const client = new MongoClient(process.env.ATLAS_URI, options);
    await client.connect();

    const db = client.db('dbName');
    const collection = db.collection('collectionName');

  } catch(err) {
    console.error(err)
  } finally {
    await client.close
  }
}
```

- When your done with your connection you'll want to run `client.close()` to close the connection
- `app.use(express.json())` Parses the data sent in a post request

## Performing CRUD Operations

- **Queries** are json objects that specify what your looking for. Example:

```
const query = {
  runtime: { $lt: 15 },
  title: "The Room"
};

```

**Query Operators**

- **$eq:** equal to
- **$ne:** not equal to
- **$gt:** greater than
- **$lt:** less than
- **$gte:** greater than or equal to
- **$lte:** less than or equal to
- **EXAMPLE:** db.trips.find({ "tripduration": { "$lte" : 70 }, "usertype": { "$ne": "Subscriber" } }).pretty()

**Logic Operators**

- $or: match one query clause
- $nor: anything doat doesn't match both query clauses (kind of like a combination of $not and $or)
  - These three share common syntax by using an array. Example: {"$logic":[{filter1}, {filter2}] }
- $and: match all query clauses
  - A lot of times $and is used implicitly without you needing to use it. Example:
    - {"$and": [{"student_id": {"$gt": 25} }, {"student_id": {"$lt": 100}} ]} versus
    - {"student_id": {"$gt": 25, "$lt": 100}}
    - Generally speaking you only need to use $and when you use the same operator more than once
- $not - negates the query requriement. Example:
  - $not syntax example: `{"student_id": {"$not": {"$gt": 25}} }`

**$expr**

- $expr allows the use of aggregation expressions (More on that later)
- $expr allows for the use of variables. You just have to use a $ to denote what the variable is. Example:
  - `db.trips.find({ "$expr": { "$eq": [ "$end station id", "$start station id"] }}).count()`

### Create

### Read

- To **GET** a single document: `const document = await collection.findOne(query, options);`
- To **GET** multiple document: ` const cursor = collection.find(query, options).toArrau();`

### Update

### Delete

## Other

### Miscellaneous Queries

- Get a list of all Databases -> `const databases = await client.db().admin().listDatabases();`
- Get a list of all collections in a database -> `const db = client.db("dbName"); let collections = await db.listCollections().toArray();`

### Next.js + MongoDB

- Here's a [semi-good example](https://github.com/hoangvvo/nextjs-mongodb-app) of combining Next.js with mongodb
