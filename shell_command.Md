First of All we try to know what is basic difference sql and mongodb datbase


    SQL        |        MONGODB
  - sql------------------nosql
  - relation-------------no-relation
  - table----------------collection
  - raw------------------document
  - column---------------field




connect
```
mongosh "mongodb+srv://caruser:Nm6CijZZ2ZlwBA1F@cluster0.1vlp1px.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0"
```
create database
```
db.createCollection("student")
```

Show Databases
```
show dbs
db // prints the current database
```

Switch Database
```
use db
```

show database collection / show table , In database which file are here
```
show collections
```

delete collection
```
db.[collection_name].drop()
```

CRUD

create
```
db.collection.insert_one(obj)
db.coll.insertOne({name: "Navi", age: 25}) // insert one document
db.collection.insert_many(arr)
db.coll.insertMany([{name: "Navi", age: 25}, {name: "Alice", age: 30}]) // insert many documents
```

read
```
db.coll.count() // count all documents in 'coll' collection
db.coll.count({name: "Navi"}) // count all documents with name "Navi"

db.coll.find() // list all documents
db.coll.find({name: "Navi"}) // list all documents with name "Navi"
db.coll.find({name: "Navi", age: 25}).limit(1) // list all documents with name "Navi" and age 25, and return only one document.

db.coll.find({name: "Navi"}).explain("executionStats") // find document and show execution stats
```

update
```
You can update all match entries with:
db.collection.update({key : 'matchcriteria'}, {key : 'newvalue'}) -> 1st key > match, 2nd key > Replace

db.coll.update({name: "Navi"}, {$set: {age: 26}}) // update all documents with name "Navi" and set age to 26
db.coll.update({name: "Navi"}, {$inc: {age: 1}}) // update all documents with name "Navi" and increment age by 1

db.coll.update({name: "Navi"}, {$unset: {age: 1}}) // update all documents with name "Navi" and set age to null
db.coll.updateMany({age: {$exists: true}}, {$unset: {age: ""}}) // remove age field from all documents with age field
```

delete
```
db.coll.deleteMany({name: "Navi"}) // remove all documents with name "Navi"
db.coll.deleteOne({name: "Navi"}) // remove one document with name "Navi"
```

Index
```
db.coll.getIndexes()   // list indexes
db.coll.getIndexKeys() // list index keys

db.coll.createIndex({"name": 1}) // create index
db.coll.createIndex({"name": 1, "date": 1})     // create a compound index

db.coll.dropIndex("name_1") // drop index

```
