## Comparison Operators


Comparison operators are used in conditions that compare one expression with another, typically containing a boolean return value.  

The MongoDB query language is unique because it uses **comparison Operators** in two different ways:  
- Comparison Query Operators
- Comparison Aggregation Operators

Let's take a quick :eyes: at what is available for **Query Selector** operations.

**Comparison Query Operators**

|Name|Description|
|---|---|
|$eq|Matches values that are equal to a specified value.|
|$gt | Matches values that are greater than a specified value.|
|$gte | Matches values that are greater than or equal to a specified value.|
|$in | Matches any of the values specified in an array.|
|$lt | Matches values that are less than a specified value.|
|$lte | Matches values that are less than or equal to a specified value.|
|$ne | Matches all values that are not equal to a specified value.|
|$nin | Matches none of the values specified in an array.|

### Working with a single field

**Exercise 1** :computer: 

In the following query, we look at movies which had a runtime greater than 180 minutes (3 hours.)

**query:**

```javascript=
db.movieDetails.find({"runtime": {"$gt":180}}, 
    {"title": 1, "_id": 0})
```

**Exercise 2** :computer: 

What about movies which ran for greater than or equal to 2 hours but less than 3 hours?

**query:**
```javascript=
db.movieDetails.find({"runtime": {"$gte":120, "$lt": 180}}, 
    {"title": 1, "_id": 0})
```
---

### Working with multiple fields

**Exercise 3** :computer: 

List the movies which not only had a runtime greater than 3 hours but were also highly rated.

**query:**
```javascript=
db.movieDetails.find({"runtime": {"$gt": 180},
    "imdb.rating": {"$gt":8}}, 
    {"title": 1, "_id": 0})
```
---

### The "$ne" operator

`ne` stands for **not equal**. Therefore the use of this operator filters out records where the matching condition is not true.

**Exercise 3** :computer: 

Filter out records where the `rating` field does not contain the value of 'UNRATED'.   

**query:**
```javascript
db.movieDetails.find({"rated": {"$ne": "UNRATED"}}, 
    {"_id":0, "title":1, "rated":1})
```
**result:** (Only a part of the output has been shown.)
```javascript
{ "title" : "Zathura: A Space Adventure", "rated" : "PG" }
{ "title" : "Space Cowboys", "rated" : "PG-13" }
{ "title" : "Lost in Space", "rated" : "PG-13" }
{ "title" : "Muppets from Space", "rated" : "G" }
{ "title" : "Turks in Space", "rated" : null }
```

Look at the last document in the output above. The `rated` field has a `null` value. This is because `$ne` **only** excludes the documents where the `rated` field has a value of `UNRATED`. All other values, (be it `APPROVED`, `R`, `PG-13`, `PG`, `null` etc) find their way into the result. In the same vein, `$ne` also outputs documents which do **not** have a `rated` field at all. 

In a future exercise, we'll see how to find documents which do not have a given field. 

---

### The "$in" operator

The `$in` operator allows us to specify 1 or more values, any 1 of which if matched, causes a resulting document to be returned. Note that the value of `$in` should always be an array. 

**Exercise 4** :computer: 

Find movies which were either rated `R`, `PG` or `PG-13`.

**query:**
```javascript
db.movieDetails.find({"rated": {"$in": ["R", "PG", "PG-13"]}}, 
    {"_id":0, "title":1, "rated":1})
```

**Exercise 5** :computer: 

Use the `$nin` operator (**not in** operator) to match **none** of the values specified in the array. 

---

## Element Operators

Since MongoDB is a non relational database:
* there can be fields which are **present in one document** but **absent in another document**. 

* there can also be fields in a collection that have **different value types** across documents. 

In the following exercises, we'll learn about operators that help us explore these aspects of our collection. 

**Exercise 1** :computer: 

Using the `$exists` operator, count the number of documents which contain the field `tomato.consensus`.

**query:**
```javascript
db.movieDetails.count({"tomato.consensus": {"$exists": true}})
```

**Exercise 2** :computer: 

* Using the `$exists` operator, count the number of documents which do **not** contain the field `tomato.consensus`. 

    *Hint: use `{"$exists": false}`*

* Now add up the results obtained in exercises 1 and 2. Is it the same as the total collection size?

* Take 5 minutes :alarm_clock: to explore the document structure with and without these fields using the `find()` method. 

* Is the output of the following query the same as the one with `{"$exists": false}`? 

    **query:**

    ```javascript
    db.movieDetails.count({"tomato.consensus": null})
    ```
    
:arrow_right: Note that the query above returns not only documents where the `tomato.consensus` field is `null` but also those documents where the `tomato.consensus` field is absent. Check it out for yourself!

**Exercise 3** :computer: 

Explore the various datatypes present per field using the `$type` operator. Refer to the [documentation](https://docs.mongodb.com/manual/reference/operator/query/type/#op._S_type) for additional reference. 

To help you get started, here's an example:

**query:**
```javascript
db.movieDetails.find({"plot": {"$type": "string"}}).pretty()
```

**query:**
```javascript
db.movieDetails.find({"plot": {"$type": "null"}}).pretty()
```
---
