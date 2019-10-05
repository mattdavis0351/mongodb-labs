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
          <h3>make a GET request to the following endpoint to see the API in action:</h3>
          <p>/api/v1/mongo/find/all</p>
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
          <form action="http://localhost/api/v1/mdbinsert" method="POST">
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
          <form action="http://localhost/api/v1/mdbfind" method="POST">
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
We navigate to endpoints on a regular basis as we use the internet.  You're doing so currently as you view this file.  We can view the specific `Endpoint` or `Route` our currently access resource is at by examining the URL in the address bar of our browser.
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
