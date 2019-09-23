# READ Operations

### Counting records:
**Exercise 1** :computer:

A simple `count()` operation in order to count the number of documents in the collection.

**query:**
```javascript
db.movieDetails.count()
```

---

### Filtering on a single field:

**Exercise 2** :computer: 

In order to filter data, add a parameter as the first argument to the `count()` method. Parameters exist as objects, and are therefore `key:value` pairs. In the query below, `rated` is the **key** and `PG-13` is the **value**.

**query:**
```javascript
db.movieDetails.count({"rated": "PG-13"})
```
---

### Filtering on multiple fields:

Adding another selector using the `,` operator **ANDs** the filters and returns the matching results.

**Exercise 3** :computer: 

**query:**

```javascript
db.movieDetails.count({"rated": "PG-13", "year":1993})
```
---

### Using the "find()" method:
In order to display the entire document, use the `find()` method.

**Exercise 4** :computer: 

**query:**
```javascript
db.movieDetails.find({"rated": "PG-13", "year":1993})
```
**result:**
```javascript
{ "_id" : ObjectId("5d7e92671ddae9be2e725550"), "title" : "Son in Law", "year" : 1993, "rated" : "PG-13", "runtime" : 95, "countries" : [ "USA" ], "genres" : [ "Comedy", "Drama", "Romance" ], "director" : "Steve Rash", "writers" : [ "Patrick J. Clifton", "Susan McMartin", "Peter M. Lenkov", "Fax Bahr", "Adam Small", "Shawn Schepps" ], "actors" : [ "Pauly Shore", "Carla Gugino", "Lane Smith", "Cindy Pickett" ], "plot" : "Having gotten a taste of college life, a drastically changed farm girl returns home for Thanksgiving break with her best friend, a flamboyant party animal who is clearly a fish out of water in a small farm town.", "poster" : "http://ia.media-imdb.com/images/M/MV5BMTUxNDkyODMwN15BMl5BanBnXkFtZTYwODA3NjU5._V1_SX300.jpg", "imdb" : { "id" : "tt0108186", "rating" : 5.6, "votes" : 12557 }, "awards" : { "wins" : 0, "nominations" : 1, "text" : "1 nomination." }, "type" : "movie" }
```


Chain the `pretty()` method to the `find()` method for a prettier print of the output.

**Exercise 5** :computer: 

**query:**
```javascript
db.movieDetails.find({"rated": "PG-13", "year":1993}).pretty()
```

**result:**
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
---

### Filtering embedded documents:
Driving down the hierarchy in documents and filtering on nested levels can be done using the dot notation.

**Exercise 6** :computer: 

**query:**

```javascript
db.movieDetails.find({"imdb.rating":9.5}).pretty()
```
---

### Filtering array values:

When it comes to matching on array values, we can match:
* the entire array

The following query matches documents with the array of `genres` containing **exactly** 'Documentary' followed by 'Family', in that order. Note that arrays that contain 'Documentary', 'Family' and any other genre like 'Drama' or 'Comedy' are not returned because it is **not** an exact match of the filter criteria. 

**Exercise 7** :computer: 

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

**Exercise 8** :computer: 
* any element in the array

A more common search criteria is filtering out a single element in the array, irrespective of its array position. Note that 'Musical' is matched irrespective of its position in the array. 

**query:**
```javascript
db.movieDetails.find({"genres":"Musical"}).pretty()
```
**result:**
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
**Exercise 9** :computer: 

* array position (for eg: arrays whose first element match a particular criteria)

Sometimes array positions matter as in the case of the `actors` array where names are listed in the order of contribution. In order to achieve this, use dot notation to specify an array index.

**query:**
```javascript
db.movieDetails.find({"actors.0":"Shah Rukh Khan"}).pretty()
```

