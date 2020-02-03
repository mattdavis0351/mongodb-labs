# Intermediate Mongo Queries

There are numerous operators that we can use in our queries to answer more and more complex questions related to our collection. Some of these operators help with learning more about the data stored in the collection whereas others help with understanding the metadata. This document will take you through those operators, which are categorized as follows:

* Comparison Operators
* Logical Operators
* Element Operators
* Array Operators

:book: [Read more](https://docs.mongodb.com/manual/reference/operator/query/) about **query selectors**. 

:arrow_right: Switch over to the `movieDetails` collection for the following exercises. 

## Comparison Operators


Comparison operators are used in conditions that compare one expression with another, typically containing a boolean return value.  

The MongoDB query language is unique because it uses **Comparison Operators** in two different ways:  
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

In the following query, we look at movies which were made in the year 1970 or before.

**query:**

```javascript
db.movieDetails.find({year: {"$lte": 1970}}, 
    {title: 1, _id: 0})
```

**Exercise 2** :computer: 

What about movies which were made in or before 1970 but after 1965?

**query:**
```javascript
db.movieDetails.find({year: {"$lte": 1970, "$gt": 1965}}, 
    {title: 1, _id: 0})
```
---

### Working with multiple fields

**Exercise 3** :computer: 

List the movies which had a runtime longer than 3 hours and had a `imdb.rating` higher than 8.

**query:**
```javascript
db.movieDetails.find({"runtime": {"$gt": 180},
    "imdb.rating": {"$gt":8}}, 
    {"title": 1, "_id": 0})
```
---

### The "$ne" query operator

`ne` stands for **not equal**. Therefore the use of this operator filters out records where the matching condition is not true.

**Exercise 3** :computer: 

Filter out records where the `rating` field does not contain the value of 'NOT RATED'.   

**query:**
```javascript
db.movieDetails.find({"rated": {"$ne": "NOT RATED"}}, 
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

Look at the last document in the output above. The `rated` field has a `null` value. This is because `$ne` **only** excludes the documents where the `rated` field has a value of `NOT RATED`. All other values, (be it `APPROVED`, `R`, `PG-13`, `PG`, `null` etc) find their way into the result. In the same vein, `$ne` also outputs documents which do **not** have a `rated` field at all. 

In a future exercise, we'll see how to find documents which do not have a given field. 

---

### The "$in" query operator

The `$in` operator allows us to specify 1 or more values in an array. If any 1 of those filter conditions is matched, a resulting document is returned. 

**Exercise 4** :computer: 

Find movies which were tagged as one of the following genres: `Sport`, `Talk-Show` or `News`. Pretty unusual genres for a movie!

**query:**
```javascript
db.movieDetails.find({"genres": {"$in": ["Sport", "Talk-Show", "News"]}}, 
    {"_id":0, "title":1, "genres":1})
```

**Exercise 5** :computer: 

Use the `$nin` query operator (**not in** operator) to match **none** of the values specified in the array. 

---

## Logical Operators

These operators perform one of the following logical operations on the fields:

Name | Description
--- | ---
$and | Joins query clauses with a logical AND returns all documents that match the conditions of both clauses.
$or | Joins query clauses with a logical OR returns all documents that match the conditions of either clause.
$not | Inverts the effect of a query expression and returns documents that do not match the query expression.
$nor | Joins query clauses with a logical NOR returns all documents that fail to match both clauses.


### The "$or" operator
This operator **OR**s the filters stated in a query and returns documents where atleast 1 of the filters is true. 

**Exercise 1** :computer: 

Write a query to list movies whose `imdb.rating` is greater than 8.5 or `metacritic` rating is greater than 85.

:arrow_right: Note that the `$or` operator takes an **array** of values.

Syntax Example:
`{"$or":[{field:condition}, {field:condition}]}`

**query:**
```javascript
db.movieDetails.find({"$or": [
    {"imdb.rating": {"$gt": 8.5}}, 
    {"metacritic": {"$gt": 85}}]}, 
        {"_id": 0, "title": 1, "imdb.rating": 1, "metacritic": 1})
```
---

### The "$and" operator

As you [already know](../exercises/01_basic-mongo-queries.md#Filtering-on-multiple-fields), MongoDB implicitly **AND**s the filters when separated by `,` in the query. 

**Exercise 2** :computer: 

To cover this next point we need to generate some output first.  Run the query below to get the output we need. 

:bulb: Can you explain the different operators and parameters in this query without seeing the output?
```javascript
db.movieDetails.find({
    "tomato.meter": {"$lt": 50}, 
    "tomato.meter": {"$ne": null}}, 
    {"_id": 0, "title": 1, "tomato.meter": 1})
```
**result:** (Only a part of the result has been shown.)
```javascript
{ "title" : "Once Upon a Time in the West", "tomato" : { "meter" : 98 } }
{ "title" : "A Million Ways to Die in the West", "tomato" : { "meter" : 33 } }
{ "title" : "Wild Wild West", "tomato" : { "meter" : 17 } }
{ "title" : "Slow West", "tomato" : { "meter" : 92 } }
{ "title" : "Journey to the West", "tomato" : { "meter" : 93 } }
```

Ideally, the 2 filter conditions would be **AND**ed. But :eyes: closely at the results and you'll see that the first filter condition `tomato.meter": {"$lt": 50}` was ignored. The result contains documents which have `tomato.meter` ratings greater than 50 as well. Why?

:arrow_right: MongoDB requires **unique** key values to be provided in its queries. In cases like the one above, where both key values are identical (`tomato.meter` in both filters), the very last filter overwrites all the previous filters and documents that satisfy the last filter are returned. 

This is where `$and` finds its use case. As you might've guessed, it is used to AND filter conditions where the key values are **not unique**. 

Take 5 minutes :alarm_clock: to run the above query using the `$and` operator. Keep in mind that the `$and` takes an **array** of values just like `$or`.

---

## Element Operators

Since MongoDB is a non relational database:
* there can be fields which are **present in one document** but **absent in another document**. 

* there can also be fields in a collection that have **different data types** across documents. 

Following are the operators that help us explore these aspects of our collection:

Name | Description
--- | ---
$exists | Matches documents that have the specified field.
$type | Selects documents if a field is of the specified type.

**Exercise 1** :computer: 

Using the `$exists` operator, count the number of documents which contain the field `poster`.

**query:**
```javascript
db.movieDetails.count({"poster": {"$exists": true}})
```

**Exercise 2** :computer: 

* Using the `$exists` operator, count the number of documents which do **not** contain the field `poster`. 

    *Hint: use `{"$exists": false}`*

* Now add up the results obtained in exercises 1 and 2. Is it the same as the total collection size?

* Take 5 minutes :alarm_clock: to explore the document structure with and without these fields using the `find()` method. 

* Is the output of the following query the same as the one with `{"$exists": false}`? 

    **query:**

    ```javascript
    db.movieDetails.count({"poster": null})
    ```
    
:arrow_right: Note that the query above returns not only documents where the `poster` field is `null` but also those documents where the `poster` field is absent. Check it out for yourself!

**Exercise 3** :computer: 

Explore the various datatypes present per field using the `$type` operator. Refer to the [documentation](https://docs.mongodb.com/manual/reference/operator/query/type/#op._S_type) for additional reference. You already know by now that `null` is considered a datatype in MongoDB.

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

## Array Operators

In the following exercises, we'll look at operators for array fields. 

Name | Description
--- | ---
$all | Matches arrays that contain all elements specified in the query.
$elemMatch | Selects documents if element in the array field matches all the specified $elemMatch conditions.
$size | Selects documents if the array field is a specified size.

### The "$all" query operator

`$all` matches fields against an array of elements. A document is returned when all the elements listed in the query are found in the document's array field. 

:arrow_right: Elements in the document's array are **not** required to be in the exact order as specified in the query. 

**Exercise 1** :computer: 

Use the `$all` operator to find movies which were both:  History and War `genres`. 

**query:**
```javascript
db.movieDetails.find({"genres": {"$all": ['History', 'War']}}, 
    {"_id": 0, "title": 1, "genres": 1})
```

:arrow_right: As you can see, returned documents contain both History and War `genres` in addition to others (like Documentary, Short etc) which were not specified in our query. Also note that the order of elements did not matter. 

---

### The "$elemMatch" query operator

One of the more powerful operators for arrays is `$elemMatch`. It matches array elements which are present in the same position and is best explained through an example. 

Copy and run the following code in your mongo shell: (Revenue is in million dollars.)
```javascript
var1 = db.movieDetails.findOne({"title": "Shutter Island"})
delete var1._id
var1.boxOffice = [{"country": "America", "revenue": 127.3},{"country": "Overseas", "revenue": 166.5}]
db.movieDetails.insertOne(var1)
```

What did we just do?!
Simply put:
* we created a variable called `var1`
* added a reference to the document titled `"Shutter Island"` within that variable
* went on to add another field called `boxOffice` to that variable which turns out to be an **array of objects**
* inserted that variable in our collection as a document of its own

Now when you run:
```javascript
db.movieDetails.find({"title": "Shutter Island"}).pretty()
```
you'll see that 2 documents are returned. One's the original document and the other is the one we just created with an additional `boxOffice` field. 

The point of this was to add an **array of objects** field to our collection. Now onto the exercise where we'll see how to leverage `$elemMatch` to query this field. 

**Exercise 2** :computer: 

Take 5 minutes :alarm_clock: to find the movies where the `boxOffice` revenue in "America" was greater than 
* $100m 
* $150m

How many of you ran similar queries?
```javascript
db.movieDetails.find({"boxOffice.country": "America", 
    "boxOffice.revenue": {"$gt": 100}}).pretty()
    
db.movieDetails.find({"boxOffice.country": "America", 
    "boxOffice.revenue": {"$gt": 150}}).pretty()
```

:arrow_right: Note that the first query runs correctly by returning the document with "Shutter Island" `boxOffice`, but the second query returns the same result even though it is :x:!!! 
There is no movie in our collection with revenue greater than $150m in America. 

This happened because the query ran the following checks: 
* is `boxOffice.country` equal to "America" :white_check_mark:
* is `boxOffice.revenue` greater than $150m :white_check_mark:

Both checks were satisfied at **different array positions**!!!

Here's where `$elemMatch` comes in. It ensures that array elements present **in the same position** are evalauted. 

Try the following 2 queries: 
```javascript
db.movieDetails.find({"boxOffice": 
    {"$elemMatch": 
        {"country": "America", "revenue": 
            {"$gt": 100}}}}).pretty()
            
db.movieDetails.find({"boxOffice": 
    {"$elemMatch": 
        {"country": "America", "revenue": 
            {"$gt": 150}}}}).pretty()
```

See the difference?! :eyes:

---

### The "$size" query operator

As the name suggests, this operator deals with the length of an array. 

**Exercise 3** :computer: 

* What were the maximum number of countries a movie was shot in? And which were those movies?

    **query:**

    ```javascript
    db.movieDetails.find({"countries": {"$size": 6}}, 
        {"_id": 0, "title": 1, "countries": 1})
    ``` 

Would you have liked to be a part of that movie and traveled to all those countries? :earth_africa:

---
## Group Activities
During this time the class should split into :five: groups to complete each section.

**Activity 1: :alarm_clock: 15 minutes** 

Let's figure out some weekend plans! Get together with the rest of your group and look at movies from a genre(s) you all like. Pick a couple of highly rated movies that you haven't watched yet and let the rest of the class know that these will be on your list! Also, are there some medium or low rated movies from that genre(s) that you've already watched? Do you agree with the rating? Why?

**Activity 2: :alarm_clock: 15 minutes**

Explore the different datasets available on your workstation.  Be creative and try to find interesting things.  
Some questions to consider while exploring:
- How is this data structured?
- Is there anything interesting to me about this data?
- Is the data malformed, incorrect or incomplete?
- What are the challenges you faced while exploring this data?

**Presentation: :alarm_clock: 25 minutes**
Present your results from **Group Activities** and answer any questions your classmates may have.
