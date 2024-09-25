
# Queries

## Fetch all documents from a collection
Get all the documents from the `listingsAndReviews`:

```
db.listingsAndReviews.find()
```

In general the syntax is:
```
db.<collection name>.find()
```

## Find by a criteria

Find all listings with 2 beds:
```
db.listinngsAndReviews.find({
	"beds": 2
});

```

## Projection
Allows us to limit the fields (or key/value pairs) sent by Mongo

Example 1: Show all documents but only their name, address and number of beds
```
db.listingsAndReviews.find({}, {
    'name': 1,
    'address': 1,
    'beds': 1
})
```

Example 2: See all listings with exactly 2 beds and only show their name and number of beds
```
db.listingsAndReviews.find({
    'beds': 2
}, {
    'name': 1,
    'beds': 1,
})
```

Example 3: Find all documents where the proprerty_type is "Apartment" and have only 2 beds
and only show their name, property type and beds.

```
db.listingsAndReviews.find({
    "property_type":"Apartment",
    "beds": 2
}, {
    "name":1,
    "property_type":1,
    "beds": 1
})
```

Example: Find all the listings where the maximum_nights is 3 and cancellation_policy is "flexible", and only show the address, the name, maximum_nights and cancellation_policy

```
db.listingsAndReviews.find({
    "maximum_nights": "3",
    "cancellation_policy":"flexible",
}, {
    "name":1,
    "address":1,
    "maximum_nights": 1,
    "cancellation_policy": 1
});
```

## Find by key in a nested object

Example: find all listings in Brazil
```
db.listingsAndReviews.find({
    'address.country':'Brazil'
}, {
    'name':1,
    'address.country':1
});
```

## Find by range

Example 1: Find all the listings that have  3 or more beds
```
db.listingsAndReviews.find({
    'beds': {
        "$gte": 3
    }
}, {
    'name':1,
    'beds': 1
})
```

Exmaple 2: Find all listings that have between 3 to 5 beds
```
db.listingsAndReviews.find({
    'beds': {
        '$gte':3,
        '$lte':5
    }
}, {
    'name':1,
    'beds':1
});
```

## Find by an element in an array

Example 1: Find all the listings that the element "Oven" in the amenities array
```
db.listingsAndReviews.find({
    'amenities':'Oven'
}, {
    'name': 1,
    'amenities': 1
});
```

Example 2: Only show the amenities that we are looking for

```
db.listingsAndReviews.find({
    'amenities':'Oven'
}, {
    'name': 1,
    'amenities.$': 1
});
```

Example 3: Only show listings that have BOTH "Oven" and "BBQ Grill" in the amenities array.
```
db.listingsAndReviews.find({
    'amenities':{
        '$all':["Oven", "BBQ grill"]
    }
}, {
    'name':1,
    'amenities':1
});
```
Example 4: Only show listings which has either 'TV' or 'Cable TV' in the amenities array
```
db.listngsAndReviews.find({
    'amenities': {
        '$in':['TV', 'Cable TV']
    }
},{
    'name':1,
    'amenities':1
});
```

## find by ObjectId
This example is supposed to be for `sample_mflix` database. Usually reserved finding a
specific document by its _id.

```
use sample_mflix;
db.movies.find({
    "_id": ObjectId("573a1391f29313caabcd71e3")
})
```

## Find all elements of arrays across different documents by a critera
Find all reviews written with the `reviewer_name` of `Alex` (usually reserved
for searching through arrays of objects):

```
db.listingsAndReviews.find({
    'reviews':{
        '$elemMatch':{
            'reviewer_name':'Alex'
        }
    }
},{
    'name':1,
    'reviews.$':1
})
```

## Find by dates

Example 1: Find all the listings which last reviews are before 30th Dec 2018 

Note: the ISO date format is YYYY-MM-DD
```
db.listingsAndReviews.find({
    'last_review':{
        '$lt': ISODate('2018-12-30')
    }
},{
    'name':1,
    'last_review':1
})

```

## Find by string patterns
Example: Find every listing that has the word 'spacious' (regardless of casing) in the name
```
db.listingsAndReviews.find({
    'name': {
        '$regex':'spacious', '$options':'i'
    }
}, {
    'name': 1
})
```

```
db.listingsAndReviews.find({}
   ,{
        arrayFilters:[
        
            {
                "reviews.date": {
                    "$lte": ISODate("2018-12-30")
                }
            }
        
        ]
    }
)
```