
## Creating a new database
Just use the database as if it exists
```
use animal_shelter
```

### Insert new document to a collection
There's no need to create a new collection. Just insert a document into a new
collection and the collection will be created

```
db.animals.insertOne({
    "name":"Fluffy",
    "age":3,
    "breed":"Golden Retriever",
    "type":"Dog"
});
```

Example 2: Insert many documents at one go
```
db.animals.insertMany([
    {
        "name":"Dazzy",
        "age": 13,
        "breed":"Greyhound",
        "type":"Dog"
    },
    {
        "name":"Timmy",
        "age": 1,
        "breed":"Border Collie",
        "type":"Dog"
    }
]);
```

### Updating existing documents
We need to know the ID of the document, so use your own ID in the example below.
We use the `$set` to indicate which keys to change, and the new value.

```
db.animals.updateOne({
    "_id":ObjectId("66f40dcdf8203e4d1fd6d910")
}, {
    "$set":{
        "age": 1.5
    }
});
```

### Delete
```
db.animals.deleteOne({
    "_id": ObjectId("66f40d03f8203e4d1fd6d90e")
})
```

# Dealing with complex documents

```
db.animals.insertOne({
    'name':'Cookie',
    'age':3,
    'breed':'Lab Retriver',
    'type':'Dog',
    'checkups':[]
});

db.animals.insertOne({
    'name':'Frenzy',
    'age': 1,
    'breed':'Wild Cat',
    'type':'Cat',
    'checkups':[
        {
            'id': ObjectId(),
            'name':'Dr. Chua',
            'diagnosis':'Heartworms',
            'treatment':'Steriods'
        }
    ]
})
```

## Add a new checkup (i.e add a new embedded document)
```
db.animals.updateOne({
    '_id':ObjectId('66f41021f8203e4d1fd6d911')
}, {
    '$push':{
        'checkups':{
            'id': ObjectId(),
            'name':'Dr. Tan',
            'diagnosis':'Diabetes',
            'treatment':'Medication'
        }
    }
});
```

## Delete an embedded document from an array
```
db.animals.updateOne({
    "_id":ObjectId("66f41021f8203e4d1fd6d911")
}, {
    "$pull":{
        "checkups":{
            "id":ObjectId("66f411aff8203e4d1fd6d914")
        }
    }
})
```

## Update one element in an array
```
db.animals.updateOne({
    'checkups':{
        '$elemMatch':{
            'id':ObjectId('66f410e1f8203e4d1fd6d912')
        }
    }
},{
    '$set':{
        'checkups.$.name':'Dr Zhao',
        'checkups.$.date':ISODate()
    }
})
```