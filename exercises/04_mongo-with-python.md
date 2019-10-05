![image](https://user-images.githubusercontent.com/38021615/66251170-fb620780-e701-11e9-9d80-cb36e88a935b.png)
## MongoDB using PyMongo

In this document, we're going to look at running Mongo queries with Python. The advantage of this kind of querying is that we don't have to leave the Python environment in favor of the Mongo shell and can fire all our queries pythonically. 

### Installing the package

Before we dive into the queries, let's install the `pymongo` package which is a native Python driver for MongoDB. 

- Left click on the windows button(bottom left corner) on your desktop.
- Type the word `Anaconda` and you should see the `Anaconda Prompt` application appear in the search results.
- Right click on `Anaconda Prompt` and select `Run As Administrator` and enjoy your cosmic powers.
- We now need to use `pip` to install two packages:
  - Type `pip install msgpack`, this package is required for `pymongo` to install correctly.  Once that operation completes move to the next one.
  - Type `pip install pymongo` and wait for this operation to complete. 
- Once successful, you can close the `Anaconda Prompt`. 
- And onto :snake: we go!  Open **Spyder** and get ready to code!

---

### Importing the package

- Using **Spyder** complete the following exercises. 
- **Optional:** Create a new file and name it whatever you want. Make sure to use the extension `.py` when saving it. I'm going to call my file `mongo_with_python.py`. Real imaginative, I know! :stuck_out_tongue: 
- Now that we have our python file, let's get started with the first line of Python code which will be to let the program know what packages we're going to reference. More specifically, we're going to be referencing the `MongoClient` class from within the `pymongo` package. 

**code:**
```python
from pymongo import MongoClient
```

:arrow_right: If `pymongo` was successfully installed, the above line of code should run without any errors. There will be no output displayed though so don't expect one yet!

---

### Establishing and verifying the connection

There's a little more groundwork to lay before we can start querying our database. We have to establish a connection to the running Mongo client and let the program know which database we're actually talking about. 

However, before telling the program, let me tell you guys first! Here's the big reveal - we're going to be querying the `titanic` :ship: collection throughout this document! 


**code:**
```python
client = MongoClient("localhost", 27017)
db = client["test"]
print("Total number of documents in the 'titanic' collection: ", db.titanic.count_documents({}))
print(db.titanic.find_one())
```

What we did above was to establish a connection to the running Mongo client, which for us is running on `localhost` at port # `27017`. We also initialized a variable `db` to the `test` database that you've interacted with and is currently running in MongoDB. Within this `db`, we'll be running queries against the `titanic` collection. 

We followed this up with a simple `count_documents()` and     `find_one()` operation as a sanity check, to ensure we were successful in establsihing our connection to the data. 

---

### Filtering

Filtering documents in Python is similar to filtering in the Mongo shell. The filter criteria is written as a **key: value** pair here too. Also, use of the `,` operator **AND**s the filters and returns the matching results. 

**Exercise 1** :computer: 

Let's get to the point and answer the most compelling question about the Titanic - How many people made it out alive?

**code:**
```python
print(db.titanic.count_documents(
    {"survived":1}
))
```

And how many children on board survived?

**code:**
```python
print(db.titanic.count_documents(
    {"$and":[
        {"survived":1}, 
        {"age":{"$lt":18}}
        ]
    }
))
```

:arrow_right: Note that just like in the Mongo shell, the `$and` operator takes an array of **key: value** pairs as input. 

---

### Presence

Unlike relational databases, presence isn't compulsory in MongoDB. Fields present in one document need not be present in another document. The `$exists` operator comes in handy and is used to check for presence of a given field.

**Exercise 2** :computer: 

Sometimes fate plays a terrible hand. Were there any passengers who weren't supposed to be there? :eyes: Or were all the passengers legit?

**code:**
```python
print(db.titanic.count_documents(
    {"ticket_number":{"$exists":False}}
))
```

---

### Distinct values

Distinct values help determine the unique values that are present for a given field. This aids in data exploratory purposes. 

**Exercise 3** :computer: 

:bulb: Did you know that Titanic II is estimated to set sail in 2022 and will be traversing the same route as the original Titanic?! Would you dare to go and if yes, at what port would you embark the :ship: ? 

**code:**
```python
print(db.titanic.distinct("point_of_embarkation"))
```

Assume you're one of the daredevils that decided to sail on Titanic II! Spend **10 minutes** :alarm_clock: analyzing the data and let us know which port would be the port of your choice? And why? Feel free to use the internet to look up the port's full names, if you wish. 

---

### Projections

Something we've already seen in the Mongo shell, projections help us limit the fields that are returned in the results. The same rules apply:
Use **{"field name": 1}** to include that field in the results document and/or use **{"field name": 0}** to exclude that field. The values 1 and 0 stand for **inclusion** and **exclusion** in the resulting document respectively. 

**Exercise 4** :computer: 

Have you ever wondered what was the biggest family on board the :ship: ? Let's start with finding out if there were any family sizes greater than 5.

**code:**
```python
for doc in db.titanic.find(
    {"parents_children":{"$gte":5}}, 
    {"name":1, "parents_children":1, "_id":0}):
    print(doc)
```

:arrow_right: We ran the query in a `for` loop so that it would loop over all the documents and print them as it iterated over each one. Using programming concepts like **loops** in conjunction with MongoDB query concepts like **key: value filters** and **projections** make for :muscle: querying! 

Spend **5 minutes** :alarm_clock: tackling the above exercise with the `distinct` method that you learnt earlier. Your output will contain an array of all the distinct family sizes, from which you'll easily be able to tell the largest one. 

**Exercise 5** :computer: 

If we aren't excluding any fields in our projections, `pymongo` accepts another simpler format of simply passing the fields to be included as:
- an array
- in double quotes
- separated by commas

Go ahead and try it!

**code:**

```python
for doc in db.titanic.find(
    {"parents_children":{"$gte":5}}, 
    ["name", "parents_children"]):
    print(doc)
```

:bulb: Do you :eyes: the difference in outputs?

---

### Sorting

Sorting in Python is slightly different as compared to what you learnt through the Mongo shell. Instead of key: value pairs, you'll be passing tuples instead. These tuples are of the format:
**(field name, sort direction)** 
where sort direction: 1 means ascending order whereas -1 means descending order. 

**Exercise 6** :computer: 

On the other side of the globe, in the land of the :us:, a quotation was penned in the 'Declaration of Independence' which has since gained worldwide popularity: "All men are created equal." Not everyone aboard the :ship: could be saved on that fateful night, but let's find out if the quotation held true atleast among children.

**code:**

```python
for doc in db.titanic.find(
    {"age":{"$lt":18}}, 
    sort = [("class", -1), ("survived", 1)]):
    print(doc["class"],doc["survived"])
```

:arrow_right: Analyze this output for **5 minutes** :alarm_clock: To aid in your analysis, make note that:
- there were 3 classes aboard: first, second and third class. 
- `survived` has 2 values: 1 for the lucky ones who weathered the storm and 0 for the ones who didn't.

Was the quotation above proved right or is there a clear distinction in the children who survived versus the ones who died? 

**Exercise 7** :computer: 

Let's take it a step further. What about introducing `gender` into the mix? Do you see any patterns there?

**code:**

```python
for doc in db.titanic.find(
    {"age":{"$lt":18}}, 
    sort = [("class", -1), ("gender", -1), ("survived", 1)]):
    print(doc["class"], doc["gender"], doc["survived"])
```

---

### Aggregation Pipeline

Last but definitely not the least, we talk about the aggregation pipeline. You already have an idea of the multiple stages in the pipeline and how results from one stage are passed onto the next. 

Here, let's see how the magic happens in :snake: !


**Exercise 8** :computer: 

So far, we've talked of :baby:, :man:, :woman: and :family: which is why the :older_man: and :older_woman: are feeling a little left out! Let's invite them to the query party! 

Analyze the`class` and `gender` of people aged 70 and above through an aggregation pipeline.

```python
from collections import OrderedDict
var = db.titanic.aggregate([
    {"$match":{"age":{"$gte":70}}},
    {"$project":{"class":1, "gender":1, "survived":1}},
    {"$sort":OrderedDict([("class", 1), ("gender", 1)])},
    ])
for doc in var:
    print(doc["class"], doc["gender"], doc["survived"])
```

:arrow_right: Note that in this case, `$sort` is part of a key: value pair, unlike earlier. And in the Python world, key: value pairs are equivalent to a dictionary. Python dictionaries do **not** maintain their order hence to preserve the sort order, we import a class called `OrderedDict` from the `collections` package and use it on our `$sort` array.

---
### Group Activities

During this time the class should split into :five: groups to complete each section.

**Activity: :alarm_clock: 15 minutes**

- Explore a MongoDB collection of your choice and put your :snake: skills to use! Come up with interesting queries. 
- Check out list comprehensions in Python and convert your queries into super geeky one liners! 
- Bring your data to life by attempting to visualizing your results with a Python visualization library like `matplotlib`!

**Presentation: :alarm_clock: 25 minutes**

Each group will prepare a small, 5 minute, 2-3 slide presentation about their queries and output.

Present your results from the **Group Activity** and answer any questions your classmates may have.
