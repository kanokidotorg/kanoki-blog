---
title: "Building a Web app using Python and Mongodb"
date: "2019-09-25"
categories: [ Data Science, Flask, Mongodb, Python, Python, Data Science, Tutorial ]
tags: [ Flask, leafletjs, Mongodb, Python, Tutorial ]
---

## **Introduction**

While working as a Data Scientist many a times we have to show a visual application which demonstrates how the model or data that we are using really works using web apps. In this blog I want to show how can we quickly build an app using Python micro framework flask and Mongodb

In this blog we will see how to develop a restaurant finder app using Flask, Mongo DB and LeafletjS. This application will take inputs like Restaurant Name, City or Zip-code and radius from the user and gets all the nearby restaurants matching the Query within the specified radius

We will use pymongo to connect to the mongodb using python and a flask backend to process the query and get the results from the mongodb and leafletjs to plot the nearby restaurant results on the map

## **How it Works?**

Here is an example to demonstrate how this restaurant finder application works:

**Query**: Restaurant Name: Kentucky / Zipcode: 10451 / Radius: 5 miles

![python mongodb tutorial](/images/2019/09/image-61.png)

**Result:**

Here is the result of all the restaurants within 5 miles of radius from zipcode 10451 and name contains the substring kentucky. The restaurants are shown as marker on the map and when you click on this map it shows the restaurant name as pop-up

![python mongodb tutorial](/images/2019/09/image-70.png)

## **Steps to Follow:**

1. Install Mongodb, Pymongo and Flask
2. Upload Restaurant data into Mongodb
3. Create a Python Flask app with Get method for Querying the Mongodb
4. Create a html file for user to input the Query parameters and to display the nearby restaurants on the map
5. Create a JS file for the AJAX call to the Flask App

## **Python Mongodb connection**

**MongoDB**:

You can just follow their [official documentation](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/) for installing it on mac/linux and windows.

#### **Starting the mongo server:**

I'm using a windows 10 machine and after mongodb installation go to following path on a terminal and start the server by typing mongod.exe

![](/images/2019/09/image-64.png)

#### **Create Mongo Db and Collection**

Use mongodb compass, which is a GUI tool for creating db's and collections and helps to run the queries and visually exploring your data

#### **Connect to mongodb using compass**

![Mongodb compass](/images/2019/09/image-65.png)

#### **Create Database and Collection**

![Mongodb compass create collection](/images/2019/09/image-67.png)

#### **Import Restaurant Data to Mongodb**

Go to Collection > Import Data and select the JSON file saved on your disk.

