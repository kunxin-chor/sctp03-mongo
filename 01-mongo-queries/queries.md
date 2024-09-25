
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