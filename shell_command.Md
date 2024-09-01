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