Download Data: [https://raw.githubusercontent.com/mongodb/docs-assets/geospatial/restaurants.json](https://raw.githubusercontent.com/mongodb/docs-assets/geospatial/restaurants.json)

![Mongodb compass import data](/images/2019/09/image-68.png)

#### **View Data from Mongo Collection**

![](/images/2019/09/image-69.png)

#### **Install pymongo and flask**

```
pip install pymongo
```

```
pip install flask
```

## **Python and Mongodb Query using Pymongo**

Before going ahead, we need to understand how the mongodb spatial Query works. Spatial Queries like [`$geoWithin`](https://docs.mongodb.com/manual/reference/operator/query/geoWithin/#op._S_geoWithin), and [`$nearSphere`](https://docs.mongodb.com/manual/reference/operator/query/nearSphere/#op._S_nearSphere) searches all the documents in mongodb collection to find the matching result within a specified radius of the origin lat and long.

Here, the query contains the zipcode 10451 and using geopy we will get the lat long of this zipcode and using this origin lat, long we will find all the documents which are in spherical distance of 5 miles from zipcode 10451

Let's understand how this entire application works:

**User enters the Query through the application**

Kentucky / Zipcode: 10451 / Radius: 5 miles

**Get the Coordinates i.e. latitude and longitude of the Query city**

Using [Geopy](https://geopy.readthedocs.io/en/stable/) get the lat and long using the below python code

```
from geopy.geocoders import Nominatim
zip_or_addr="10451"
geolocator = Nominatim(user_agent='myapplication')
location = geolocator.geocode(zip_or_addr)
lat=location.raw['lat']
lon=location.raw['lon']
```

**Connect to Mongodb using pymongo**

We will connect to mongodb using pymongo and query the mongodb using the query filters entered by the user

```
import pymongo
myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["flask_tutorial"]
mycol = mydb["restaurants"]
```

**Query the Mongodb collection**

Here the search string is kentucky and radius is 5 miles and the origin city(zipcode 10451) lat, long is derived above using geopy. Now mongodb spatial queries [`$geoWithin`](https://docs.mongodb.com/manual/reference/operator/query/geoWithin/#op._S_geoWithin) and [`$nearSphere`](https://docs.mongodb.com/manual/reference/operator/query/nearSphere/#op._S_nearSphere). The difference between two query is that former gives an unsorted result and latter gives sorted result and [`$maxDistance`](https://docs.mongodb.com/manual/reference/operator/query/maxDistance/#op._S_maxDistance) as an additional parameter to define the radius within which the documents has to be searched

```
srch = 'kentucky'
rad = 5
METERS_PER_MILE = 1609.34

cursor = mycol.find(
{ 'location': { '$nearSphere': { '$geometry': { 'type': "Point",
                                 'coordinates': [ float(lon), float(lat) ] },
                                '$maxDistance': rad * METERS_PER_MILE } },
 'name': {'$regex': srch, "$options" : "i"} },
)
```

**Result**

Result is a JSON object that contains all the restaurants having a substring kentucky in their name and which are located within a radius of 5 miles of zipcode 10451

```
{'_id': ObjectId('55cba2476c522cafdb0574cd'), 'location': {'coordinates': [-73.9365102, 40.8202205], 'type': 'Point'}, 'name': 'Kentucky Fried Chicken'}
{'_id': ObjectId('55cba2476c522cafdb0574c3'), 'location': {'coordinates': [-73.94509699999999, 40.791549], 'type': 'Point'}, 'name': 'Kentucky Fried Chicken'}
{'_id': ObjectId('55cba2476c522cafdb058bab'), 'location': {'coordinates': [-73.9682766, 40.8013318], 'type': 'Point'}, 'name': 'Kentucky Fried Chicken'}
{'_id': ObjectId('55cba2476c522cafdb058988'), 'location': {'coordinates': [-73.90446299999999, 40.8799332], 'type': 'Point'}, 'name': 'Kentucky Fried Chicken'}
{'_id': ObjectId('55cba2476c522cafdb057088'), 'location': {'coordinates': [-73.8540964, 40.8737543], 'type': 'Point'}, 'name': 'Kentucky Fried Chicken'}
{'_id': ObjectId('55cba2476c522cafdb056c36'), 'location': {'coordinates': [-73.9160315, 40.7629446], 'type': 'Point'}, 'name': 'Kentucky Fried Chicken'}
```

## **leafletjs plotting**

[leafletjs](https://leafletjs.com/) is an open source javascript library, which is very simple and easy to use library for plotting the data on map and visualizing it. It internally uses the open street maps under the hood

Now using the javascript we will retrieve the lat long and restaurants names from all the results JSON and plot it using leafletjs

**HTML**

This html file(./templates/index.html) uses bootstrap 4 just to make it work and support for all device types. There are five text boxes for each of the input field and a submit button in the first column of the page and the secod column contains a leaflet map to show the results

**Javascript**

We have written a small JS file (flask_tuts.js) which is used to get the Query filters from the UI and trigger the AJAX query to send those filters as a parameter to the flask app which then Queries the mongodb and sends back the result to Javascript and after filtering the result it plots the matching nearby restaurant markers on the map using leafletjs

## **Flask App**

We will have one python flask file(\_\_init\_\_.py) with two routes. The route '/' will show a page with blank fields for Restaurant Name, City, State, Zipcode and Radius. Another route /api/v1.0/tasks/autoc2/restaurantfinder is a Get method which is invoked by the Javascript ajax function and it takes the following parameters from the UI: Restaurant Name, City, State, Zipcode and Radius and follow the steps defined in the Pymongo Query section above to return the JSON data for all the nearby restaurants.

## **Github Link:**

You can get the complete code on this github link

[https://github.com/min2bro/Flask\_Tuts](https://github.com/min2bro/Flask_Tuts)