**result:**
```javascript
{
	"_id" : ObjectId("5d7e92671ddae9be2e7257e5"),
	"title" : "One 2 Ka 4",
	"year" : 2001,
	"rated" : null,
	"runtime" : 169,
	"countries" : [
		"India"
	],
	"genres" : [
		"Action",
		"Comedy",
		"Drama"
	],
	"director" : "Shashilal K. Nair",
	"writers" : [
		"Sanjay Chhel",
		"Raaj Kumar Dahima",
		"Manoj Lalwani"
	],
	"actors" : [
		"Shah Rukh Khan",
		"Juhi Chawla",
		"Jackie Shroff",
		"Nirmal Pandey"
	],
	"plot" : "Javed Abbas (Jackie Shroff) is widowed Police Officer with four children. His partner is Arun Verma (Shahrukh Khan). Both Javed and Arun are honest, hard-working and diligent. And this ...",
	"poster" : "http://ia.media-imdb.com/images/M/MV5BMTYzODgyMDkyMF5BMl5BanBnXkFtZTcwNjg4NzUyMQ@@._V1_SX300.jpg",
	"imdb" : {
		"id" : "tt0227194",
		"rating" : 5.4,
		"votes" : 3083
	},
	"awards" : {
		"wins" : 0,
		"nominations" : 0,
		"text" : ""
	},
	"type" : "movie"
}
{
	"_id" : ObjectId("5d7e92671ddae9be2e7258be"),
	"title" : "Yeh Lamhe Judaai Ke",
	"year" : 2004,
	"rated" : null,
	"runtime" : 135,
	"countries" : [
		"India"
	],
	"genres" : [
		"Drama"
	],
	"director" : "Birendra Nath Tiwari",
	"writers" : [ ],
	"actors" : [
		"Shah Rukh Khan",
		"Raveena Tandon",
		"Navneet Nishan",
		"Avtar Gill"
	],
	"plot" : "Dushyant and Jaya are childhood friends. In spite of his family's poverty, Dushyant has one great ambition in life: to become a successful singer. Jaya encourages him all she can and helps ...",
	"poster" : "http://ia.media-imdb.com/images/M/MV5BMjkxZDc3NjAtMWNjMC00YWI5LTkxNzgtOGQ3MzM2NTMyODk2XkEyXkFqcGdeQXVyNDUzOTQ5MjY@._V1_SX300.jpg",
	"imdb" : {
		"id" : "tt0385351",
		"rating" : 4.1,
		"votes" : 1179
	},
	"awards" : {
		"wins" : 0,
		"nominations" : 0,
		"text" : ""
	},
	"type" : "movie"
}
{
	"_id" : ObjectId("5d7e92671ddae9be2e7258ec"),
	"title" : "Rab Ne Bana Di Jodi",
	"year" : 2008,
	"rated" : null,
	"runtime" : 167,
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
		"Aditya Chopra"
	],
	"actors" : [
		"Shah Rukh Khan",
		"Vinay Pathak",
		"Anushka Sharma",
		"M.K. Raina"
	],
	"plot" : "A middle-aged man who lost his love for life rediscovers it by assuming a new identity in order to rekindle the romantic spark in his marriage.",
	"poster" : "http://ia.media-imdb.com/images/M/MV5BNTU5MjI3MTE0NV5BMl5BanBnXkFtZTcwMDk4OTUxMg@@._V1_SX300.jpg",
	"imdb" : {
		"id" : "tt1182937",
		"rating" : 7.1,
		"votes" : 26562
	},
	"awards" : {
		"wins" : 3,
		"nominations" : 11,
		"text" : "3 wins & 11 nominations."
	},
	"type" : "movie"
}
{
	"_id" : ObjectId("5d7e92671ddae9be2e725943"),
	"title" : "Om Shanti Om",
	"year" : 2007,
	"rated" : "PG-13",
	"runtime" : 162,
	"countries" : [
		"India"
	],
	"genres" : [
		"Action",
		"Comedy",
		"Drama"
	],
	"director" : "Farah Khan",
	"writers" : [
		"Farah Khan",
		"Mushtaq Sheikh",
		"Farah Khan",
		"Mayur Puri"
	],
	"actors" : [
		"Shah Rukh Khan",
		"Arjun Rampal",
		"Kiron Kher",
		"Shreyas Talpade"
	],
	"plot" : "In the 1970s, Om, an aspiring actor, is murdered, but is immediately reincarnated into the present day. He attempts to discover the mystery of his demise and find Shanti, the love of his previous life.",
	"poster" : "http://ia.media-imdb.com/images/M/MV5BMTgwMTc5MzMxOF5BMl5BanBnXkFtZTcwOTA4MTU1MQ@@._V1_SX300.jpg",
	"imdb" : {
		"id" : "tt1024943",
		"rating" : 6.6,
		"votes" : 25825
	},
	"awards" : {
		"wins" : 32,
		"nominations" : 21,
		"text" : "32 wins & 21 nominations."
	},
	"type" : "movie"
}
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

### Projections:
By default, MongoDB returns all fields for all matching documents. Projections reduce network overhead and processing requirements by limiting the fields that are returned in the results documents. Projections are defined as the second argument to the `find()` method. 

**Exercise 10** :computer: 

In the output below, the query returns the movie titles as expected but the `_id` field gets returned by default too. 

**query:**
```javascript
db.movieDetails.find({
    "genres":["Action", "Adventure"]},
    {"title":1})
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

**Exercise 11** :computer: 

In order to exclude the `_id` field, we have to state it explicitly in our query.

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

All in all, use `<field name>: 1` to include that field in the results document and/or use `<field name>: 0` to exclude that field. 

What's the output of the following query?

```javascript
db.movieDetails.find({
    "genres":["Action", "Adventure"]},
    {"_id": 0, "director": 0, "writers": 0, "actors": 0})
    .pretty()
```
---
