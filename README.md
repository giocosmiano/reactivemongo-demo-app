# MongoDB with MongoAsync : a Sample App

This is a sample app to show in a few lines of code how MongoDB can be used to build a simple (yet full featured) web application.

This example use the following:
* MongoDB (*yeah, no kidding*)
* MongoAsync, a non-blocking and asynchronous Scala driver for MongoDB
* Play 2.1 as a web framework
* Play MongoAsync module

This application manages articles. An article has a title, a text content and a publisher. The articles can be updated and sorted by title, publisher, creation/update date, etc. One or more attachments can be uploaded and bound to an article (like an image, a pdf, an archive...). All the classic CRUD operations are implemented.

To sum up, this sample covers the following features of MongoDB:
* Simple queries
* Sorting the results of a query
* Update
* Delete
* GridFS a storage engine for the attachments

The following features of MongoAsync driver are covered:
* (Non-blocking) queries, updates, deletes
* (Non-blocking) GridFS storage
* Streaming files from and into GridFS

## A Glimpse at MongoDB Features with MongoAsync

### A Word About the MongoDB's Model

> MongoDB (from "humongous") is a scalable, high-performance, open source NoSQL database.

It's a NoSQL database - in other words, MongoDB does not use SQL as a query language, and is not fully ACID-compliant. In fact, it's not even a relational database: Mongo stores data as *documents*, into buckets that are called *collections*.

A document is a set of data, in BSON (Binary JSON). This means that the manipulation of a document can be very easy, and that turning a document into plain text JSON is a piece of cake. A document that you get from MongoDB is quite ready to be used in a web context (like a web API).

As for data, queries are expressed in JSON style. It is very easy to write and read Mongo queries - you use the same format that you manipulate every day when you build web applications.

But the real power of MongoDB is that it is very scalable. It is very easy to add replicas (for replication, or for balancing eventual consistent read operations), and shards: when you get a high volume of data, you can set up a new server that will store only a part of the data. Mongo handles the query routing so that it is transparent for the developpers.

### Simple query

Queries are written in BSON (binary JSON). An empty query matches all the documents that are stored in the collection.

```scala
val collection = db("articles")
// empty query will match all documents by default
val query = Bson()
// run this query over the collection
val found = collection.find(query)
// got the list of documents (in a fully non-blocking way)
val list = collect[List, Article](found)
```

If we want to get only the documents that have a field *publisher* which value is *Stephane*, we may just write the following query:

```scala
val query = Bson("publisher" -> BSONString("Stephane"))
// run this query over the collection
val articlesPublishedByStephane = collection.find(query)
val list = collect[List, Article](articlesPublishedByStephane)
```

### Sort a query

### Update

### Delete

### GridFS