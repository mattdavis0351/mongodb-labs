## READ Operations

### Counting records:
**Exercise 1** :computer:

A simple `count()` operation in order to count the number of documents in the collection:

**query:**
```javascript
db.movieDetails.count()
```

### Filtering on a single field:

**Exercise 2** :computer: 

In order to filter data, add a parameter as the first argument to the `count()` method. 
Parameters exist as objects, and are therefore `key:value` pairs.  In the query below, `rated` is the **key** and `PG-13` is the **value**.

**query:**
```javascript
db.movieDetails.count({"rated": "PG-13"})
```

### Filtering on multiple fields:

Adding another selector using the `,` operator ANDs the filters and returns the matching results:

**Exercise 3** :computer: 

**query:**

```javascript
db.movieDetails.count({"rated": "PG-13", "year":1993})
```
### Using the "find()" method:
In order to display the entire document, use the `find()` method: 

**Exercise 4** :computer: 

**query:**
```javascript
db.movieDetails.find({"rated": "PG-13", "year":1993})
```
**result:**
```javascript
{ "_id" : ObjectId("5d7e92671ddae9be2e725550"), "title" : "Son in Law", "year" : 1993, "rated" : "PG-13", "runtime" : 95, "countries" : [ "USA" ], "genres" : [ "Comedy", "Drama", "Romance" ], "director" : "Steve Rash", "writers" : [ "Patrick J. Clifton", "Susan McMartin", "Peter M. Lenkov", "Fax Bahr", "Adam Small", "Shawn Schepps" ], "actors" : [ "Pauly Shore", "Carla Gugino", "Lane Smith", "Cindy Pickett" ], "plot" : "Having gotten a taste of college life, a drastically changed farm girl returns home for Thanksgiving break with her best friend, a flamboyant party animal who is clearly a fish out of water in a small farm town.", "poster" : "http://ia.media-imdb.com/images/M/MV5BMTUxNDkyODMwN15BMl5BanBnXkFtZTYwODA3NjU5._V1_SX300.jpg", "imdb" : { "id" : "tt0108186", "rating" : 5.6, "votes" : 12557 }, "awards" : { "wins" : 0, "nominations" : 1, "text" : "1 nomination." }, "type" : "movie" }
{ "_id" : ObjectId("5d7e92671ddae9be2e7256e7"), "title" : "Ed and His Dead Mother", "year" : 1993, "rated" : "PG-13", "runtime" : 93, "countries" : [ "USA" ], "genres" : [ "Horror", "Comedy" ], "director" : "Jonathan Wacks", "writers" : [ "Chuck Hughes" ], "actors" : [ "Eric Christmas", "Steve Buscemi", "Harper Roisman", "Sam Sorbo" ], "plot" : "A mourning son makes a deal to reanimate his one year dead mother, however things turn into an unexpected direction.", "poster" : "http://ia.media-imdb.com/images/M/MV5BMTc0ODYxODc0MF5BMl5BanBnXkFtZTYwMDMxOTk5._V1_SX300.jpg", "imdb" : { "id" : "tt0106792", "rating" : 6, "votes" : 1428 }, "awards" : { "wins" : 0, "nominations" : 1, "text" : "1 nomination." }, "type" : "movie" }
{ "_id" : ObjectId("5d7e92671ddae9be2e725c45"), "title" : "So I Married an Axe Murderer", "year" : 1993, "rated" : "PG-13", "runtime" : 93, "countries" : [ "USA" ], "genres" : [ "Comedy", "Crime", "Romance" ], "director" : "Thomas Schlamme", "writers" : [ "Robbie Fox" ], "actors" : [ "Mike Myers", "Nancy Travis", "Anthony LaPaglia", "Amanda Plummer" ], "plot" : "A San Francisco poet who fears commitment has a girlfriend who he suspects may not be who she appears.", "poster" : "http://ia.media-imdb.com/images/M/MV5BMTY3OTY0NzYzMV5BMl5BanBnXkFtZTcwMDc2MjYxMQ@@._V1_SX300.jpg", "imdb" : { "id" : "tt0108174", "rating" : 6.4, "votes" : 27102 }, "awards" : { "wins" : 0, "nominations" : 0, "text" : "" }, "type" : "movie" }
```

