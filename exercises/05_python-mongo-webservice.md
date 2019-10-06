# Python Webservice with Mongo and Flask

## Components

- **Flask:** is the most popular Python web application framework.  This is the Python library that is going to power our endpoints when we make a http request. :book: [Read More](http://flask.palletsprojects.com/en/1.1.x/) about Flask.
- **templates:** this is going to be a folder in our project that will house our `html` files.  These files will be rendered by `Flask` when a particular endpoint is navigated to in your web browser.

:warning: This is not a comprehensive discussion about `Flask` or `HTML`, rather it is a series of exercises to get you familiar with web services and how you may interact with them.

## Setting Up

Steps:

1. Create a project folder named `API`
2. Navigate inside the `API` folder and create the following items:
    - A folder named `templates`
    - A file named `app.py`
      -  Becuase of the way namespaces work in Python, you cannot name your Python file `Flask.py` so for the sake of these exercises make sure your Python file is named `app.py`!
  
3. Navigate inside of the `templates` folder and add the create the following files:
  - `docs.html`
  - `index.html`
  - `mongoinsert.html`
  - `query.html`
4. Copy and paste the following code into its respective files:

      **docs.html**
      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <meta http-equiv="X-UA-Compatible" content="ie=edge">
          <title>API Docs</title>
      </head>
      <body>
          <h1>Yo... you should really put some helpful stuff here</h1>
          <p>As you come across API's in real life you will find they typically have nice documentation that shows what endpoints are available.</p>
          <p>You should consider being a good neighbor and doing the same in this document, but for learning purposes this will work just fine as our docs</p>
          <h3>To start make a GET request to the following endpoint to see the API in action:</h3>
          <p>/api/v1/mongo/insert</p>
      </body>
      </html>
      ```
      
      **index.html**
      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <meta http-equiv="X-UA-Compatible" content="ie=edge">
          <title>Web Services</title>
      </head>
      <body>
          <h1>Welcome to your web service!</h1>
          <p>In reality this page might contain a product, or some other functionality of a web application.  Consider YouTube for example.
              When you reach the home page you are greeted with the ability to play videos, but hidden deep within is an API, similiar to what
              you will be building in the next few days.  By making specific HTTP requets to certain URLs we can be greeted with a world of
              hidden functionality.
          </p>
          <h4>click <a href="/api/v1">here</a> to view documentation on how to use this API</h4>
      </body>
      </html>
      ```
      **mongoinsert.html**
      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <meta http-equiv="X-UA-Compatible" content="ie=edge">
          <title>Document</title>
      </head>
      <body>
          <h2>Submit this form to POST data into Mongo</h2>
          <form action="http://localhost:35080/api/v1/mongo/insert" method="POST">
              <p>First Name</p><input type="text" name="fname"/>
              <p>Occupation</p><input type="text" name="occupation"/>
              <p>Occupation2</p><input type="text" name="occupation">
              <p>Home Number</p><input type="text" name="phone"/>
              <p>Mobile Number</p><input type="text" name="phone"/>
              <input type="submit" value="submit">
         </form>
      </body>
      </html>
      ```

      **query.html**
      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <meta http-equiv="X-UA-Compatible" content="ie=edge">
          <title>Document</title>
      </head>
      <body>
          <form action="http://localhost:35080/api/v1/mongo/find" method="POST">
              <p>First Name</p><input type="text" name="fname"/>
       
              <input type="submit" value="submit">
          </form>
      </body>
      </html>
      ```
 5. Open the `Anaconda Prompt` as administrator and `pip install flask` along with any dependencies that appear.
 6. Open `app.py` in your editor and get ready to being building our web service :tada:
 
 ## Getting Our Tools Around
 
For our API we will use the Flask library.  Flask allows us to serve our code with a lightweight web server.  Flask can be used in poduction environments, however, if you plan to implement it the way I do in this document, then you're going to have a bad time in the long run.

We also need a MongoDB library , `pymongo`, so that we can implement CRUD operations with our API.
At the top of your `app.py` file place the following statments:


**app.py**

```python
from flask import Flask, request, jsonify, render_template
import pymongo
```
Next, we need to create our initial Flask instance and set its `DEBUG` features to give us helpful information in the event that things go wrong:
```python
app = Flask(__name__)
app.config["DEBUG"] = True

```

:bulb: If you are using a single module (as in this example), you should use `__name__` because depending on if it’s started as an application or imported as a module, the name will be different (`__main__` versus the actual import name).

Lastly, let's check to see if we can start our `Flask` application.  We are going to tell our app to run and add a `PORT` for our applicaiton to listen on:
**app.py**
```python
app.run(port=35080)
```

**Complete View of app.py**
```python
from flask import Flask, request, jsonify, render_template
import pymongo

app = Flask(__name__)
app.config["DEBUG"] = True

app.run(port=35080)
```

Using Python run **app.py** and observe the terminal.  You should see output similar to that below, indicating that flask is running on `localhost:35080`

![image](https://user-images.githubusercontent.com/38021615/66258917-ad7bec80-e75f-11e9-8ec1-a5ebc2c165c6.png)

Press `Ctrl+C` to stop the server and head back to your **app.py** file to start building endpoints.

## API Endpoints

`Endpoints`, also known as `Routes`, are locations on our server that have content which is meant to be accessed.  Access to this content can take place through a web browser such as **Google Chrome** or **Safari**, programatcially by processing `HTTP` requests or using a utility like `POSTMAN` to query the resources.
We navigate to endpoints on a regular basis as we use the internet.  You're doing so currently as you view this file.  We can view the specific `Endpoint` or `Route` our currently accessed resource is at by examining the URL in the address bar of our browser.
Consider what happens when you visit [Facebook](https://www.facebook.com), you type `www.facebook.com` into your browser and this usually takes you to the `root` endpoint which is denoted with a `/` character.

What if I want to **create a page** for a Band using Facebook?  Kindly Facebook provides us an `Endpoint` to reach the resources necessary to do so:
[create a page](https://www.facebook.com/pages/creation/?ref_type=registration_form) uses the `Endpoint` of `/pages/creation`.  This means we started at the root, `/`, then from there we found a location called `pages/` which is some sort of directory, and within it we found `creation` which could be a static file or some kind of functionality that Facebook provides.

## Creating Endpoints

**Exercise 1:**
Let's walk through building a few simple endpoints so that we can see how that works with `Flask` prior to our MongoDB integration.

We will start with the `root` route since that is the essentially where any user will land when they visit our URL.

In your **app.py** file add the following code:

```python

@app.route("/", methods=["GET"])
def home_page():
    return "Congrats!  You made it to the home page!"
    
```

You can :book:[Read More](http://flask.palletsprojects.com/en/1.1.x/quickstart/#routing) about `Flask` routing for a full explaination about what is happening in the above code block.

The quick version is that we created a route using the `app.route()` method.  As we created that route we passed in two arguments.  The first argument is the name of the route as a `string` and the second is the [HTTP Request Method](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) that will trigger the code block below it, in our case the `home_page()` function.

We simply return some text in the [HTTP Response Body](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages#Body_2)

**Complete View of app.py**
```python
from flask import Flask, request, jsonify, render_template
import pymongo

app = Flask(__name__)
app.config["DEBUG"] = True


@app.route("/", methods=["GET"])
def home_page():
    return "Congrats!  You made it to the home page!"

app.run(port=35080)
```
:arrow_right: Note that the code we just added was between the DEBUG feature and the PORT specification.

**Save app.py and use Python to run it.  Once the server is running, navigate to `http://localhost:35080/` in your web browser, or use `POSTMAN` to send a `GET` request to that URL**

**Expected Output**
![image](https://user-images.githubusercontent.com/38021615/66260455-bd9dc700-e773-11e9-8db9-5bc4d0911aa1.png)

**Exercise 2:**
Okay, admittedly that wasn't the most useful API endpoint ever :man_shrugging:.  So let's see how we can use this same logic to render a static file, such as `index.html`.  

Instead of returning the `string` explicitly, when we return a file we can make real-time changes to that file without the need to restart the `Flask` server.

:bulb:  What are some scenarios that you can think of where server downtime can cause issues?  What are some negatives of real-time updates of static assets?

Inside of **app.py** make the following changes.  We will show you the complete code snippet all at once this time.

```python
from flask import Flask, request, jsonify, render_template
import pymongo

app = Flask(__name__)
app.config["DEBUG"] = True


@app.route("/", methods=["GET"])
def home_page():
    return render_template('index.html')

app.run(port=35080)
```

You can see the `Flask` method `render_template()` is used to send our `index.html` file in the HTTP Response Body this time.

Save your **app.py** file and run it using Python.  Once the server is running navigate to the endpoint, `localhost:35080/`, to see the changes to the endpoint.

**POSTMAN View**
![image](https://user-images.githubusercontent.com/38021615/66260568-fab68900-e774-11e9-957b-b4326fcdbb1e.png)

**Web Browser View**
![image](https://user-images.githubusercontent.com/38021615/66260577-191c8480-e775-11e9-888c-5634141a0bc6.png)

:warning: The link on this page won't work yet since we haven't configured it to be an endpoint. We will do so soon so wait for it!

**Bonus :rocket:** : Try making a change to the `index.html` page by adding a different welcome message (**Do Not Edit The Link On This Page**) and refresh your broswer without restarting the server to see the new content load.


**Exercise 3:**
Our `Endpoints` aren't limited to just sending content in an HTTP Response Body.  They can also do computational stuff.  Let's take an admittedly arbitrary look at an `Endpoint` which contains computational logic when it receives HTTP Requests.

We are also going to add two new routes to our **app.py** file at the same time.  Open **app.py** and add the following routes, again we will show you the complete file all at once.  We will make the following changes:
- Add two global variables `var1` and `var2` which are `integers`
- Add two new routes, one that points to `/addition` and the other pointing to `/multiplication`.  Each will contain different but relevant logic.

```python
from flask import Flask, request, jsonify, render_template
import pymongo

app = Flask(__name__)
app.config["DEBUG"] = True

var1 = 34
var2 = 91

@app.route("/", methods=["GET"])
def home_page():
    return render_template('index.html')

@app.route("/addition", methods=["GET"])
def add_stuff():
    sum = var1 + var2
    return str(sum)

@app.route("/multiplication", methods=["GET"])
def multiply_stuff():
    product = var1 * var2
    return jsonify(product)
    
app.run(port=35080)
```

Save your **app.py** file and run it with Python.  Once the server is running navigate to your two new routes and watch the :sparkles: happen.

In the above example we demonstrated the use of returning a `string` using `str()` as well as `JSON` using `jsonify()`.

:book: [Read More](http://flask.palletsprojects.com/en/1.1.x/quickstart/#apis-with-json) about using JSON with API's.

## Helper Functions

:warning: **Before we begin, remove your `/addition` and `/multiplication` routes, as well as the global variables `var1` and `var2` from your `app.py` file.  You can leave the `/` route as is.**

Now that we have an understanding of how routing is working let's create a few functions that will allow us to interact with MongoDB.  We are going to lean on [what you already know](../exercises/04_mongo-with-python.md) about using the `PyMongo` library, and transition that information into the logic for our endpoints.

Keep in mind that these **Helper Functions** may be better suited stored in a more modular fashion in a real-world scenario.  For our examples they are going to live in one file, **app.py**.  

These **Helper Functions** are in no way the only solution to accomplishing this task, so feel free to be creative if you understand what is happening, and share it with those around you!

**Exercise 1:**

To connect to MongoDB we have to establish a connection.  This will happen anytime we perform a CRUD operation, so I put it into a function to save a few lines of code.  

:bulb: You could also build a function that ensures the connection closes after each operation, and you should really take the time to do so for your own practice!

Edit your **app.py** file and add the `create_mongo_session()` helper function:

```python
def create_mongo_session(database, collection):
    client = pymongo.MongoClient('localhost', 27017)
    db = client[database]
    col = db[collection]
    return db, col
```

**Exercise 2:**
I have created a few HTML forms, to show how a POST request works.  These forms return data into Flask in a strange way.  They are called MultiDicts, and are essentially a list of tuples, that get treated like dictionaries... I know right :man_shrugging:

What we gain from this is the ability to have a single key with multiple values.  What I mean by that is consider you have two middle names, and your database uses 'middle-name' as a key.  This data type allows for you to correctly input that individual's middle names.

But becuase of that, we have to parse it out and create a dictionary that makes sense, or else we can't get the `PyMongo` library to play nice with it for CRUD operations.

Edit your **app.py** file and add the `parse_form()` helper function:

```python
def parse_form():
    x = {}
    d = request.form
    for key in d.keys():
        value = request.form.getlist(key)
        for val in value:
            x[key] = val
    return x
```

**Complete View of app.py**
```python
from flask import Flask, request, jsonify, render_template
import pymongo

app = Flask(__name__)
app.config["DEBUG"] = True


def create_mongo_session(database, collection):
    client = pymongo.MongoClient('localhost', 27017)
    db = client[database]
    col = db[collection]
    return db, col


def parse_form():
    x = {}
    d = request.form
    for key in d.keys():
        value = request.form.getlist(key)
        for val in value:
            x[key] = val
    return x


@app.route("/", methods=["GET"])
def home_page():
    return render_template('index.html')


app.run(port=35080)
```

## MongoDB Functions

Now we are going to set up our functions that will query our MongoDB.  Some of these functions, like `mongo_find(query)` are designed to be flexible with their query parameters.  These functions will do our heavy lifting by running the queries we need and doing any processing on the data before returning it to our route.

**Exercise 1:**
Building out our `mongo_find(query)` function.  Inside of **app.py** add this function:

```python
def mongo_find(query):
    _, col = create_mongo_session('apitest', 'v1')
    find_result = []
    for i in col.find(query):
        find_result.append(i)
    return str(find_result)
```
:bulb: As you can see, this function calls our `create_mongo_session(database, collection)` helper function.  It takes one `query` parameter and we will handle a blank parameter being passed in when we build the route.

**Exercise 2:**
Since the first exercise focuses on a **Read** operation so in this exercise we will build two **Update** operations.  

Edit **app.py** to add the `mongo_insert_one(doc)` and `mongo_insert_many(doc)` functions.

**mongo_insert_one(doc)**
```python
def mongo_insert_one(doc):
    #insert one document in the 'v1' collection of the 'apitest' database
    _, col = create_mongo_session('apitest', 'v1')
    col.insert_one(doc)
```

And

**mongo_insert_many(doc)**
```python
def mongo_insert_many(doc):
    #inserting many... just in case!
    db, col = create_mongo_session('somedb', 'somecol')
    col.insert_many(doc)
````

Okay, that was a lot of functions to add, let's go ahead and view our entire **app.py** file just to make sure everyone is on the right track.

**Complete View of app.py:**
```python

from flask import Flask, request, jsonify, render_template
import pymongo

app = Flask(__name__)
app.config["DEBUG"] = True


def create_mongo_session(database, collection):
    client = pymongo.MongoClient('localhost', 27017)
    db = client[database]
    col = db[collection]
    return db, col


def parse_form():
    x = {}
    d = request.form
    for key in d.keys():
        value = request.form.getlist(key)
        for val in value:
            x[key] = val
    return x


def mongo_find(query):
    _, col = create_mongo_session('apitest', 'v1')
    find_result = []
    for i in col.find(query):
        find_result.append(i)
    return str(find_result)


def mongo_insert_one(doc):
    _, col = create_mongo_session('apitest', 'v1')
    col.insert_one(doc)


def mongo_insert_many(doc):
    _, col = create_mongo_session('somedb', 'somecol')
    col.insert_many(doc)


@app.route("/", methods=["GET"])
def home_page():
    return render_template('index.html')


app.run(port=35080)
```

**Save app.py but understand that we have not made any changes that will be visible, we have only setup the functions we are going to use in our routes.  The next section implements our changes.**

You should also consider experimenting with SQL libraries and implementing helper functions like this for your API. You may not be using MongoDB, but the API concepts will remain the same regardless of your database. However, how you interact with that database will differ significantly.

Notice how things like authentication have been left out?!

That's one major reason setting this up in production is a very bad thing!!  You should consider security at every step along the way.  

There are also no request limits, consider what happens if someone makes 90 million requests at once... this is called a denial of service attack, and can render your resources inaccessible.  If you are running on a cloud provider's platform, you will most likely be paying per request for things like this... and that bill gets expensive fast, so consider everything!

## Tying It All Together

Phew... at this point we have an understanding of `Endpoints`, `PyMongo` and `Flask`.  That's a lot to cover in such a short time, but the good news is that now all that is left is tying things together and we will have a fully functional web service!

**Exercise1:**
A web service in Flask is defined by routes.  Routes is just a fancy word for URL.  Our routes will point to Python logic, and when we browse to them, something will happen to our MongoDB.

We already have our first route in our **app.py** file, its our `/` route.  Let's add another!

The /api/v1 route uses flask to render another template from a static HTML file.  This is where our API starts however.  Somtimes you will see public APIs have a `/api/v1/docs` route that contains instructions for using their API. This route is where I plan to put the instructions for our API.

It's basic, so there is nothing to really see here when it comes to what happens at this route, but we will add it anyway just to get familiar with this practice.

Edit **app.py** to include the new route.

```python
@app.route('/api/v1', methods=['GET'])
def api_root():
    return render_template('docs.html')
```

:warning: These routes depened on the `HTML` template files mentioned at the beginning of this document.  If these routes don't load, make sure you have those files in your project directory!

**Exercise 2:**
Let's start using some of our helper functions at these routes.  The first route will be the simplest one to implement logic for. 

We want a route that will return to us, ALL of the data in a given collection from a specific database.

If you think back to our helper function (hint... go look :wink:) you will know which collection this is.

In our case, the result of `mongo_find()` gets returned in the HTTP response when a HTTP GET request is made at this route.

No `GET` request, no function.  However this route is only accepting of a `GET` request.  If we tried to use a `POST`, `UPDATE`, or `DELETE` we would see an error saying the method is not allowed.

:arrow_right:  Refer to the MongoDB documentation if you don't understand why `{}` was the filter arguement for our `mongo_find` function.

Edit **app.py** to accept our `/find/all` route:

```python
@app.route('/api/v1/mongo/find/all', methods=['GET'])
def api_mongo_find_all():
    return mongo_find({})
```

**Exercise 3:**
The next route implements even more logic.  It can look at whether or not the request at the route is a `GET` or a `POST`, based on that information it determines which block of code to run.

In our example, if the request is `GET`, then it will return a `Flask` method to render a form written in HTML.

If the request is a `POST`, it calls some of our helper functions to extract the data from the form and then run the query based on the form information.

`POST` is necessary here becuase the action our form makes, located in the `query.html` file, is a `POST` when the submit button is pressed.

Add this route to your **app.py** file.

```python
@app.route('/api/v1/mongo/find', methods=['GET', 'POST'])
def api_mongo_find():
    if request.method == 'GET':
        return render_template('query.html')
    elif request.method == 'POST':
        data = parse_form()
        return mongo_find(data)
```

**Exercise 4:**
Before we add the next route to our **app.py** file we need to change our `import` statment to include a few more methods.  Edit your `from flask import Flask, request, jsonify, render_template` line to look like this:

```python
from flask import Flask, request, jsonify, render_template, url_for, redirect
```

:arrow_right:  The two changes are `url_for` and `redirect`.

Now we will add a route to insert data, located in an `HTML` form into our MongoDB.  This route has a little extra :fire: to it as it also redirects you back to the main page after you submit the form.

Add this route to your **app.py**

```python
@app.route('/api/v1/mongo/insert', methods=['GET', 'POST'])
def api_mongo_insert():
    if request.method == 'GET':
        return render_template('mongoinsert.html')
    elif request.method == 'POST':
        data = parse_form()
        mongo_insert_one(data)
        return redirect(url_for('api_root'))
```

:bulb: You may have noticed that this route handles both `GET` and `POST` requests.  Can you think of when or how each type of request would be sent?

**Complete View of app.py**

```python
from flask import Flask, request, jsonify, render_template, url_for, redirect
import pymongo

app = Flask(__name__)
app.config["DEBUG"] = True


def create_mongo_session(database, collection):
    client = pymongo.MongoClient('localhost', 27017)
    db = client[database]
    col = db[collection]
    return db, col


def parse_form():
    x = {}
    d = request.form
    for key in d.keys():
        value = request.form.getlist(key)
        for val in value:
            x[key] = val
    return x


def mongo_find(query):
    _, col = create_mongo_session('apitest', 'v1')
    find_result = []
    for i in col.find(query):
        find_result.append(i)
    return str(find_result)


def mongo_insert_one(doc):
    _, col = create_mongo_session('apitest', 'v1')
    col.insert_one(doc)


def mongo_insert_many(doc):
    _, col = create_mongo_session('somedb', 'somecol')
    col.insert_many(doc)


@app.route("/", methods=["GET"])
def home_page():
    return render_template('index.html')


@app.route('/api/v1', methods=['GET'])
def api_root():
    return render_template('docs.html')


@app.route('/api/v1/mongo/find/all', methods=['GET'])
def api_mongo_find_all():
    return mongo_find({})


@app.route('/api/v1/mongo/find', methods=['GET', 'POST'])
def api_mongo_find():
    if request.method == 'GET':
        return render_template('query.html')
    elif request.method == 'POST':
        data = parse_form()
        return mongo_find(data)


@app.route('/api/v1/mongo/insert', methods=['GET', 'POST'])
def api_mongo_insert():
    if request.method == 'GET':
        return render_template('mongoinsert.html')
    elif request.method == 'POST':

        data = parse_form()
        mongo_insert_one(data)

        return redirect(url_for('api_root'))


app.run(port=35080)
```

Restart your `Flask` server and explore your routes!

Navigate to `http://localhost:35080/` to get started, the link on this page will now work... **Follow It!**

## Using Our Web Service

- Navigate with your browser or use `POSTMAN` to send a `GET` request to `http://localhost:35080/` and clik the link to **View Documentation** about your API
- Insert a person into your database by filling out this form, once you click submit you will be redirected to the `/` route
- Navigate with your browser or use `POSTMAN` to send a `GET` request to `http://localhost:35080/api/v1/mongo/find/all`.  You should see ALL of the documents in your target collection returned to you.  (At this stage you should have 1 document, revisit your `insert` route and add a few more for full effect)
- Use your web browser to navigate to the `http://localhost:35080/api/v1/mongo/find` route and enter a query parameter for one of the documents in your database (note, as it stands right now we can only query for `First Names`)


## Group Activities
During this time the class should split into :five: groups to complete each section.

**Activity 1: :alarm_clock: 15 minutes** 

Challenge yourself to go ahead and build the rest of the **CRUD** operations into this api to see what you can come up with!

**Activity 2: :alarm_clock: 15 minutes**

Right now, our query parameter only uses `First Names`. Expand that functionality by querying on other fields present in the collection. After all, in today's :earth_asia:, hardly anyone remembers first names anyway!

**Presentation: :alarm_clock: 25 minutes**
Present your results from **Group Activities** and answer any questions your classmates may have.

