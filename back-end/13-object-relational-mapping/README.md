![cf](http://i.imgur.com/7v5ASc8.png) 13: MongoDB & Object Relational Mapping
===

## Learning Objectives
* Students will be able to persist data using MongoDB
* Students will learn about Object Relation Mapping (ORM)
* Students will learn about Object Document Mapping (ODM)
* Students will learn to model data using a MongoDB ODM

## Resources
* Read [MongoDB Driver QuickStart](http://mongodb.github.io/node-mongodb-native/2.2/quick-start/quick-start/)
* Skim [MongoDB Driver Collection API Docs](http://mongodb.github.io/node-mongodb-native/2.2/api/Collection.html)
* Read [mongoose guide](http://mongoosejs.com/docs/guide.html)


## MongoDB
MongoDB is a free and open-source document oriented database. This is referred to as a NoSQL database management system. It allows developers to create schemas and store and query JSON like documents. MongoDB has a powerful query system that supports field queries, range queries, and regular expression searches. Documents can be indexed with primary and secondary keys. MongoDB databases can scale horizontally using a technique called sharding, that enables many MongoDB databases to act as one through the use of a load balancer. MongoDB has an aggregation framework that enables developers to process data and return computed results. MongoDB has grown in popularity among teams that strive for agile and rapid development of their products. It is a great database for storing key value pairs.

Although MongoDB has many great features for build modern web application, it is not good at solving the following problems:

 - MongoDB does not support joins, and therefor doesn't model relational data well
 - It is not good for storing binary data (like images)
 - It is not small enough to run as an embedded DB (like on a phone)
 - It is also not good a doing large MapReduce jobs (huge aggregations)

 If your application is depending on doing many of these operations, you may want to consider using other database technologies.

MongoDB, like many databases, is built on the client-server model. A MongoDB Server called a Mongo [Daemon](https://en.wikipedia.org/wiki/Daemon_(computing)) is run as a background task on a host. A Client such as a MongoDB Shell or MongoDB Driver can connect to the MongoDB Dameon to run queries. MongoDB Drivers are libraries that enable developers to make queries from a programing language.


## ORMs and ODMs
Object Relational Mapping (ORM) is a programming technique for converting relational database models to a programming language data-type using Object Oriented Programing. ORM libraries enable developers to use their programming language to control a database instead of using the database's native query language. They can help developers write cleaner and safer code, by abstracting away the complexities of database languages. Object Document Mapping (ODM) libraries function similar to ORM libraries but work with NoSQL databases that use document database models.

## Mongoose Example
```js
var mongoose = require('mongoose');
mongoose.Promise = global.Promise;
mongoose.connect('mongodb://localhost/test', { useMongoClient: true })
  .then(createCat)
  .catch(err => console.log(err.message));

function createCat() {
  // Create something to model Cats
  var Cat = mongoose.model('Cat', { name: String, sound: String });

  // Create a cat, save it to the database and read it.
  var kitty = new Cat({ name: 'Zildjian', sound: 'meow'});
  kitty.save(function (err, savedCat) {
    if (err) {
      console.log(err);
    } else {
      console.log(savedCat.name, 'says', savedCat.sound);
    }

    mongoose.disconnect();
  });
}
```

## Common Mongoose Methods
Mongoose provides these useful CRUD methods

* `Cat.save()`
* `Cat.findById(id)`
* `Cat.find()`
* `Cat.findByIdAndRemove(id)`
* `Cat.findByIdAndUpdate(id, modelParams)`
  * `modelParams = {name: 'Happy Zildjian', sound: 'purr'})`
  * Lots of [docs](http://mongoosejs.com/docs/api.html#model_Model.findByIdAndUpdate)
    to read on this one!

## Mongoose Query Methods
Mongoose provides a way to query documents too.

* [Mongoose Queries](http://mongoosejs.com/docs/queries.html)

Here's a simple query:

```js
Tshirts.find({ size: 'small' }).where('createdDate').gt(oneYearAgo).exec(callback);
```

Here's two complex examples to give you an idea of what else it allows you to do:

```js
Person.
  find({
    occupation: /host/,
    'name.last': 'Ghost',
    age: { $gt: 17, $lt: 66 },
    likes: { $in: ['vaporizing', 'talking'] }
  }).
  limit(10).
  sort({ occupation: -1 }).
  select({ name: 1, occupation: 1 }).
  exec(callback);

// Using query builder
Person.
  find({ occupation: /host/ }).
  where('name.last').equals('Ghost').
  where('age').gt(17).lt(66).
  where('likes').in(['vaporizing', 'talking']).
  limit(10).
  sort('-occupation').
  select('name occupation').
  exec(callback);
```

## Don't Like Mongo Yet?
Find an interesting JSON data set and see how easily you can import it into
MongoDB.

This is an example of import "Now That's What I Call Music!" data from a JSON
file. (The file is available in the demos folder).

```
mongoimport --jsonArray --db nowmusic --collection tracks now-thats-what-i-call-music.json
```
