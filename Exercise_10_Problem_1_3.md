## Problem 1: Geocode shopping centers

The aim of problems 1-3 is to find out **how many people live within a walking distance (1.5 km) from selected shopping centers in Tokyo**.

In problem 1 the task is to find out the addresses for a list of shopping centers and to geocode these addresses in order to represent them as points. The output should be stored in a Shapefile called `shopping_centers.shp` 

**Preparation:** Find out the addresses for following shopping centers from the internet, and write the addresses into a text file called `shopping_centers.txt`:

- Tokyo Department Store
- Seibu Shibuya Store
- National Azabu

`shopping_centers.txt` should have semicolon (`;`) as a separator, and the file should include the following columns:

- ``id`` (integer) containing an unique identifier for each shopping center
- ``name`` (string) of each shopping center
- ``addr`` (string) the address 


See and example of how to format the text file [in the lesson 3 materials](https://autogis-site.readthedocs.io/en/latest/notebooks/L3/geocoding_in_geopandas.html). Save (and upload) the text file into your exercise repository.

- Read `shopping_centers.txt` that you just created into a pandas DataFrame called ``data``:


```python
# Import modules
import geopandas as gpd
import pandas as pd
# Read the data (replace "None" with your own code)
data = None
# YOUR CODE HERE 1 to read the data
```


```python
# TEST CODE
# Check your input data
print(data)
```

- Geocode the addresses using the Nominatim geocoding service. Store the output in a variable called `geo`:


```python
# Geocode the addresses using Nominatim
geo = None
from geopandas.tools import geocode

# Geocode addresses using Nominatim. Remember to provide a custom "application name" in the user_agent parameter!
# YOUR CODE HERE 2 for geocoding
```


```python
# TEST CODE
# Check the geocoded output
print(geo)
```


```python
# TEST CODE
# Check the data type (should be a GeoDataFrame!)
print(type(geo))
```

Check that the coordinate reference system of the geocoded result is correctly defined, and **reproject the layer into JGD2011** (EPSG:6668):


```python
# YOUR CODE HERE 3 to set crs.
```


```python
#TEST CODE
# Check layer crs
print(geo.crs)
```

- Make a table join between the geocoded addresses (``geo``) and the original addresses (``data``) in order to link the numerical coordinates and  the `id` and `name` of each shopping center. 
- Store the output in a variable called ``geodata`` 



```python
# YOUR CODE HERE 4 to join the tables
geodata = None
```


```python
#TEST CODE
# Check the join output
print(geodata.head())
```

- Save the output as a Shapefile called `shopping_centers.shp` 


```python
# Define output filepath
out_fp = None
#  YOUR CODE HERE 5 to save the output 
```

```python
#TEST CODE
# Print info about output file
print("Geocoded output is stored in this file:", out_fp)
```

## Problem 2: Create buffers around shopping centers

Let's continue with our case study and calculate a 1.5 km buffer around the geocoded points. 


- Start by creating a new column called `buffer` to ``geodata`` GeoDataFrame:


```python
# YOUR CODE HERE 6 to create a new column
```

- Calculate a 1.5 km buffer for each geocoded point. Store the buffer geometry in the new `buffer` column.

Here, you can use the [GeoDataFrame buffer() method](http://geopandas.org/geometric_manipulations.html#GeoSeries.buffer), which uses Shapely's [buffer](http://toblerity.org/shapely/manual.html#object.buffer) in the bacground. You only need to use the `distance` -parameter, don't worry about the other parameters.

Before using buffer() method, convert the crs to a projected crs in meters, for example EPSG:32634. Then the buffer distance will be in meters.


```python
# YOUR CODE HERE 7 to set buffer column
```


```python
#TEST CODE
print(geodata.head())
```


```python
#TEST CODE
# Check the data type of the first value in the buffer-column
print(type(geodata.at[0,'buffer']))
```


```python
#TEST CODE
# Check the areas of your buffers in km^2
print(round(gpd.GeoSeries(geodata["buffer"]).area / 1000000))
```

- Replace the values in `geometry` column with the values of `buffer` column:


```python
# YOUR CODE HERE 8 to replace the values in geometry
```


```python
#NON-EDITABLE TEST CELL
print(geodata.head())
```




```python

```

## Problem 3: How many people live near shopping centers? 

Last step in our analysis is to make a spatial join between our buffer layer and population data in order to find out **how many people live near each shopping center**. 

We will use the data which can be downloaded from the [webpage](https://nlftp.mlit.go.jp/ksj/gml/datalist/KsjTmplt-mesh500h30.html#prefecture13) 

https://nlftp.mlit.go.jp/ksj/gml/datalist/KsjTmplt-mesh500h30.html#prefecture13

named 500m_mesh_suikei_2018_shape_13.zip. This is the population data in Tokyo. This zip file contains 

- 500m_mesh_2018_13.dbf,
- 500m_mesh_2018_13.prj,
- 500m_mesh_2018_13.shp and 
- 500m_mesh_2018_13.shx.

The coordinate reference system of the population grid is **(EPSG:4612)**.


**Steps:**

- Read the population shape file (500m_mesh_2018_13.shp) into a geodataframe

- Select only the useful columns from the population grid: ``'PTN_2020'`` (=population count per mesh) and ``'geometry'`` 

- Make a spatial join between your buffered point layer and population grid layer. Join the information now from buffer layer **into the population grid layer**. Use sjoin() for spatial join with the paramter `how="inner", op="intersects"`

- Group the joined layer by shopping center index

- Calculate the sum of population living within 1.5 km for each shopping center.

**Finally:**

- Print out the population living within 1.5 km from each shopping center:

    - Tokyo Department Store
    - Seibu Shibuya Store
    - National Azabu
     
**Final print out should contain both the shopping center name and population count**, for example: `25858 people live within 1.5 km from Tokyo Department Store`. In order to find the number of people in integer, round to the nearest whole number.


```python
# YOUR CODE HERE 9 
# Read population grid data for 2018 into a variable `pop`. 
# Remember to check the crs info! 
pop = None
```


```python
#NON-EDITABLE TEST CODE
# Check your input data
print("Number of rows:", len(pop))
print(pop.head(3))
```


```python
# Create a spatial join between grid layer and buffer layer. 
# YOUR CODE HERE 10 for spatial join
```


```python
#YOUR CODE HERE 11 to report how many people live within 1.5 km distance from each shopping center
```

**Reflections:**
    
- How challenging did you find problems 1-3 (on scale to 1-5), and why?
- What was easy?
- What was difficult?

YOUR ANSWER HERE

Well done!
