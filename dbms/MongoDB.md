# MongoDB

## General
- A database based on a non-relational document model (**NoSQL**)
- Designed for ease of application development and scaling
- Documents are stored in collections which live in databases
- Databases are only created when we add data to them
- Documents are in **BSON** format which is a binary form of **JSON**
- Documents don't have to contain same fields (*non-homogeneous*)
- There is no need to create a *schema* (columns, etc.)

## MongoDB Shell (mongosh)
The **MongoDB Shell**, `mongosh`, is a fully functional **JavaScript** and **Node.js 16.x REPL** environment for interacting with **MongoDB** deployments. We can use the **MongoDB Shell**
to test queries and operations directly with our database.

### Commands
- `show dbs` - print a list of all databases on the server
- `show collections` - print a list of all collections for current database
- `show users` - print a list of users for current database
- `db` - display the current database
- `use <db>` - switch current database to `<db>`
- `use <new-db>` - create the new database `<new-db>`

### Operations
- `db.collection.find()` - find all documents in the collection and return a cursor
- `db.collection.insertOne()` - insert a new document into the collection
- `db.collection.insertMany()` - insert multiple new documents into the collection
- `db.collection.updateOne()` -  update a single existing document in the collection
- `db.collection.updateMany()` - update multiple existing documents in the collection
- `db.collection.deleteOne()` - delete a single document from the collection
- `db.collection.deleteMany()` - delete multiple documents from the collection
- `db.collection.drop()` - drops or removes the collection completely
- `db.dropDatabase()` - removes the current database, deleting the associated data files

## CRUD operations
**CRUD** operations *create*, *read*, *update*, and *delete* documents.

### Insert documents
The **MongoDB Shell** provides the following methods to insert documents into a collection:

- `db.collection.insertOne()` - insert a single document into a collection
- `db.collection.insertMany()` - insert multiple documents into a collection

#### Insert a single document
`db.collection.insertOne()` inserts a *single* document into a collection. If  the document does not specify an `_id`
field, **MongoDB** adds the `_id` field with an `ObjectId` value to the new document.

`insertOne()` returns a document that includes the newly inserted document's `_id` field value.

**Example:**

```javascript
db.movies.insertOne(
  {
    title: "The Favourite",
    genres: ["Drama", "History"],
    runtime: 121,
    rated: "R",
    year: 2018,
    directors: ["Yorgos Lanthimos"],
    cast: ["Olivia Colman", "Emma Stone", "Rachel Weisz"],
    type: "movie"
  }
)
```

#### Insert multiple documents
`db.collection.insertMany()` can insert *multiple* documents into a collection. Accepts an array of documents. If the
documents do not specify an `_id` field, **MongoDB** adds the `_id` field with an `ObjectId` value to each document.

`insertMany()` returns a document that includes the newly inserted documents' `_id` field values.

**Example:**

```javascript
db.movies.insertMany([
   {
      title: "Jurassic World: Fallen Kingdom",
      genres: ["Action", "Sci-Fi"],
      runtime: 130,
      rated: "PG-13",
      year: 2018,
      directors: [ "J. A. Bayona" ],
      cast: ["Chris Pratt", "Bryce Dallas Howard", "Rafe Spall"],
      type: "movie"
    },
    {
      title: "Tag",
      genres: ["Comedy", "Action"],
      runtime: 105,
      rated: "R",
      year: 2018,
      directors: ["Jeff Tomsic"],
      cast: ["Annabelle Wallis", "Jeremy Renner", "Jon Hamm"],
      type: "movie"
    }
])
```

### Query documents
We can use the `db.collection.find()` method in the **MongoDB Shell** to query documents in a collection.

#### Comparison query operators
- `$eq` - matches values that are equal to a specified value
- `$ne` - matches values that are not equal to a specified value
- `$gt` - matches values that are greater than a specified value
- `$gte` - matches values that are greater than or equal to a specified value
- `$lt` - matches values that are less than a specified value
- `$lte` - matches values that are less than or equal to a specified value
- `$in` - matches any of the values specified in an array
- `$nin` - matches none of the values specified in an array

#### Logical query operators
- `$and` - joins query clauses with a logical **AND**; returns all documents that match the conditions of both
  clauses
- `$or` - joins query clauses with a logical **OR**; returns all documents that match the conditions of either clause
- `$nor` - joins query clauses with a logical **NOR**; returns all documents that fail to match both clauses
- `$not` - inverts the effect of a query expression; returns documents that do **NOT** match the query expression

#### Element query operators
- `$exists` - matches documents that have the specified field (even if it has a `null` value)
- `$type` - selects documents if a field is of the specified type

#### Examples

- ***<ins>Return all users</ins>:***
  ```javascript
  db.users.find()
  ```

- ***<ins>Return all users whose `name` is `Kyle`</ins>:***
  ```javascript
  db.users.find({ name: "Kyle" })
  ```

- ***<ins>Return the first 2 users</ins>:***
  ```javascript
  db.users.find().limit(2)
  ```

- ***<ins>Return all users sorted by their `name` in ascending order</ins>:***
  ```javascript
  db.users.find().sort({ name: 1 })
  ```

