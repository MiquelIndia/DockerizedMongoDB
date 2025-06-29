# DockerizedMongoDB

## Getting Started

This guide will help you set up a MongoDB database using Docker and interact with it using `mongosh`.

### Prerequisites

- [Docker](https://www.docker.com/get-started) installed on your machine
- [mongosh](https://www.mongodb.com/try/download/shell) (MongoDB Shell) installed

### 1. Create and Run MongoDB Container

```bash
docker run --name my-mongo -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=secret mongo:latest
```

- `my-mongo`: Name of the container
- `admin`: MongoDB root username
- `secret`: MongoDB root password

### 2. Connect to MongoDB Using mongosh

```bash
mongosh "mongodb://admin:secret@localhost:27017"
```

### 3. Create a Database and Collection

```js
use mydatabase
db.createCollection("users")
```

### 4. Insert Example Data

```js
db.users.insertMany([
    { name: "Alice", age: 30 },
    { name: "Bob", age: 25 }
])
```

### 5. Query the Data

```js
db.users.find()
```

**Expected Output:**
```json
[
    { _id: ObjectId("..."), name: "Alice", age: 30 },
    { _id: ObjectId("..."), name: "Bob", age: 25 }
]
```

### 6. Stop and Remove the Container

```bash
docker stop my-mongo
docker rm my-mongo
```

---

For more details, see the [MongoDB Docker documentation](https://hub.docker.com/_/mongo).