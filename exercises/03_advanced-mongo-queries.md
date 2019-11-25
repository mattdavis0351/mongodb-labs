# Advanced Queries

Modeled on the data processing pipeline, aggregation is a powerful feature that allows documents to enter a pipeline, go through multiple stages and compute an aggregated result. 

[Read more](https://docs.mongodb.com/manual/aggregation/#aggregation-framework) :book: about the aggregation pipeline. 

:arrow_right: When writing queries using the aggregate pipeline, we use the `aggregate()` function as follows: 
```javascript
db.<collection_name>.aggregate([
    {pipeline_stage_1 in the form of key: value pairs},
    {pipeline_stage_2 in the form of key: value pairs},
    ...
])
```

### The "$group" operator

The `$group` operator groups the documents by an identifier specified by `_id` field, and based on that distinct grouping, performs an agggregation like `$sum` and returns the resulting documents. 

[Read more](https://docs.mongodb.com/manual/reference/operator/aggregation/group/) :book: about **$group**. 

:warning: Before diving into the exercises, switch over to the `cricket_players` collection in the `test` database.

**Exercise 1** :computer: 

Suppose there is a dearth in willow production this year and the bat manufacturer can only supply bats for either right-handed or left-handed batsmen but not both. Write a query for the bat manufacturer that groups the players by batting hand so that you can inform him which kind of bat he should put into production to gain maximum revenue. 

**query:**
```javascript
db.cricket_players.aggregate([
    {$group: 
        {_id: "$Batting_Hand", 
        playerCountByBattingHand: {$sum: 1}
        }
    }
])
```

**result:**
```javascript
{ "_id" : "NULL", "playerCountByBattingHand" : 52 }
{ "_id" : "Left_Hand", "playerCountByBattingHand" : 126 }
{ "_id" : "Right_Hand", "playerCountByBattingHand" : 345 }
```

:arrow_right: A word on the aggregate pipleine syntax: By now, we're all familiar with the JSON format of **key: value** pairs. 
- Whenever a field is accessed as the **key**, it is written as it is. We've seen numerous examples of this starting from our very first filtering query `db.movieDetails.count({"rated": "PG-13"})`. 
    - Here the field `rated` is used as a key and is simply written as `rated`.
    
- Whenever a field is accessed as the **value**, which you'll see frequently in the aggregation pipeline queries, it is denoted with a `$` before the name and must be in quotes. For example, in the query above, you saw `{_id: "$Batting_Hand"}`. 
    - Here the field `Batting_Hand` is used as a value and written as `$Batting_Hand`. In this case, `$Batting_Hand` is acting as a variable or a placeholder for each of its distinct values ("NULL", "Left_Hand", "Right_Hand") which are then returned in the output documents. 
- `playerCountByBattingHand` is a user-defined label, we could've named that anything we wanted.

**Exercise 2** :computer: 

There is talk of adding cricket to the Olympic sports. But before a conclusive decision can be made, the Olympic board needs to find out if its even feasible to do so. Write a query for the Olympic board informing them of the number of players in each country.

**query:**
```javascript
db.cricket_players.aggregate([
    {$group: 
        {_id : "$Country", 
        playerCountByCountry : {$sum: 1}
        }
    }
])
```

:bulb: Only 35 players have ever captained Inida's national cricket team!

**Exercise 3** :computer: 
The Olympic board is well aware of the willow crisis. Therefore, to be on the safe side, they have one more request for you. They would like to know the number of players of each country that bat with a given hand.

**query:**
```javascript
db.cricket_players.aggregate([
    {$group:
        {_id:{"Country":"$Country",     
        "Batting_Hand":"$Batting_Hand"}, 
        playerCount:{$sum:1}
        }
    }
])
```
:warning: Wait a minute! Before you send these over to the bat manufacturer or the Olympic board, let's work on making these query results more readable. We'll do this in the following exercises.

---

### The "$match" operator

The `$match` operator matches input documents to a given criteria and passes those matched documents to the next stage of the pipeline. 

[Read more](https://docs.mongodb.com/manual/reference/operator/aggregation/match/) :book: about **$match**.

**Exercise 4** :computer: 

We have observed `NULL` values in our previous results and even though it's important for us as database engineers to know what data is missing, our end users like the bat manufacturer or the Olympic board can't really do much with the `NULL` values. Let's improve our queries then to exclude the `NULL` values when grouping the players by batting hand.

**query:**
```javascript
db.cricket_players.aggregate([
    {$match: 
        {Batting_Hand:{$ne: "NULL"}}
    },
    {$group:
        {_id : "$Batting_Hand", 
        playerCountByBattingHand : {$sum: 1}
        }
    }
])
```

:arrow_right: The query above starts to highlight the :muscle: of the aggregation pipeline. 
- `$match` is the first stage in this pipeline which matches only those documents where `Batting_Hand` is not equal to "NULL". 
- These documents are then sent to the second stage of the pipeline where the `$group` operation groups the players by distinct non-null values of`Batting_Hand`.

:bulb: Why might this be important when we query data?  

Imagine if there was a room down the corridor that was full of colored boxes.  Then we asked you to go get all of those boxes and bring them into this room.  Now that all the boxes were in our room, we decided to tell you that we only want the blue boxes, and the rest need to stay in the other room! Now you'd have to separate the blue boxes from the rest, and make another trip to return the unwanted boxes.  Once you get back to our current room we ask you to count the number of blue boxes.

By using **$match** we can say "go to the box room and find all the blue ones, bring them back to our current room, once they are all here we will count them".

**Exercise 5** :computer: 

Similar to Exercise 4, let's count the number of players by non null country.

**query:**
```javascript
db.cricket_players.aggregate([
    {$match: 
        {Country:{$ne: "NULL"}}
    },
    {$group:
        {_id : "$Country", 
        playerCount : {$sum: 1}
        }
    }
])
```

### The "$sort" operator

**Exercise 6** :computer: 

One last thing we can do to ease readability for the Olympic board is to sort the players in alphabetical order in addition to all the changes we implemented previously. Put all your knowledge together and count number of players of each country that bat with a given hand. Remove null values of `Batting_Hand` and sort the output in alphabetical order. 

**query:**
```javascript
db.cricket_players.aggregate([
    {$match:
        {Batting_Hand:{$ne:"NULL"}}
    },
    {$group:
        {_id:{"Country":"$Country", 
        "Batting_Hand":"$Batting_Hand"}, 
        playerCount:{$sum:1}
        }
    },
    {$sort:
        {_id:1}
    }
])
```

:tada: Great! I'm sure looking at this easy-to-read result, you might stand a chance of getting a free ticket or two to the next Olympics! :wink: 

:arrow_right: The `$sort` operator takes a **key: value** pair as input:
- **key** being the field by which to sort (in our case `_id`) 
- **value** being the sort direction (1 for acending order and -1 for descending order.) 

[Read more](https://docs.mongodb.com/manual/reference/operator/aggregation/sort/) :book: about **$sort**. 

---

### The "$unwind" operator

The `$unwind` operator deconstructs an array resulting in a document for **each** array element. The concept will become more evident through the exercise. 

:warning: Before diving into the exercises, switch over to the `employee_info` collection in the `employee_db` database that was setup earlier. 

**Exercise 7** :computer: 

Our employees are members of different departments. Deconstruct the `deparments` array such that there is a separate document for each department an employee belongs to. 

**query:**
```javascript
db.employee_info.aggregate([{$unwind: "$departments"}]).pretty()
```

**result:** (Only a part of the output has been shown.)
```javascript
{
	"_id" : ObjectId("5d93c37ed5b37ed12379b1a2"),
	"empno" : 1,
	"fname" : "charlie",
	"lname" : "rodgers",
	"role" : "manager",
	"departments" : "sales"
}
{
	"_id" : ObjectId("5d93c37ed5b37ed12379b1a2"),
	"empno" : 1,
	"fname" : "charlie",
	"lname" : "rodgers",
	"role" : "manager",
	"departments" : "marketing"
}
```

As you can see in the result above, the document of `Charlie Rodgers` occurs twice with each document containing a **unique deconstructed array element** from the `departments` array. Also, make note that `_id` stayed the same when deconstructed.

:arrow_right: A more precise method of writing the query would be to specify the path of what you are trying to `$unwind`.

**query:**
```javascript
db.employee_info.aggregate([{$unwind: {path: "$departments"}}]).pretty()
```

**Exercise 8** :computer: 

In order to find an object's location in an array, include the index position. 

**query:**
```javascript
db.employee_info.aggregate([{$unwind: {
    path: "$departments", 
    includeArrayIndex: "arrayIndex"
    }}]).pretty()
```

Do you :eyes: the difference in output? Also make note that array indexes start with 0!

**Exercise 9** :computer: 

Let's crunch some numbers! Count the number of departments an employee belongs to.

**query:**
```javascript
db.employee_info.aggregate([
    {$unwind: "$departments"}, 
    {$group:
        {_id:{firstName:"$fname"}, 
        numberOfDepartments:{$sum:1}
        }
    }
])
```

Let's twist it around! Spend **5 minutes** :alarm_clock: to write a query that counts the number of employees in each department. To make things more interesting, perform this action only against employees having `empno` greater than or equal to 3.

**query:**
```javascript
db.employee_info.aggregate([
    {$unwind: "$departments"},
    {$match: 
        {empno: {$gte: 3}}
    },
    {$group: 
        {_id: {departmentName: "$departments"}, 
        numberOfEmployees: {$sum:1}
        }
    }
])
```

---
  
## Group Activities 
During this time the class should split into :five: groups to complete each section.

**Activity: :alarm_clock: 15 minutes** 

Explore the `pokemon` collection and show off your aggregation pipeline skills by matching, grouping, sorting and unwinding arrays. Time to massage those brains! :massage: 


**Presentation: :alarm_clock: 25 minutes**

Each group will prepare a small, **5 minute**, 2-3 slide presentation about their queries and output.

Present your results from the **Group Activity** and answer any questions your classmates may have.
