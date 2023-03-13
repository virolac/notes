# Mongoose

## Installation
```sh
npm install mongoose --save
```

## Connect to a MongoDB database
The first thing we need to do is connect to a **MongoDB** database:

```javascript
const mongoose = require("mongoose");

mongoose.connect(
  "mongodb:localhost/testdb",
  () => {
    console.log("connected");
  },
  (err) => console.error(e)
);
```

**Mongoose** will queue up commands and only run them after the connection to the database is established.

## Main concepts
There are 3 main concepts in **Mongoose**:
- `schema` - what the structure of the data looks like
- `model` - the `schema` in an actual form that we can use
- `query` - a **CRUD** operation performed on a `model`

### Schema
Everything in **Mongoose** starts with a `Schema`. Each schema maps to a **MongoDB** collection and defines the shape of
the documents within that collection.

```javascript
const mongoose = require("mongoose");

const blogSchema = new mongoose.Schema({
  title: String, // String is shorthand for { type: String }
  author: {
    type: String,
    minLength: 5,
    required: true,
    immutable: true,
    uppercase: true // lowercase also available
  },
  body: String,
  comments: [{ body: String, date: Date }],
  date: { type: Date, default: Date.now },
  hidden: Boolean,
  meta: {
    votes: {
      type: Number,
      min: 1,
      max: 8000000000
    }
    favs: Number
  }
});
```

Each key defines a property in our documents which will be cast to its associated `SchemaType`.

The permitted `SchemaType`s are:

- `String`
- `Number`
- `Date`
- `Buffer`
- `Boolean`
- `Mixed`
- `ObjectId`
- `Array`
- `Decimal128`
- `Map`

By default, **Mongoose** adds an `_id` property to our schemas.

### Model
Models are fancy constuctors compiled from `Schema` definitions. An instance of a model is called a `document`. Models
are responsible for creating and reading documents from the underlying **MongoDB** database.

To create a model from a schema, we pass it into `mongoose.model(modelName, schema)`:

```javascript
const User = mongoose.model("User", userSchema);

const user = new User({ name: "Kyle", age: 26 });

user.save().then(() => console.log("User Saved"));
                    //or
const user = await User.create({ name: "Kyle", age: 26 });
```

### Queries
**Mongoose** [models](#model) provide several static helper functions for **CRUD** operations. Each of these functions
returns a **Mongoose** `Query` object.

- `Model.find()`
- `Model.findById()`
- `Model.findByIdAndUpdate()`
- `Model.findByIdAndDelete()`
- `Model.findByIdAndRemove()`
- `Model.findOne()`
- `Model.findOneAndUpdate()`
- `Model.findOneAndReplace()`
- `Model.findOneAndDelete()`
- `Model.findOneAndRemove()`
- `Model.findOneAndDelete()`
- `Model.updateOne()`
- `Model.updateMany()`
- `Model.replaceOne()`
- `Model.deleteOne()`
- `Model.deleteMany()`

A query can accept a callback for processing the results but also has a `.then()` function, and thus can be used as a
promise.

```javascript
// find each person with a last name matching `Ghost`
const query = Person.findOne({ "name.last": "Ghost" });

// select the `name` and `occupation` fields
query.select("name occupation");

// execute the query at a later time
query.exec(function (err, person) {
  if (err) return handleError(err);
  // Prints "Space Ghost is a talk show host."
  console.log("%s %s is a %s.", person.name.first, person.name.last. person.occupation);
});
```

A `Query` also enables us to build up a query using chaining syntax, rather than specifying a **JSON** object:

```javascript
Person
  .find({ occupation: /host/ })
  .where("name.last").equals("Ghost")
  .where("age").gt(17).lt(66)
  .where("likes").in(["vaporizing", "talking"])
  .limit(10)
  .sort("-occupation")
  .select("name occupation")
  .exec(callback);
```

**Mongoose** queries are **NOT** promises. They have a `.then()` function and `async/await` as a convenience. However,
unlike promises, calling a query's `.then()` can execute the query multiple times.


## Custom validation
We can easily add custom validation to our schema:

```javascript
const personSchema = mongoose.Schema({
  name: {
    type: Number,
    min: 1,
    max: 100,
    validate: {
      validator: value => value % 2 === 0,
      message: props => `${props.value} is not an even number`
    }
  },
  ...
});
```

We should avoid methods that ignore the validation (findOneAndDelete, updateOne, etc.). Instead, we should use
`findById()` or `find()` combined with `save()`. The `deleteOne()` and `deleteMany()` methods are also fine.

## References to other documents
Using `populate()` we can reference documents in other collections, similar to **SQL** joins.

```javascript
const mongoose = require("mongoose");

const personSchema = mongoose.Schema({
  _id: mongoose.SchemaTypes.ObjectId,
  name: String,
  age: Number,
  stories: [{
    type: mongoose.SchemaTypes.ObjectId,
    ref: "Story"
  }]
});

const storySchema = mongoose.Schema({
  author: { type: mongoose.SchemaTypes.ObjectId, ref: "Person" },
  title: String,
  fans: [{ type: mongoose.SchemaTypes.ObjectId, ref: "Person" }]
});

const Story = mongoose.model("Story", storySchema);
const Person = mongoose.model("Person", personSchema);

Story
  .findOne({ title: "Casino Royale" })
  .populate("author")
  .exec(function (err, story) {
    if (err) return handleError(err);
    // Prints "The author is Ian Fleming"
    console.log("The author is %s", story.author.name);
  });
```

Populate paths are no longer set to their original `_id`; their value is replaced with the document returned from the
database by performing a separate query before returning the results.

## Instance methods
We can define our own custom document instance methods:

```javascript
const animalSchema = new mongoose.Schema({
  name: String,
  type: String
});

animalSchema.methods.findSimilarType = function (cb) {
  return mongoose.model("Animal").find({ type: this.type }, cb);
};
```

Now when we have an intsance of `Animal` we can call our `findSimilarType` method and find all animals with a matching
`type`.

```javascript
const Animal = mongoose.model("Animal", animalSchema);
const dog = new Animal({ name: "Rover", type: "dog" });

dog.findSimilarType(function (err, dogs) {
  if (err) return ...
  dogs.forEach(...);
});
```

## Statics
Statics are pretty much the same as methods but allow for defining functions that exist directly on our `model`:

```javascript
animalSchema.statics.findByName = function (name) {
  return this.find({ name: new RegExp(name, "i") });
};

const Animal = mongoose.model("Animal", animalSchema);
const animals = await Animal.findByName("fido");

console.log(animals);
```

## Query helpers
We can also add query helper functions, which are like instance methods but for **Mongoose** queries:

```javascript
animalSchema.query.byName = function (name) {
  return this.where({ name: new RegExp(name, "i") });
};
...
Animal.find().byName("fido").exec((err, animals) => {
  console.log(animals);
});
```

Query helper methods let us extend **Mongoose**'s chainable query builder **API**.

## Virtuals
`Virtual`s are document properties that we can get and set but that do not get persisted to **MongoDB**:

```javascript
personSchema.virtual("fullName").get(function () {
  return `${this.name.first} ${this.name.last}`;
});
```

The getters are useful for formatting or combining fields, while setters are useful for de-composing a single value into multiple values for storage.

## Middleware
`Middleware` (also called `pre` and `post` *hooks*) are functions which are passed control during execution of
asynchronous functions. `Middleware` is specified on the schema level and is useful for writing plugins.

```javascript
userSchema.pre("save", function (next) {
  this.updatedAt = Date.now();
  next(); // move on to the next middleware
});
```

`post` works similarly but accepts the document as the first parameter.
