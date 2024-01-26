 

# MongoDB 

#### Introduction

###### MongoDB is a popular NoSQL database that provides a flexible and scalable solution for storing and retrieving data. In this tutorial, we'll cover the basics of MongoDB, including installation, CRUD operations, querying, and more.

Installation
##### To install MongoDB, follow the instructions on the official MongoDB installation guide.
Connecting to MongoDB
##### After installation, start the MongoDB server and connect to it using the MongoDB shell or a MongoDB client. Use the following command to connect to the local MongoDB instance
   
    mongo
Creating a Database
##### To create a new database, use the use command. Replace <database_name> with your desired database name
    use <database_name>
Creating a Collection
##### Collections in MongoDB are equivalent to tables in relational databases. To create a new collection, use the db.createCollection() method.
    db.createCollection("<collection_name>")
Inserting Documents
 ##### Use the insertOne() or insertMany() method to insert documents into a collection.
    db.<collection_name>.insertMany([
      {
        key1: "value1",
        key2: "value2",
      },
      {
        key1: "value3",
        key2: "value4",
      },
    ]);
Querying Documents
##### MongoDB supports various query operators for filtering documents. Use the find() method to query documents.
    // Find all documents in a collection
    db.<collection_name>.find()

    // Find documents matching a specific condition
    db.<collection_name>.find({ key1: "value1" })
Updating Documents
##### Use the updateOne() or updateMany() method to update documents in a collection.
    // Update a single document
    db.<collection_name>.updateOne(
      { key1: "value1" },
      { $set: { key2: "new_value" } }
    )

    // Update multiple documents
    db.<collection_name>.updateMany(
      { key1: "value1" },
      { $set: { key2: "new_value" } }
    )
Deleting Documents
##### To delete documents from a collection, use the deleteOne() or deleteMany() method.
    // Delete a single document
    db.<collection_name>.deleteOne({ key1: "value1" })

    // Delete multiple documents
    db.<collection_name>.deleteMany({ key1: "value1" })
