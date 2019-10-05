# Python Webservice with Mongo and Flask

## Components

- **Flask:** is the most popular Python web application framework.  This is the Python library that is going to power our endpoints when we make a http request.
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
4. Copy and paste the following code into it's respective files:

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
Next, we need to create our initial Flask instance and set it's `DEBUG` features to give us helpful information in the event that things go wrong:
```python
app = Flask(__name__)
app.config["DEBUG"] = True

```

:bulb: If you are using a single module (as in this example), you should use `__name__` because depending on if it’s started as application or imported as module the name will be different (`__main__` versus the actual import name).

Lastly, let's check to see if we can start our `Flask` application.  We are going to tell our app to run and add a `PORT` for our applicaiton to listen on:
**app.py**
```python
app.run(port=35080)
```
Using Python run **app.py** and observe the terminal.  You should see output similar to that below, indicating that flask is running on `localhost:35080`

![image](https://user-images.githubusercontent.com/38021615/66258917-ad7bec80-e75f-11e9-8ec1-a5ebc2c165c6.png)

Press `Ctrl+C` to stop the stop the server and head back to your **app.py** file to start building endpoints.

## API Endpoints

`Endpoints`, also known as `Routes`, are locations on our server thatn have content which is meant to be accessed.  Access to this content can take place through a web browser such as *Google Chrome** or **Safari**, programatcially by processing `HTTP` requests or using a utility like `POSTMAN` to query the resources.
We navigate to endpoints on a regular basis as we use the internet.  You're doing so currently as you view this file.  We can view the specific `Endpoint` or `Route` our currently access resource is at by examining the URL in the address bar of our browser.
Consider what happens when you visit [Facebook](https://www.facebook.com), you type `www.facebook.com` into your browser and this usually takes you to the `root` endpoint which is denoted with a `/` character.

What if I want to **create a page** for a Band using Facebook?  Kindly Facebook provides us an `Endpoint` to reach the resources necessary to do so:
[create a page](https://www.facebook.com/pages/creation/?ref_type=registration_form) uses the `Endpoint` of `/pages/creation`.  This means we started at the root, `/`, then from there we found a location called `pages/` which is some sort of directory, and within it we found `creation` which could be a static file or some kind of functionality that Facebook provides.
