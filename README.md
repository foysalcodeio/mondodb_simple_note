# MongoDb Simple Note


Keep in mind Mongodb and mongoose are simple difference

MongoDb : MongoDB is a popular, open-source NoSQL database that uses a document-oriented data model. Data is stored in flexible, JSON-like documents rather than in fixed tables as in relational databases. Each document can have a different number of fields, content, and size.

MongoDB provides a powerful aggregation framework for data processing and transformation, which is more versatile compared to SQL-based group-by operations

Mongoose : Mongoose is an Object Data Modeling (ODM) library for MongoDB and Node.js. It provides a schema-based solution to model your application data, offering structure and organization to your database interactions.

## Ready to Backend using following steps
```
npm init -y
npm i express cors mongodb dotenv
- now create index.js
  - go to package.json > scripts > edit - "start": "node index.js"
- install dotenv using npm
   - npm install dotenv --save
```

## Add your connection string into your application code
```

const { MongoClient, ServerApiVersion } = require('mongodb');
const uri = "mongodb+srv://<USERNAME>:<PASSWORD>@cluster0.1vlp1px.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0";

// Create a MongoClient with a MongoClientOptions object to set the Stable API version
const client = new MongoClient(uri, {
  serverApi: {
    version: ServerApiVersion.v1,
    strict: true,
    deprecationErrors: true,
  }
});

async function run() {
  try {
    // Connect the client to the server	(optional starting in v4.7)
    await client.connect();


    // CODE HERE


    // Send a ping to confirm a successful connection
    await client.db("admin").command({ ping: 1 });
    console.log("Pinged your deployment. You successfully connected to MongoDB!");
  } finally {
    // Ensures that the client will close when you finish/error
    //await client.close();
  }
}
run().catch(console.dir);

//TESTING CONNECTION
app.get('/', (req, res) => {
    res.send('window port is running ...')
})

app.listen(port, ()=> {
    console.log(`console is port is running on ${port}`)
})
```


### Connect database:
 .db('carEngineer'):
client ইনস্ট্যান্সের .db('carEngineer') পদ্ধতিটি ডাটাবেস নির্বাচন করা হয় । এখানে 'carEngineer' হল সেই ডাটাবেসের নাম ব্যাবহারকারী অ্যাক্সেস করতে পারে । যদি এই নামের ডাটাবেসটি আগে থেকে বিদ্যমান না থাকে, তবে MongoDB এটি তৈরি করবে।
3. .collection('services'):
client.db('carEngineer') পদ্ধতির পরে .collection('services') পদ্ধতিটি নির্দিষ্ট ডাটাবেসের মধ্যে একটি সংগ্রহ (collection) নির্বাচন করে। এখানে 'services' হল সংগ্রহটির নাম। সংগ্রহ হল MongoDB তে ডকুমেন্টগুলির একটি গ্রুপ, যা রেকর্ড বা এন্ট্রি সংরক্ষণ করে। যদি এই নামের সংগ্রহটি আগে থেকে বিদ্যমান না থাকে, তবে MongoDB এটি তৈরি করবে।

```JS 
const serviceCollection = client.db('carEngineer').collection('services')
```

#### `Now the question is what is middle-ware?`
মিডলওয়্যার হলো সফটওয়্যারের একটি স্তর যা অপারেটিং সিস্টেম এবং অ্যাপ্লিকেশনগুলির মধ্যে বা অ্যাপ্লিকেশন এবং নেটওয়ার্কের মধ্যে অবস্থান করে এবং যোগাযোগ ও ডেটা ব্যবস্থাপনায় সহায়তা করে। এটি সাধারণত বিভিন্ন অ্যাপ্লিকেশন বা সিস্টেমের মধ্যে তথ্য এবং সেবা বিনিময় সহজতর করতে ব্যবহৃত হয়। ওয়েব ডেভেলপমেন্টে, মিডলওয়্যার সাধারণত ওয়েব অ্যাপ্লিকেশন ফ্রেমওয়ার্কে অনুরোধ এবং প্রতিক্রিয়া পরিচালনার জন্য ব্যবহৃত ফাংশন বা মডিউল বোঝায়।
Middleware is a term used in software development to refer to a layer of software that sits between the operating system and the applications running on it, or between an application and the network, facilitating communication and data management. In web development, middleware typically refers to functions or modules that handle requests and responses in a web application framework.
Example : Cors Middleware
```
app.use(cors({
  origin: ['http://localhost:5173'],
  credentials: true //that means set cookie
}))
```
মিডলওয়্যার কম্পোনেন্টগুলি সাধারণত একটি চেইনে সংগঠিত হয় যেখানে প্রতিটি কম্পোনেন্ট নির্দিষ্ট কাজগুলি পরিচালনা করে এবং তারপর চেইনের পরবর্তী কম্পোনেন্টে নিয়ন্ত্রণ হস্তান্তর করে।
Custome made Middleware function 3 section 1. req 2. res & 3. res
```
const verifyToken = async(req, res, next) => {
  // code
}
```
Here is example of verfityToken full code
```
const verifyToken = async(req, res, next) => {
  const token = req.cookies?.token;
  console.log('value of token in middleware', token)
  if(!token){
    return res.status(401).send({message: 'authorized access'})
  }
  //verify - token expire or not
  jwt.verify(token, process.env.ACCESS_TOKEN_SECRET, (err, decoded) => {
    //error
    if(err){
      console.log(err)
      return res.status(401).send({message: 'unauthorized access'})
    }
    //if token is valid then it would be decoded
    console.log('value in the token', decoded)
    req.user = decoded
    next()
  }) 
}
```
### Access Get Method
```
app.get('/services', logger, async(req, res) => {
  const cursor = serviceCollection.find()
  const result = await cursor.toArray();
  res.send(result)
 })

```
### Access Specific Get Method
```
app.get('/services/:id', async (req, res) => {
 const id = req.params.id;
 const query = {_id: new ObjectId(id)}

 const options = {
 projection: {title: 1, price: 1, service_id: 1, img: 1},
};

 const result = await serviceCollection.findOne(query, options);
 res.send(result);
})
```
### Access Post Method
```
app.post('/bookings', async(req, res) => {
    const booking = req.body;
    console.log(booking)
    const result = await bookingCollection.insertOne(booking);
    res.send(result)
})
```

### Access Delete Method
```
app.delete('/bookings/:id', async (req, res) => {
    const id = req.params.id;
    const query = { _id: new ObjectId(id) }
    const result = await bookingCollection.deleteOne(query);
    res.send(result);
})
```

### Access Update Method
```
  app.patch('/bookings/:id', async (req, res) => {
    const id = req.params.id;
    const filter = { _id: new ObjectId(id) }
    const updateBooking = req.body;
    console.log(updateBooking)

    const updateDoc = {
      $set: {
        status: updateBooking.status
      }
    }
    const result = await bookingCollection.updateOne(filter, updateDoc);
    res.send(result)

  })
```

