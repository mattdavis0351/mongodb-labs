## READ Operations

### Counting records
**Exercise 1** :computer:

A simple `count()` operation in order to count the number of documents in the collection.

**query:**
```javascript
db.movieDetails.count()
```

:arrow_right: `count()` returns the total of all documents, which means duplicate documents will be counted.  
To get a count of unique values replace `count()` with `distinct()`.


---

### Filtering on a single field

**Exercise 2** :computer: 

In order to filter data, add a parameter as the first argument to the `count()` method. Parameters exist as objects, and are therefore `key:value` pairs. 

In the query below, `rated` is the **key** and `PG-13` is the **value**.

**query:**
```javascript
db.movieDetails.count({"rated": "PG-13"})
```
---

### Filtering on multiple fields

**Exercise 3** :computer:

Adding another parameter using the `,` operator **AND**s the filters and returns the matching results.

 

**query:**

```javascript
db.movieDetails.count({"rated": "PG-13", "year":1993})
```
As you can see the documents returned are only those where BOTH parameters match.

:book: [Read More](https://docs.mongodb.com/manual/reference/command/count/index.html) about the `count()` method

---

### Using the "find()" method

**Exercise 4** :computer:

To display an entire document, use the `find()` method.

:arrow_right: When using `find()` with no additional parameters, every document in the database will be returned. 

Usually you will want to add parameters to `find()` like you see in the example below.  Remember that `,` is an **AND** operator.

**query:**
```javascript
db.movieDetails.find({"rated": "PG-13", "year":1993})
```

**result:** (Only a part of the output has been shown.)
```javascript
{ "_id" : ObjectId("5d7e92671ddae9be2e725550"), "title" : "Son in Law", "year" : 1993, "rated" : "PG-13", "runtime" : 95, "countries" : [ "USA" ], "genres" : [ "Comedy", "Drama", "Romance" ], "director" : "Steve Rash", "writers" : [ "Patrick J. Clifton", "Susan McMartin", "Peter M. Lenkov", "Fax Bahr", "Adam Small", "Shawn Schepps" ], "actors" : [ "Pauly Shore", "Carla Gugino", "Lane Smith", "Cindy Pickett" ], "plot" : "Having gotten a taste of college life, a drastically changed farm girl returns home for Thanksgiving break with her best friend, a flamboyant party animal who is clearly a fish out of water in a small farm town.", "poster" : "http://ia.media-imdb.com/images/M/MV5BMTUxNDkyODMwN15BMl5BanBnXkFtZTYwODA3NjU5._V1_SX300.jpg", "imdb" : { "id" : "tt0108186", "rating" : 5.6, "votes" : 12557 }, "awards" : { "wins" : 0, "nominations" : 1, "text" : "1 nomination." }, "type" : "movie" }
```

As you can see, the output contains the entire document in a single line. This makes it hard to discern the layout of the document. We'll cover easy document parsing in the next exercise.

**Exercise 5** :computer: 

Like JavaScript, the MongoDB query language can chain methods together to refine the output of your queries.  Chaining happens by placing a `.` between method names.

Let's chain the `pretty()` method to the `find()` method for a prettier print of the output where each new field is written on a new line.

**query:**
```javascript
db.movieDetails.find({"rated": "PG-13", "year":1993}).pretty()
```

**result:** (Only a part of the output has been shown.)
```javascript
{
	"_id" : ObjectId("5d7e92671ddae9be2e725550"),
	"title" : "Son in Law",
	"year" : 1993,
	"rated" : "PG-13",
	"runtime" : 95,
	"countries" : [
		"USA"
	],
	"genres" : [
		"Comedy",
		"Drama",
		"Romance"
	],
	"director" : "Steve Rash",
	"writers" : [
		"Patrick J. Clifton",
		"Susan McMartin",
		"Peter M. Lenkov",
		"Fax Bahr",
		"Adam Small",
		"Shawn Schepps"
	],
	"actors" : [
		"Pauly Shore",
		"Carla Gugino",
		"Lane Smith",
		"Cindy Pickett"
	],
	"plot" : "Having gotten a taste of college life, a drastically changed farm girl returns home for Thanksgiving break with her best friend, a flamboyant party animal who is clearly a fish out of water in a small farm town.",
	"poster" : "http://ia.media-imdb.com/images/M/MV5BMTUxNDkyODMwN15BMl5BanBnXkFtZTYwODA3NjU5._V1_SX300.jpg",
	"imdb" : {
		"id" : "tt0108186",
		"rating" : 5.6,
		"votes" : 12557
	},
	"awards" : {
		"wins" : 0,
		"nominations" : 1,
		"text" : "1 nomination."
	},
	"type" : "movie"
}
```
Much nicer on our :eyes:!

---

### Filtering embedded documents

**Exercise 6** :computer:

Driving down the hierarchy in documents can be done using the dot notation. In the query below, we access the `rating` field (which is nested inside the `imdb` field) using `imdb.rating` as the **key**.

**query:**

```javascript
db.movieDetails.find({"imdb.rating":9.5}).pretty()
```
:arrow_right: It is important to note that if the **value** of the **key** is an array, you will need to use the aggregate pipeline instead of a simple `find()` method.

---

### Filtering array values

**Exercise 7** :computer:

When it comes to matching on array values, we can match in 3 ways:
* the entire array

The following query matches documents where the `genres` array contains **exactly** 'Documentary' followed by 'Family', in that order. Note that arrays that contain 'Documentary', 'Family' and any other genre like 'Drama' or 'Comedy' are not returned because they are not an **exact** match of the filter criteria. 



 **query:**

```javascript
db.movieDetails.find({"genres":["Documentary", "Family"]}).pretty()
```

**result:**
```javascript
{
	"_id" : ObjectId("5d7e92671ddae9be2e7259f4"),
	"title" : "Coming Off the DL",
	"year" : 2010,
	"rated" : null,
	"runtime" : 26,
	"countries" : [
		"USA"
	],
	"genres" : [
		"Documentary",
		"Family"
	],
	"director" : "Trish Campbell, Dan Hunt",
	"writers" : [
		"Justin Ferguson",
		"Megan Hansler"
	],
	"actors" : [
		"Nick Gaynor",
		"Frank Kineavy"
	],
	"plot" : "When athletes are 'on the DL,' or disabled list, they cannot play due to injury. In the film 'Coming Off The DL,' the meaning of 'disabled list' changes, but the feeling of exclusion ...",
	"poster" : null,
	"imdb" : {
		"id" : "tt1723754",
		"rating" : 9,
		"votes" : 11
	},
	"awards" : {
		"wins" : 0,
		"nominations" : 0,
		"text" : ""
	},
	"type" : "movie"
}
{
	"_id" : ObjectId("5d7e92671ddae9be2e725d78"),
	"title" : "Small Town, USA: Andrews, NC",
	"year" : 2014,
	"rated" : null,
	"runtime" : 51,
	"countries" : [
		"USA"
	],
	"genres" : [
		"Documentary",
		"Family"
	],
	"director" : "Johnathon Proctor",
	"writers" : [
		"Johnathon Proctor"
	],
	"actors" : [
		"Ann Lukens"
	],
	"plot" : null,
	"poster" : null,
	"imdb" : {
		"id" : "tt3588916",
		"rating" : null,
		"votes" : null
	},
	"awards" : {
		"wins" : 0,
		"nominations" : 0,
		"text" : ""
	},
	"type" : "movie"
}
```
:arrow_right: Documents with the array of `genres:["Family","Documentary"]` will also not be return for the same reason.

**Exercise 8** :computer: 
* any element in the array

A more common search criteria is filtering out a single element in the array, irrespective of its array position. Note that 'Musical' is matched irrespective of its position in the `genres` array. 

**query:**
```javascript
db.movieDetails.find({"genres":"Musical"}).pretty()
```
**result:** (Only a part of the output has been shown.)
```javascript
{
	"_id" : ObjectId("5d7e92671ddae9be2e72570a"),
	"title" : "Rok Sako To Rok Lo",
	"year" : 2004,
	"rated" : null,
	"runtime" : null,
	"countries" : [
		"India"
	],
	"genres" : [
		"Adventure",
		"Musical",
		"Romance"
	],
	"director" : "Arindam Chowdhuri",
	"writers" : [
		"Arindam Chowdhuri"
	],
	"actors" : [
		"Sunny Deol",
		"Yash Pandit",
		"Manjari Phadnis",
		"Carran Kapur"
	],
	"plot" : "In a small scenic town in India there are two schools, Valley High School, which basically caters to the affluent, and Bharti High School for the middle-class. The annual sports fest has ...",
	"poster" : null,
	"imdb" : {
		"id" : "tt0423087",
		"rating" : 6.7,
		"votes" : 1784
	},
	"awards" : {
		"wins" : 0,
		"nominations" : 1,
		"text" : "1 nomination."
	},
	"type" : "movie"
}
{
	"_id" : ObjectId("5d7e92671ddae9be2e725740"),
	"title" : "Har Dil Jo Pyar Karega...",
	"year" : 2000,
	"rated" : null,
	"runtime" : 173,
	"countries" : [
		"India"
	],
	"genres" : [
		"Comedy",
		"Drama",
		"Musical"
	],
	"director" : "Raj Kanwar",
	"writers" : [
		"Rumi Jaffery",
		"Rumi Jaffery"
	],
	"actors" : [
		"Salman Khan",
		"Preity Zinta",
		"Rani Mukerji",
		"Shah Rukh Khan"
	],
	"plot" : "Raj is a struggling singer with big dreams who is still waiting for his big break. One night he witnesses an accident where a car spins out of control and lands on the tracks of an ...",
	"poster" : "http://ia.media-imdb.com/images/M/MV5BMjY1NzQ4NzM2OF5BMl5BanBnXkFtZTcwMzY1ODgzMQ@@._V1_SX300.jpg",
	"imdb" : {
		"id" : "tt0250415",
		"rating" : 5.1,
		"votes" : 2835
	},
	"awards" : {
		"wins" : 1,
		"nominations" : 3,
		"text" : "1 win & 3 nominations."
	},
	"type" : "movie"
}
{
	"_id" : ObjectId("5d7e92671ddae9be2e725cb1"),
	"title" : "Ez Már Nemcsak Játék",
	"year" : 1982,
	"rated" : null,
	"runtime" : null,
	"countries" : [
		"Hungary"
	],
	"genres" : [
		"Musical"
	],
	"director" : "Judit Szakall",
	"writers" : [
		"Andras Lantos aka. Andy Lant",
		"Andy Lant",
		"Judit Szakall"
	],
	"actors" : [
		"Zsuzsa Gaspar",
		"Andy Lant",
		"János Szani",
		"Marianna Tál"
	],
	"plot" : "A high school musical about kids getting accepted to High School in Hungary and their funny, or at times quite rough circumstances of growing up.",
	"poster" : null,
	"imdb" : {
		"id" : "tt1956486",
		"rating" : 7.8,
		"votes" : 6
	},
	"awards" : {
		"wins" : 0,
		"nominations" : 0,
		"text" : ""
	},
	"type" : "movie"
}
```
:arrow_right: This method only works if the **value** is not an object.  We will cover how to access objects in an array in a later exercise.

**Exercise 9** :computer: 

* array position (for eg: arrays whose first element match a particular criteria)

Sometimes array positions matter as in the case of the `actors` array where names are listed in the order of contribution. In order to achieve this, use dot notation to specify an array index.

**query:**
```javascript
db.movieDetails.find({"actors.0":"Shah Rukh Khan"}).pretty()
```

**result:** (Only a part of the output has been shown.)
```javascript
{
	"_id" : ObjectId("5d7e92671ddae9be2e725a15"),
	"title" : "Main Hoon Na",
	"year" : 2004,
	"rated" : null,
	"runtime" : 179,
	"countries" : [
		"India"
	],
	"genres" : [
		"Action",
		"Comedy",
		"Romance"
	],
	"director" : "Farah Khan",
	"writers" : [
		"Farah Khan",
		"Abbas Tyrewala",
		"Farah Khan",
		"Rajesh Saathi",
		"Abbas Tyrewala"
	],
	"actors" : [
		"Shah Rukh Khan",
		"Sushmita Sen",
		"Sunil Shetty",
		"Zayed Khan"
	],
	"plot" : "An army major goes undercover as a college student. His mission is both professional and personal: to protect his general's daughter from a radical militant, and to find his estranged half-brother.",
	"poster" : "http://ia.media-imdb.com/images/M/MV5BMjE1Mjg3NTY3NV5BMl5BanBnXkFtZTcwNjQyNDE0MQ@@._V1_SX300.jpg",
	"imdb" : {
		"id" : "tt0347473",
		"rating" : 6.9,
		"votes" : 20277
	},
	"awards" : {
		"wins" : 8,
		"nominations" : 34,
		"text" : "8 wins & 34 nominations."
	},
	"type" : "movie"
}
{
	"_id" : ObjectId("5d7e92671ddae9be2e725a8d"),
	"title" : "Dilwale Dulhania Le Jayenge",
	"year" : 1995,
	"rated" : "NOT RATED",
	"runtime" : 181,
	"countries" : [
		"India"
	],
	"genres" : [
		"Comedy",
		"Drama",
		"Musical"
	],
	"director" : "Aditya Chopra",
	"writers" : [
		"Aditya Chopra",
		"Aditya Chopra",
		"Aditya Chopra",
		"Javed Siddiqui"
	],
	"actors" : [
		"Shah Rukh Khan",
		"Kajol",
		"Amrish Puri",
		"Farida Jalal"
	],
	"plot" : "A young man and woman - both of Indian descent but born and raised in Britain - fall in love during a trip to Switzerland. However, the girl's traditional father takes her back to India to fulfill a betrothal promise.",
	"poster" : "http://ia.media-imdb.com/images/M/MV5BNDk3MTU5MDA3OF5BMl5BanBnXkFtZTgwODM3MzY2MzE@._V1_SX300.jpg",
	"imdb" : {
		"id" : "tt0112870",
		"rating" : 8.3,
		"votes" : 37795
	},
	"awards" : {
		"wins" : 12,
		"nominations" : 0,
		"text" : "12 wins."
	},
	"type" : "movie"
}
```

---

### Projections
By default, MongoDB returns all fields for all matching documents as seen in all the results above. Projections reduce network overhead and processing requirements by limiting the fields that are returned in the results documents. Projections are passed as the second argument to the `find()` method. 

**Exercise 10** :computer: 

 Use `<field name>: 1` to include that field in the results document and/or use `<field name>: 0` to exclude that field. Note that the **values** `1` and `0` stand for **inclusion** and **exclusion** in the resulting document respectively. In the query below, we are asking only for the `title` field to be shown in the output. 

**query:**
```javascript
db.movieDetails.find({"genres":["Action", "Adventure"]},{"title":1})
```

**result:**
```javascript
{ "_id" : ObjectId("5d7e92671ddae9be2e725510"), "title" : "MacGyver: Trail to Doomsday" }
{ "_id" : ObjectId("5d7e92671ddae9be2e72557e"), "title" : "Raiders of the Lost Ark" }
{ "_id" : ObjectId("5d7e92671ddae9be2e7255df"), "title" : "Spider-Man" }
{ "_id" : ObjectId("5d7e92671ddae9be2e7255e5"), "title" : "Spider-Man 3" }
{ "_id" : ObjectId("5d7e92671ddae9be2e7257ff"), "title" : "Bruce Li the Invincible Chinatown Connection" }
{ "_id" : ObjectId("5d7e92671ddae9be2e725c77"), "title" : "Bo ming chan dao duo ming qiang" }
```

In the output above, the query returns the `title` field as expected but the `_id` field gets returned by default too.

**Exercise 11** :computer: 

In order to exclude the `_id` field, we have to explicitly exclude it in our query.

**query:**
```javascript
db.movieDetails.find({"genres":["Action", "Adventure"]},
    {"title": 1, "_id": 0})
```
**result:**
```javascript
{ "title" : "MacGyver: Trail to Doomsday" }
{ "title" : "Raiders of the Lost Ark" }
{ "title" : "Spider-Man" }
{ "title" : "Spider-Man 3" }
{ "title" : "Bruce Li the Invincible Chinatown Connection" }
{ "title" : "Bo ming chan dao duo ming qiang" }
```
:book:[Read More](https://docs.mongodb.com/manual/reference/method/db.collection.find/index.html) about the `find()` method.

---

**Lab 1: :alarm_clock: 10 minutes** 

Share with the rest of the class the kind of movies you like by writing 3 interesting queries against this database, at the same time limiting the resulting documents to fields that you find relevant. 

For example, movies that were part of both the 'Action' and 'Adventure' genres! :boom: 


**Lab 2: :alarm_clock: 15 minutes**
Using `some other dbs we have` break into small groups and explore the documents in this database.  Create a small 2-3 slide long presentation about the data.  

Some questions to consider while exploring:
- How is this data structured?
- Is there anything interesing to me about this data?
- Is the data malformed, incorrect or incomplete?
- What are the challenges you faced while exploring this data?

**Lab 3: :alarm_clock: 15 minutes**
Present your results from **Lab 2** and answer any questions your classmates may have.

**are we okay with this :arrow_up: ... or should we expand?**