### Using the "pretty()" method:

Chain the `pretty()` method to the `find()` method for a pretty print.

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
{
	"_id" : ObjectId("5d7e92671ddae9be2e7256e7"),
	"title" : "Ed and His Dead Mother",
	"year" : 1993,
	"rated" : "PG-13",
	"runtime" : 93,
	"countries" : [
		"USA"
	],
	"genres" : [
		"Horror",
		"Comedy"
	],
	"director" : "Jonathan Wacks",
	"writers" : [
		"Chuck Hughes"
	],
	"actors" : [
		"Eric Christmas",
		"Steve Buscemi",
		"Harper Roisman",
		"Sam Sorbo"
	],
	"plot" : "A mourning son makes a deal to reanimate his one year dead mother, however things turn into an unexpected direction.",
	"poster" : "http://ia.media-imdb.com/images/M/MV5BMTc0ODYxODc0MF5BMl5BanBnXkFtZTYwMDMxOTk5._V1_SX300.jpg",
	"imdb" : {
		"id" : "tt0106792",
		"rating" : 6,
		"votes" : 1428
	},
	"awards" : {
		"wins" : 0,
		"nominations" : 1,
		"text" : "1 nomination."
	},
	"type" : "movie"
}
{
	"_id" : ObjectId("5d7e92671ddae9be2e725c45"),
	"title" : "So I Married an Axe Murderer",
	"year" : 1993,
	"rated" : "PG-13",
	"runtime" : 93,
	"countries" : [
		"USA"
	],
	"genres" : [
		"Comedy",
		"Crime",
		"Romance"
	],
	"director" : "Thomas Schlamme",
	"writers" : [
		"Robbie Fox"
	],
	"actors" : [
		"Mike Myers",
		"Nancy Travis",
		"Anthony LaPaglia",
		"Amanda Plummer"
	],
	"plot" : "A San Francisco poet who fears commitment has a girlfriend who he suspects may not be who she appears.",
	"poster" : "http://ia.media-imdb.com/images/M/MV5BMTY3OTY0NzYzMV5BMl5BanBnXkFtZTcwMDc2MjYxMQ@@._V1_SX300.jpg",
	"imdb" : {
		"id" : "tt0108174",
		"rating" : 6.4,
		"votes" : 27102
	},
	"awards" : {
		"wins" : 0,
		"nominations" : 0,
		"text" : ""
	},
	"type" : "movie"
}
```
### Filtering embedded documents:
Driving down the hierarchy in documents and filtering on nested levels can be done using the dot notation.

**Exercise 6** :computer: 

**query:**

```javascript
db.movieDetails.find({"imdb.rating":9.5}).pretty()
```

**result:**
```javascript
{
	"_id" : ObjectId("5d7e92671ddae9be2e725d04"),
	"title" : "Liverpool FC: Champions of Europe 2005",
	"year" : 2005,
	"rated" : null,
	"runtime" : 90,
	"countries" : [
		"UK"
	],
	"genres" : [
		"Sport"
	],
	"director" : null,
	"writers" : [ ],
	"actors" : [
		"Xabi Alonso",
		"Milan Baros",
		"Rafael Benítez",
		"Igor Biscan"
	],
	"plot" : "At the start of the season there was a nervous excitement at Anfield. With a new manager and some potentially excellent signings Liverpool looked to improve on their performances in ...",
	"poster" : null,
	"imdb" : {
		"id" : "tt0469787",
		"rating" : 9.5,
		"votes" : 410
	},
	"awards" : {
		"wins" : 0,
		"nominations" : 0,
		"text" : ""
	},
	"type" : "movie"
}
```
