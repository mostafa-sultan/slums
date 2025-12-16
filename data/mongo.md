# MongoDB Tutorial

## Introduction

MongoDB is a popular NoSQL database that provides a flexible and scalable solution for storing and retrieving data. It stores data in flexible, JSON-like documents, making it ideal for modern applications. In this comprehensive tutorial, we'll cover the basics of MongoDB, including installation, CRUD operations, querying, indexing, aggregation, and more.

---

## What is MongoDB?

MongoDB is a document-oriented NoSQL database that stores data in BSON (Binary JSON) format. Unlike relational databases, MongoDB doesn't require a fixed schema, allowing for more flexible data modeling.

### Key Features

- **Document-based**: Data stored as documents (similar to JSON)
- **Schema-less**: No fixed schema required
- **Scalable**: Horizontal scaling with sharding
- **High Performance**: Fast read/write operations
- **Rich Query Language**: Powerful querying capabilities
- **Indexing**: Support for various index types
- **Aggregation**: Complex data processing pipelines
- **Replication**: High availability with replica sets

---

## Installation

### Installing MongoDB

**Windows:**
1. Download MongoDB Community Server from [mongodb.com](https://www.mongodb.com/try/download/community)
2. Run the installer and follow the setup wizard
3. MongoDB will be installed as a Windows service

**macOS:**
    brew tap mongodb/brew
    brew install mongodb-community

**Linux (Ubuntu/Debian):**
    sudo apt-get install -y mongodb

### Starting MongoDB

**Windows:**
    # MongoDB runs as a service automatically

**macOS/Linux:**
    brew services start mongodb-community
    # or
    mongod --config /usr/local/etc/mongod.conf

### Verifying Installation

    mongod --version
    mongo --version

---

## Connecting to MongoDB

### MongoDB Shell (mongosh)

After installation, start the MongoDB shell and connect to it using the following command:

    mongosh

This connects to the local MongoDB instance running on the default port (27017).

### Connection String

    mongosh "mongodb://localhost:27017"

### Connection with Authentication

    mongosh "mongodb://username:password@localhost:27017/database"

---

## Basic Concepts

### Database

A database is a container for collections. MongoDB can have multiple databases.

### Collection

A collection is a group of documents (similar to a table in SQL databases).

### Document

A document is a set of key-value pairs (similar to a row in SQL databases). Documents are stored in BSON format.

### Field

A field is a key-value pair in a document (similar to a column in SQL databases).

---

## Creating a Database

To create a new database, use the `use` command. Replace `<database_name>` with your desired database name:

    use <database_name>

**Note:** The database is not actually created until you insert data into it.

**Example:**

    use myapp
    // Database switched to myapp

---

## Creating a Collection

Collections in MongoDB are equivalent to tables in relational databases. To create a new collection, use the `db.createCollection()` method:

    db.createCollection("<collection_name>")

**Example:**

    db.createCollection("users")
    db.createCollection("products")

**Note:** Collections are also created automatically when you first insert a document.

---

## Inserting Documents

### insertOne()

Use the `insertOne()` method to insert a single document into a collection:

    db.<collection_name>.insertOne({
      key1: "value1",
      key2: "value2",
    })

**Example:**

    db.users.insertOne({
      name: "John Doe",
      email: "john@example.com",
      age: 30,
      city: "New York"
    })

### insertMany()

Use the `insertMany()` method to insert multiple documents:

    db.<collection_name>.insertMany([
      {
        key1: "value1",
        key2: "value2",
      },
      {
        key1: "value3",
        key2: "value4",
      },
    ])

**Example:**

    db.users.insertMany([
      {
        name: "Jane Smith",
        email: "jane@example.com",
        age: 25,
        city: "Los Angeles"
      },
      {
        name: "Bob Johnson",
        email: "bob@example.com",
        age: 35,
        city: "Chicago"
      }
    ])

### Return Values

Both methods return an object with:
- `insertedId`: The _id of the inserted document(s)
- `acknowledged`: Boolean indicating success

---

## Querying Documents

### find()

Use the `find()` method to query documents from a collection.

#### Find All Documents

    // Find all documents in a collection
    db.<collection_name>.find()

**Example:**

    db.users.find()

#### Find Documents Matching a Condition

    // Find documents matching a specific condition
    db.<collection_name>.find({ key1: "value1" })

**Example:**

    db.users.find({ name: "John Doe" })
    db.users.find({ age: { $gt: 25 } })  // age greater than 25

#### Pretty Print Results

    db.users.find().pretty()

### findOne()

Returns a single document that matches the query:

    db.users.findOne({ name: "John Doe" })

### Query Operators

#### Comparison Operators

    // Greater than
    db.users.find({ age: { $gt: 25 } })
    
    // Greater than or equal
    db.users.find({ age: { $gte: 25 } })
    
    // Less than
    db.users.find({ age: { $lt: 30 } })
    
    // Less than or equal
    db.users.find({ age: { $lte: 30 } })
    
    // Not equal
    db.users.find({ age: { $ne: 25 } })
    
    // In array
    db.users.find({ age: { $in: [25, 30, 35] } })
    
    // Not in array
    db.users.find({ age: { $nin: [25, 30] } })

#### Logical Operators

    // AND
    db.users.find({ $and: [{ age: { $gt: 25 } }, { city: "New York" }] })
    
    // OR
    db.users.find({ $or: [{ age: { $gt: 30 } }, { city: "Los Angeles" }] })
    
    // NOT
    db.users.find({ age: { $not: { $gt: 25 } } })
    
    // NOR
    db.users.find({ $nor: [{ age: { $gt: 30 } }, { city: "Chicago" }] })

#### Element Operators

    // Exists
    db.users.find({ email: { $exists: true } })
    
    // Type
    db.users.find({ age: { $type: "number" } })

#### Array Operators

    // All elements match
    db.users.find({ tags: { $all: ["developer", "javascript"] } })
    
    // Array size
    db.users.find({ tags: { $size: 3 } })
    
    // Element matches
    db.users.find({ tags: "developer" })

#### String Operators

    // Regex
    db.users.find({ name: { $regex: /^John/, $options: "i" } })
    
    // Text search (requires text index)
    db.users.find({ $text: { $search: "developer" } })

### Projection

Select only specific fields:

    // Include specific fields
    db.users.find({}, { name: 1, email: 1 })
    
    // Exclude specific fields
    db.users.find({}, { password: 0, _id: 0 })
    
    // Include all except some
    db.users.find({}, { name: 1, email: 1, _id: 0 })

### Sorting

    // Sort ascending
    db.users.find().sort({ age: 1 })
    
    // Sort descending
    db.users.find().sort({ age: -1 })
    
    // Sort by multiple fields
    db.users.find().sort({ age: 1, name: 1 })

### Limiting Results

    // Limit number of results
    db.users.find().limit(5)
    
    // Skip documents
    db.users.find().skip(10)
    
    // Combine
    db.users.find().skip(10).limit(5)

### Counting Documents

    // Count all documents
    db.users.countDocuments()
    
    // Count with condition
    db.users.countDocuments({ age: { $gt: 25 } })
    
    // Estimated count (faster, less accurate)
    db.users.estimatedDocumentCount()

---

## Updating Documents

### updateOne()

Update a single document:

    // Update a single document
    db.<collection_name>.updateOne(
      { key1: "value1" },
      { $set: { key2: "new_value" } }
    )

**Example:**

    db.users.updateOne(
      { name: "John Doe" },
      { $set: { age: 31 } }
    )

### updateMany()

Update multiple documents:

    // Update multiple documents
    db.<collection_name>.updateMany(
      { key1: "value1" },
      { $set: { key2: "new_value" } }
    )

**Example:**

    db.users.updateMany(
      { age: { $lt: 30 } },
      { $set: { status: "young" } }
    )

### Update Operators

#### $set

Set the value of a field:

    db.users.updateOne(
      { name: "John Doe" },
      { $set: { age: 31, city: "Boston" } }
    )

#### $unset

Remove a field:

    db.users.updateOne(
      { name: "John Doe" },
      { $unset: { city: "" } }
    )

#### $inc

Increment a numeric field:

    db.users.updateOne(
      { name: "John Doe" },
      { $inc: { age: 1 } }
    )

#### $push

Add element to array:

    db.users.updateOne(
      { name: "John Doe" },
      { $push: { tags: "developer" } }
    )

#### $pull

Remove element from array:

    db.users.updateOne(
      { name: "John Doe" },
      { $pull: { tags: "developer" } }
    )

#### $addToSet

Add element to array if not exists:

    db.users.updateOne(
      { name: "John Doe" },
      { $addToSet: { tags: "developer" } }
    )

#### $pop

Remove first or last element from array:

    db.users.updateOne(
      { name: "John Doe" },
      { $pop: { tags: 1 } }  // 1 = last, -1 = first
    )

### replaceOne()

Replace entire document:

    db.users.replaceOne(
      { name: "John Doe" },
      { name: "John Doe", email: "newemail@example.com", age: 31 }
    )

### Upsert

Insert if document doesn't exist:

    db.users.updateOne(
      { email: "newuser@example.com" },
      { $set: { name: "New User", age: 25 } },
      { upsert: true }
    )

---

## Deleting Documents

### deleteOne()

Delete a single document:

    // Delete a single document
    db.<collection_name>.deleteOne({ key1: "value1" })

**Example:**

    db.users.deleteOne({ name: "John Doe" })

### deleteMany()

Delete multiple documents:

    // Delete multiple documents
    db.<collection_name>.deleteMany({ key1: "value1" })

**Example:**

    db.users.deleteMany({ age: { $lt: 18 } })

### Delete All Documents

    db.users.deleteMany({})

**Note:** This doesn't delete the collection itself.

---

## Indexes

Indexes improve query performance by allowing MongoDB to find documents more efficiently.

### Creating Indexes

    // Single field index
    db.users.createIndex({ email: 1 })  // 1 = ascending, -1 = descending
    
    // Compound index
    db.users.createIndex({ name: 1, age: -1 })
    
    // Unique index
    db.users.createIndex({ email: 1 }, { unique: true })
    
    // Text index
    db.users.createIndex({ name: "text", description: "text" })
    
    // TTL index (expires after specified seconds)
    db.sessions.createIndex({ createdAt: 1 }, { expireAfterSeconds: 3600 })

### Listing Indexes

    db.users.getIndexes()

### Dropping Indexes

    // Drop specific index
    db.users.dropIndex({ email: 1 })
    
    // Drop all indexes (except _id)
    db.users.dropIndexes()

---

## Aggregation Pipeline

Aggregation allows you to process data and return computed results.

### Basic Aggregation

    db.users.aggregate([
      { $match: { age: { $gt: 25 } } },
      { $group: { _id: "$city", count: { $sum: 1 } } },
      { $sort: { count: -1 } }
    ])

### Common Aggregation Stages

#### $match

Filter documents:

    { $match: { age: { $gt: 25 } } }

#### $group

Group documents:

    {
      $group: {
        _id: "$city",
        totalAge: { $sum: "$age" },
        avgAge: { $avg: "$age" },
        count: { $sum: 1 }
      }
    }

#### $project

Reshape documents:

    {
      $project: {
        name: 1,
        age: 1,
        isAdult: { $gte: ["$age", 18] }
      }
    }

#### $sort

Sort documents:

    { $sort: { age: -1 } }

#### $limit

Limit documents:

    { $limit: 10 }

#### $skip

Skip documents:

    { $skip: 5 }

#### $unwind

Deconstruct array field:

    { $unwind: "$tags" }

#### $lookup

Join collections:

    {
      $lookup: {
        from: "orders",
        localField: "_id",
        foreignField: "userId",
        as: "orders"
      }
    }

### Aggregation Operators

    // Arithmetic
    { $add: ["$price", "$tax"] }
    { $subtract: ["$price", "$discount"] }
    { $multiply: ["$price", "$quantity"] }
    { $divide: ["$total", "$count"] }
    
    // Comparison
    { $eq: ["$age", 25] }
    { $gt: ["$age", 25] }
    { $lt: ["$age", 25] }
    
    // String
    { $concat: ["$firstName", " ", "$lastName"] }
    { $toUpper: "$name" }
    { $toLower: "$email" }
    
    // Date
    { $year: "$date" }
    { $month: "$date" }
    { $dayOfMonth: "$date" }

---

## Relationships

### Embedded Documents

Store related data in the same document:

    db.users.insertOne({
      name: "John Doe",
      address: {
        street: "123 Main St",
        city: "New York",
        zip: "10001"
      }
    })

### References

Store references to other documents:

    // Users collection
    db.users.insertOne({
      _id: 1,
      name: "John Doe"
    })
    
    // Orders collection
    db.orders.insertOne({
      userId: 1,
      product: "Laptop",
      price: 999
    })
    
    // Join using $lookup
    db.orders.aggregate([
      {
        $lookup: {
          from: "users",
          localField: "userId",
          foreignField: "_id",
          as: "user"
        }
      }
    ])

---

## Working with MongoDB in Node.js

### Installation

    npm install mongodb

### Basic Connection

    const { MongoClient } = require('mongodb');
    
    const uri = 'mongodb://localhost:27017';
    const client = new MongoClient(uri);
    
    async function connect() {
      try {
        await client.connect();
        console.log('Connected to MongoDB');
        
        const db = client.db('myapp');
        const collection = db.collection('users');
        
        // Insert document
        await collection.insertOne({
          name: 'John Doe',
          email: 'john@example.com'
        });
        
        // Find documents
        const users = await collection.find({}).toArray();
        console.log(users);
        
      } catch (error) {
        console.error('Error:', error);
      } finally {
        await client.close();
      }
    }
    
    connect();

### Using Mongoose (ODM)

    // Installation
    // npm install mongoose
    
    const mongoose = require('mongoose');
    
    // Connect
    mongoose.connect('mongodb://localhost:27017/myapp', {
      useNewUrlParser: true,
      useUnifiedTopology: true
    });
    
    // Define schema
    const userSchema = new mongoose.Schema({
      name: { type: String, required: true },
      email: { type: String, required: true, unique: true },
      age: Number,
      createdAt: { type: Date, default: Date.now }
    });
    
    // Create model
    const User = mongoose.model('User', userSchema);
    
    // Create user
    const user = new User({
      name: 'John Doe',
      email: 'john@example.com',
      age: 30
    });
    await user.save();
    
    // Find users
    const users = await User.find();
    const user = await User.findById(userId);
    const user = await User.findOne({ email: 'john@example.com' });
    
    // Update user
    await User.findByIdAndUpdate(userId, { age: 31 });
    
    // Delete user
    await User.findByIdAndDelete(userId);

---

## Best Practices

1. **Use indexes** for frequently queried fields
2. **Design schemas** based on query patterns
3. **Use embedded documents** for one-to-one or one-to-few relationships
4. **Use references** for one-to-many or many-to-many relationships
5. **Limit document size** (16MB max)
6. **Use projection** to limit returned fields
7. **Use aggregation** for complex queries
8. **Implement proper error handling**
9. **Use connection pooling**
10. **Monitor performance** and optimize queries
11. **Use transactions** for multi-document operations
12. **Implement proper security** (authentication, authorization)
13. **Regular backups**
14. **Use replica sets** for high availability

---

## Security

### Authentication

    // Create user
    use admin
    db.createUser({
      user: "admin",
      pwd: "password",
      roles: ["root"]
    })
    
    // Connect with authentication
    mongosh "mongodb://admin:password@localhost:27017"

### Authorization

    // Create user with specific permissions
    use myapp
    db.createUser({
      user: "appuser",
      pwd: "password",
      roles: [
        { role: "readWrite", db: "myapp" }
      ]
    })

---

## Backup and Restore

### Backup

    mongodump --db myapp --out /backup

### Restore

    mongorestore --db myapp /backup/myapp

---

## Conclusion

MongoDB is a powerful and flexible NoSQL database that's well-suited for modern applications. This tutorial covered the fundamentals, but there's much more to learn including advanced aggregation, sharding, replication, and performance optimization. Continue practicing and building projects to master MongoDB.
