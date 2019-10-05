# Importing data into MongoDB

## Your Datasets

To make life easy on you, we have built a table to show you the different kinds of data you have available to you.  Use the appropriate method for each dataset when importing.

|File Name|File Type|
|---|---|
|Titanic.json|Regular JSON File|
|movieDetails.json|Regular JSON File|
|100YWeather.json|Regular JSON File|
|Citibike.json|Regular JSON File|
|Retail.json|Regular JSON File|
|Shipwrecks.json|Regular JSON File|
|AirRoutes.json|Regular JSON File|
|Orders.json|Regular JSON File|
|pokemon-cards.json|JSON Array|
|Player.csv|CSV File|

## 1. Navigate to the folder containing all datasets

Right click on the windows button (in the bottom left corner) and click the non-admin 'Command Prompt'. 

To navigate to the folder containing all the datasets, type:

`cd C:\Users\associate\Documents` and hit 'Enter'.

You should now be in the Documents folder containing all your data files. 

## 2. Import the datasets

We're going to import a variety of file types into MongoDB using the `mongoimport` utility. 

[Read more](https://docs.mongodb.com/manual/reference/program/mongoimport/#bin.mongoimport) :book: about **mongoimport**. 

### JSON

JSON data comprises multiple JSON objects. Here's an example of what it looks like:
```JSON
{
    "city": "Chennai",
    "state": "Tamil Nadu",
    "population_in_millions": 7.08
},
{
    "city": "Mumbai",
    "state": "Maharashtra",
    "population_in_millions": 18.41,
    
},
{
    "city": "New Delhi",
    "population_in_millions": 21.75
}
```

**Importing JSON Data:**
```javascript
mongoimport --db <my_db_name> --collection <my_collection_name> --file <my_file_name.json>
```

In the query above, the following options have been used:

- **--db**: specifies the name of the database on which to run `monogimport`. If that database does not exist, it will be created.
- **--collection**: specifies the name of the collection.
- **--file**: specifies the location and name of the file to be imported. If you have already navigated to the directory containing the data file, simply typing the file name will suffice, as in this case. 

**result:** (On successful run, your result will look something like this.)
```
2019-10-03T09:07:53.000-0700	connected to: localhost
2019-10-03T09:07:53.123-0700	imported 891 documents
```

### JSON array

A JSON array is when multiple JSON objects are enclosed between a single array`[]` as follows:

```JSON
[
    {
        "city": "Chennai",
        "state": "Tamil Nadu",
        "population_in_millions": 7.08
    },
    {
        "city": "Mumbai",
        "state": "Maharashtra",
        "population_in_millions": 18.41,
    
    },
    {
        "city": "New Delhi",
        "population_in_millions": 21.75
    }
]
```

**Importing a JSON Array:**
```javascript
mongoimport --db <my_db_name> --collection <my_collection_name> --file <my_file_name.json> --jsonArray
```

The syntax for importing JSON arrays is similar to importing JSON objects with the addition of one option:

- **--jsonArray**: specifies that the file is a JSON array.

### CSV files

**C**omma **S**eparated **V**alue files are flat files that store tabular data as plain text. Most commonly, each data point is separated by commas, which is where it gets its name. Other delimiters like semicolons can also be used. 

**Importing CSV Files:**
```javascript
mongoimport --db <my_db_name> --collection <my_collection_name> --file <my_file_name.csv> --type csv --headerline
```

In addition to the options you've already come across, the query above contains:

- **--type**: specifies the file type to import. Can be `csv` or `tsv`. The **default value is JSON** hence we didn't use this flag when importing JSON data earlier. 
- **--headerline**: indicates that the first line of the file is the header line. If this flag is not used, `mongoimport` will import the first line of the file as a separate document. This flag only works with `csv` and `tsv` file types. If used with JSON, an errror will be thrown. 
- **--fields**: specifiy a comma separated list of field names, in cases where the data file being imported does not have a header line in the first line. This flag only works with `csv` and `tsv` file types. If used with JSON, an errror will be thrown. We didn't need to use this flag in our query since our `csv` files have a header line as the first line.

## 3. Open the Mongo Shell

On your desktop, double click the Mongo shortcut and you'll be brought straight to the MongoDB shell. This is where you'll write the queries against the various databases and collections you just imported. Happy querying! :smiley:

:warning: `mongoimport` will throw an error from within the Mongo shell, even if your syntax is correct becuase it is a **command line** utitlity and not part of the MongoDB query language. **Therefore, remember to always run the `mongoimport` function from the Command Prompt.**


