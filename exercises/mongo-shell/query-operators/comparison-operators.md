## Comparison Operators

These operators allow us to match a field's value relative to another value. 

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