- ***<ins>Return all users sorted by their `name` in descending order</ins>:***
  ```javascript
  db.users.find().sort({ name: -1 })
  ```

- ***<ins>Return all users sorted first by `age` in ascending order and then by `name` in descending order</ins>:***
  ```javascript
  db.users.find().sort({ age: 1, name: -1 })
  ```

- ***<ins>Return all users but the first</ins>:***
  ```javascript
  db.users.find().skip(1)
  ```

- ***<ins>Return the `name` and `age` of users who are 26 years old</ins>:***
  ```javascript
  db.users.find({ age: "26" }, { name: 1, age: 1 })
  ```

- ***<ins>Return the `name` and `age` of users who are 26 years old but don't include the `_id` field</ins>:***
  ```javascript
  db.users.find({ age: "26" }, { name: 1, age: 1, _id: 0 })
  ```

- ***<ins>Return all fields of users who are 26 years old except their `age`</ins>:***
  ```javascript
  db.users.find({ age: "26" }, { age: 0 })
  ```

- ***<ins>Return all users with ages between 20 and 40</ins>:***
  ```javascript
  db.users.find({ age: { $gte: 20, $lte: 40 })
  ```

- ***<ins>Return all users who are 20 or younger AND whose `name` is `Kyle`</ins>:***
  ```javascript
  db.users.find({ age: { $lte: 20 }, name: "Kyle" })
  ```

- ***<ins>Return all users who are 20 or younger OR whose `name` is `Kyle`</ins>:***
  ```javascript
  db.users.find({ $or: [{ age: { $lte: 20 } }, { name: "Kyle" }] })
  ```

- ***<ins>Return all users whose `name` is in the specified list of names</ins>:***
  ```javascript
  db.users.find({ name: { $in: ["Kyle", "Sally"] } })
  ```

- ***<ins>Return all users in debt</ins>:***
  ```javascript
  db.users.find({ $expr: { $gt: ["$debt", "$balance"] } })
  ```

- ***<ins>Return all users whose street address is `123 Main St`</ins>:***
  ```javascript
  db.users.find({ "address.street": "123 Main St" })
  ```

- ***<ins>Return the first user who is 40 or younger</ins>:***
  ```javascript
  db.users.findOne({ age: { $lte: 40 }})
  ```

- ***<ins>Count the users who are 40 or younger</ins>:***
  ```javascript
  db.users.countDocuments({ age: { $lte: 40 }})
  ```

### Update documents
The **MongoDB Shell** provides the following methods to update documents in a collection:

- `db.collection.updateOne()` - update a single document
- `db.collection.updateMany()` - update multiple documents
- `db.collection.replaceOne()` - replace a document

To update a document **MongoDB** provides update operators, such as `$set`, to modify field values.

To use the update operators, we pass to the update methods an update document of the form:

```javascript
{
  <update operator>: { <field1>: <value1>, ... },
  <update operator>: { <field2>: <value2>, ... },
  ...
}
```

Some update operators, such as `$set`, create the field if the field does not exist.

Common update operators include:

- `$set` - sets the value of a field in a document
- `$unset` - removes the specified field from a document
- `$inc` - increments the value of the field by the specified amount
- `$rename` - renames a field
- `$push` - adds an item to an array
- `$pop` - removes the first or last item of an array
- `$pull` - removes all array elements that match a specified query

#### Update a single document
We can use the `db.collection.updateOne()` method to update the first document that matches a specified filter.

**Example:**

```javascript
db.users.updateOne({ age: 26 }, { $set: { age: 27 } })
```

#### Update multiple documents
We can use the `db.collection.updateMany()` method to update all documents that match a specified filter.

**Example:**

```javascript
db.users.updateMany({ address: { $exists: true } }, { $unset: { address: "" } })
```

#### Replace a document
To replace the entire content of a document except for the `_id` field, we can pass an entirely new document as the
second argument to `db.collection.replaceOne()`.

When replacing a document, the replacement document must contain only field/value pairs, not update operator
expressions.

The replacement document can have different fields from the original document. We can omit the `_id` field since it is
immutable; however, if we include it, it must have the same value as the current value.

**Example:**

```javascript
db.users.replaceOne({ age: 30 }, { name: "John" })
```

### Delete documents
The **MongoDB Shell** provides the following methods to delete documents from a collection:

- `db.collection.deleteOne()` - delete a single document
- `db.collection.deleteMany()` - delete multiple documents

We can specify criteria, or filters, that identify the documents to delete. The filters use the same syntax as query
operations.

#### Delete a single document
To delete at most a single document that matches a specified filter (even though multiple documents may match the
specified filter) we can use the `db.collection.deleteOne()` method.

**Example:**
```javascript
db.users.deleteOne({ name: "John" })
```

#### Delete multiple documents
To delele multiple documents from a collection, we can specify criteria, or filters, that identify the documents to
delete. The filters use the same syntax as read operations.

**Example:**

```javascript
db.users.deleteMany({ age: { $exists: false } })
```

#### Delete all documents
To delete all documents from a collection, we can pass an empty filter document `{}` to the `db.collection.deleteMany()`
method.

The method returns a document with the status of the operation.

**Example:**

```javascript
db.users.deleteMany({})
```
