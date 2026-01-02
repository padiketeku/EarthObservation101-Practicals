# Activity 12: Working with LiDAR Data



## Introduction

LiDAR is an acronym for light detection and ranging, pulses of light (usually NIR) are sent down to the target and the intensity and distance (range) of the return pulse are recorded. The pulses occur as point clouds that provide a 3D structure of the target. The lidar returns can be extracted to provide the characteristics of the terrain, surface materials, and thus height of vertical objects. In this practical, you will work through lidar data products with the aim of evaluating the elevation of Australia and determining the height of vertical objects using data from England. 



## Learning Outcomes

At the end of the activity, you should be able to

- Characterise the elevation of Australia

- Compute canopy height model

- Interpret lidar data



## Activity Begins


In this activity we explored LiDAR products describing the terrain and surface objects of Australia and England. The following are the data sets used: <br>

1, Australia data. "AU/GA/AUSTRALIA_5M_DEM". This is an image collection <br>

2, England data.  “UK/EA/ENGLAND_1M_TERRAIN/2022” . This is an image.

Follow through the following work flow to complete the activity.


### Australian LiDAR data


 1, Type ‘Australia’ into the GEE search bar to find the Australian data (i.e, **"AU/GA/AUSTRALIA_5M_DEM"**). Import the data into your Code Editor. Rename the collection to **aus**.

 2, Print the collection to the Console and explore the metadata.

How many images in the collection. Describe bands of each image, including the coordinate reference system

```Javascript
print(aus)
```


3,  Retrieve the image for NT using **ee.Image()**

```JavaScript
var eleNT = ee.Image("AU/GA/AUSTRALIA_5M_DEM/NT")
print (eleNT)
```

4, Visualise the elevation

```JavaScript
Map.addLayer(eleNT, {}, 'elevation NT')
```



#### Evaluate data


5, Use the Inspector tool to inspect the data range and specify a colour palette to create a pseudo-colour display

6, Visualise the data again specify the range of values and colour palette

```JavaScript
Map.addLayer(eleNT, {min:5, max:20, palette:['red', 'yellow', 'brown']})
```

7, Visualise the NSW data, too.

```JavaScript

//retrieve the NSW layer
var eleNSW = ee.Image("AU/GA/AUSTRALIA_5M_DEM/NSW")

//visualise the NSW data
Map.addLayer(eleNSW , {min:500, max:1000, palette:['red', 'yellow', 'brown']})
```




### England LiDAR data



8, Type ‘England’ into the GEE search bar to find the England data. Import the data into Code Editor and rename the variable **uk**

Print the variable to the Console.

```JavaScript
'print(uk, 'UK')
```

Visualise the England data.

```JavaScript
Map.addLayer(uk, {bands:['dtm', 'dsm_first', 'dsm_last'], min:100, max:700}, 'UK')
```


#### Canopy height model

The LiDAR returns can be turned into two important products namely digital surface model (DSM) and digital terrain model (DTM). The DSM encompasses all natural and artificial surfaces the LiDAR pulse interacted with on its round trip, while the DTM is the 3D of the LiDAR return from the bareground. Canopy height model (CHM) is obtained by subtracting the DTM from the DSM. The CHM helps discriminate between vertical and flat objects.

 ```JavaScript

//canopy height model = dsm minus dtm
var dsm = uk.select('dsm_first')
var dtm = uk.select('dtm')

// thus CHM is
var chm = dsm.subtract(dtm). rename('chm')

//9, visualise chm. it's a greyscale with height objects being white

Map.addLayer (chm, {}, 'CHM')
```


10, Turn on the 'Satellite' base layer to verify tree canopy and urban areas



11, Using the base satellite layer, identify the following surface types <br>

- water <br>

- tree vegetation areas <br>

- urban areas <br>




12, Describe the CHM for the 3 surface types above. Did the CHM difference you observed between surface types make sense? hint- use the Inspector tool



##### Identify vertical surface types using pseudocolour visualisation

13,  pseudocolour display is done to identify or discriminate vertical surface types

```JavaScript
Map.addLayer(chm, {min:0, max:12, palette:['black','cyan', 'yellow', 'purple']}, 'CHM-Pseudo')
```


The result image is shown below. 





![image](https://github.com/user-attachments/assets/336bf23f-781a-490d-8890-49e77ef4e264)





## DIY


Given the result, shown in the figure above, discern between tall and flat objects.



## Conclusion


This practical task explained an application of lidar data in that a canopy height model was computed and visualised.























