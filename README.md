# Turning food bank data into a D3 map
We'll take a look at the [food bank data](https://github.com/maptimeLA/food_bank_data_bank) and see what can be mapped using D3.

Our main goal will be to calculate and show the total number of food pantries in a given L.A. neighborhood.

You'll need some knowledge of the following:

- QGIS
- HTML
- CSS
- JavaScript

## Step 1 - Join two GIS files 
Let's start by grabbing the the [FoodPantriesMaster file](https://github.com/maptimeLA/food_bank_data_bank/blob/master/datasets/FoodSources/FoodPantries/FoodPantriesMaster.geojson) in the food bank data repo.

![Food pantries](http://www.jschleuss.com/wp-content/uploads/2015/09/food-pantries1.png)

In QGIS, each dot represents a food pantry location. The pantry's location includes agency information and an address. But since it's a GIS file it already has the latitude and longitude for each location. We can use this to do a count of the number of pantries in a particular L.A. neighborhood.

![LA County neighborhoods](http://www.jschleuss.com/wp-content/uploads/2015/09/lacounty-hoods.png)
We'll need a shapefile with polygons of neighborhoods to do this. Luckily, these shapes are available for free at the Los Angeles Times' [boundaries site](http://boundaries.latimes.com/sets/). 

I've already removed neighborhoods from Orange County and saved both files with NAD83 projection (EPSG:4269). They can be found in step1/start.

Open both shape files in QGIS and select Vector > Analysis Tools > Points in Polygon

QGIS should figure out which file has polygons and which has points. We'll need to set an output name and find a place to save it. 

![Step1](http://www.jschleuss.com/wp-content/uploads/2015/09/step1.png)

Hit 'OK' and now you've got a file that has the total number of pantries in each neighborhood polygon.

## Step 2 - Simplify the shapefile
If we were to output this file as a geojson for the Web map we'd get a 1.3 megabyte file. That's a bit large. 

So, take the file into [mapshapper](http://mapshaper.org/) and simplify it. You'll need to use Chrome. Compress your shapefiles (or save it as GeoJSON using QGIS). Then upload it to mapshaper. 

Select "Simplify" in the top-right corner. Then make sure "prevent shape removal" is checked. And "Visvalingam/weighted area" should be fine for this project. Now, drag the slider at the top. 

![Slider mapshaper](http://www.jschleuss.com/wp-content/uploads/2015/09/slider.png)

I'd go to about 19% or so and then export as geojson.

## Step 3 - Get a basic map in D3
Now let's get it into D3.

You'll need to be running a server locally using Python.

2. `cd d3-foodpantries`
3. `python -m SimpleHTTPServer 8000`
4. Go to your browser and open `http://localhost:8000/`
5. To open on your phone, find your IP address and open `http://[YOUR-IP-ADDRESS]:8000/`



## Step 4 - Add colors based on data in GeoJSON file
We can use D3 to color the map based on the data in the file.

## Step 5 - Add interactivity with tooltip
Here's some code to get us a tooltip.
