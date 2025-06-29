# DockerizedMongoDB

## Getting Started

This guide will help you set up a MongoDB database using Docker Compose with automated initialization and interact with it using `mongosh`.

### Prerequisites

- [Docker](https://www.docker.com/get-started) and Docker Compose installed on your machine
- [mongosh](https://www.mongodb.com/try/download/shell) (MongoDB Shell) installed

### Project Structure

- `docker-compose.yml`: Docker Compose configuration
- `Dockerfile`: Custom MongoDB image with initialization
- `init-mongo.js`: MongoDB initialization script that creates a user and sample data
- `README.md`: This guide

### 1. Start MongoDB with Docker Compose

```bash
docker-compose up -d
```

This will:
- Build a custom MongoDB image (mongo:7.0 based)
- Create a container named `mongo-dev`
- Set up persistent data storage with a Docker volume
- Run the initialization script that creates:
  - A database called `mydatabase`
  - A user `myuser` with password `mypassword`
  - Sample data in `sampleCollection`

### 2. Connect to MongoDB Using mongosh

#### Option 1: Connect as admin user
```bash
mongosh "mongodb://admin:adminpass@localhost:27017"
```

#### Option 2: Connect as the application user
```bash
mongosh "mongodb://myuser:mypassword@localhost:27017/mydatabase"
```

### 3. Query the Pre-loaded Data

The initialization script automatically creates sample data. You can query it:

```js
use mydatabase
db.sampleCollection.find()
```

**Expected Output:**
```json
[
    { _id: ObjectId("..."), name: "Alice", age: 30 },
    { _id: ObjectId("..."), name: "Bob", age: 25 }
]
```

### 4. Working with Additional Data

You can add more collections and data as needed:

```js
db.createCollection("users")
db.users.insertMany([
    { name: "Charlie", age: 35 },
    { name: "Diana", age: 28 }
])
```

### 5. Stop and Clean Up

```bash
# Stop the containers
docker-compose down

# Stop and remove containers with volumes (removes all data)
docker-compose down -v
```


### Credentials

- **Admin User**: `admin` / `adminpass`
- **Application User**: `myuser` / `mypassword` (has readWrite access to `mydatabase`)
- **Database**: `mydatabase`
- **Container Name**: `mongo-dev`
- **Port**: `27017`

### Troubleshooting

#### Check container status
```bash
docker-compose ps
```

#### View container logs
```bash
docker-compose logs mongo
```

#### Rebuild after changing Dockerfile or init script
```bash
docker-compose down
docker-compose build --no-cache
docker-compose up -d
```

---

For more details, see the [MongoDB Docker documentation](https://hub.docker.com/_/mongo).