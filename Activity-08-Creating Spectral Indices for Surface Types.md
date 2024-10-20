# Activity 8: Creating Spectral Indices 
In this activity, you will explore Landsat 8 and Sentinel-2 imagey to consolidate your knowledge on spectral indices. A range of spectral indices can be computed from the imagery, but in this activity the frequently used indices, such as the normalised difference vegetation index (NDVI), normalised difference water index (NDWI), normalized difference moisture index (NDMI), enhanced vegetation index (ENVI), soil adjusted vegetation index (SAVI), and normalised burn ratio (NBR) were explored. 

**NDVI** = (NIR – RED) / (NIR + RED) <br>

**NDWI** = (GREEN – NIR) / (GREEN + NIR) measures open water bodies, including flooded surfaces and turbidity. <br>

**NDMI** = (NIR-SWIR) / (NIR + SWIR) measures water content in vegetation canopy <br>

**SAVI** = ((NIR - Red) / (NIR + Red + L)) x (1 + L) measures impact of the background soil line, particularly useful for areas with sparse vegetation with open bare land. L, correction factor, ranges from -1 to 1, is dependent on vegetation density. L is large for low vegetation density and low for high vegetation density. <br>

**EVI** = 2.5 * ((NIR – RED) / ((NIR) + (C1 * RED) – (C2 * BLUE) + L)) proposed to account for atmospheric and soil effects. EVI also mitigates saturation, a major challenge for NDVI when vegetation is dense.
EVI is usually associated with MODIS, but can be derived from other satellite imagery, too. The EVI parameters for MODIS are: C1=6, C2=7.5, and L=1
EVI is normalised to –1 to 1, it varies between 0.2 and 0.8 for healthy vegetation. <br>

**NBR** = (NIR – SWIR) / (NIR + SWIR). This measures post-fire scars, and relevant for the analysis of burn severity and the detection of active vegetation post-fire.


The activity assumes knowledge of previous actvities and that you can create spectral profiles of surface types, inclduing vegetation, water, and bare soil.

## Introduction

Spectral indices are explored to improve our understanding of the spectral characteristics of environmental surface types. The environment comprise a range of different surface types, usually represented in remotely sensed imagery. A satellite imagery may contain several surafce types, such as water, vegetation, background soils, and artificial surfaces. However, usually, the user may be interested in only one of the surface types, say vegetation. Spectral indices is obtained to discriminate the surface types with an objective of detecting the surface type of interest.  Spectral indices can be more sensitive to one surface than another. While there are numerous spectral indices, the ones more sensitive to vegetation condition are referred to as vegetation indices. 



## Learning Outcomes

At the end of the activity, you should be able to: <br>

- create feature collections
- apply built-in function to compute spectral indices
- write and apply a user-defined function to compute spectral indices
- visualise and interpret spectral indices layers
  


## Activity Begins

Below is a Sentinel-2 Bottom of Atmosphere image ID obtained from the Earth Engine catalogue

COPERNICUS/S2_SR_HARMONIZED/20240428T013701_20240428T013722_T52LGL

Using this image,

1, compute (use the built-in function) and visualise the following spectral indices: 
    NDVI
    NDMI
    NBR

2, write your own function to compute the following spectral indices:
    
    NDVI
    NDMI
    NDWI
    NBR
    SAVI
    EVI



 ### Define a variable for the imagery and print this to the Console

 ```Javascript
var s2 = ee.Image("COPERNICUS/S2_SR_HARMONIZED/20240428T013701_20240428T013722_T52LGL")

//print variable to the Console
print(s2, 'Sentinel-2 data') //the second argument is an idenitifier of the variable in the Console
```
Run and click the expander to reveal the number of bands in the image. Your Console should be appear as shown in the figure below.


![image](https://github.com/user-attachments/assets/932324de-74e4-4a81-98d1-fec6264b1970)



### Select bands
The image has 26 bands, but not all of these bands are approriate for the computation of vegetation indices. Thus, select the relevant bands.

```Javascript
var s2 = s2.select(['B2', 'B3', 'B4','B8','B11','B12'])
print(s2, 'Sentinel-2 with selected bands') 
```


### Compute vegetation indices using built-in function

The function, **normalizedDifference(bandNames)**, would be used. This function can be found under **ee.Image** in Docs.
The function requires a lists of band names as the input. Two bands, at a time, are required. The second band is subtracted or added to the first as shown below:
 (first − second) / (first + second) <br>
 The output image band is given the name **'nd'** by default.

#### Make NDVI layer


```JavaScript
var ndvi = s2 //this is the multiband image to create the ndvi layer from
.normalizedDifference(["B8", "B4"]) //ndvi is computed

print(ndvi)
````
Expand the result in the Console, revealing the band name; this should be as the figure below:

![image](https://github.com/user-attachments/assets/8d89f376-304d-4776-9c22-01b6ef61525c)


The band name is "nd". The band can be renamed to a more meaningful name. Rename the "nd" as "NDVI". To do this, re-write the code as:

```JavaScript
var ndvi = s2 //this is the multiband image to create the ndvi layer from
.normalizedDifference(["B8", "B4"]) //ndvi is computed
.rename("NDVI") //renames the band

print(ndvi)
````

**Visualise the NDVI layer in greyscale and pseudocolour** 

```JavaScript
Map.addLayer(ndvi, {min:-1, max:1}, 'NDVI layer in greyscale')
Map.addLayer(ndvi, {min:-1, max:1, palette:[ 'purple', 'red', 'black', 'yellow', 'green']}, 'NDVI layer in pseudocolour')
```

The figures below show the result. The figure on the left is the NDVI layer in default greyscale, brighter pixels show higher NDVI values than the darker pixels. The figure on the right is the layer manipulated to have colours, the green pixels show higher NDVI which represent healthy vegetation.



![image](https://github.com/user-attachments/assets/85a6143b-ee68-48d1-be26-c8ed4b769fcd)   ![image](https://github.com/user-attachments/assets/db3fdb89-5bb8-437f-a31a-b1fa56ec455e)





#### Make NDWI layer

```JavaScript
var ndwi = s2 //this is the multiband image to create the ndvi layer from
.normalizedDifference(["B3", "B8"]) //ndwi is computed
.rename("NDWI") //renames the band

print(ndwi)
````

**Visualise the NDWI layer in greyscale and pseudocolour** 

```JavaScript
Map.addLayer(ndwi, {min:-1, max:1}, 'NDWI layer in greyscale')
Map.addLayer(ndwi, {min:-1, max:1, palette:[ 'red', 'green', 'yellow', 'blue', 'darkblue']}, 'NDWI layer in pseudocolour')
```
The figures below show the result for greyscale and pseudocolour, respectively:

![image](https://github.com/user-attachments/assets/4fd6db67-b014-4196-a0b7-8ae44b6c3585)       ![image](https://github.com/user-attachments/assets/0a2d8f74-32ab-45f0-9021-3ef4c0b97baf)









#### Make NBR layer

First, create and visualise a false colour combination (FCC) that accentuates fire scars in the image. You do this to justify the need for NBR.
The bands required for this FCC are SWIR, NIR, Red, in which they are assigned to RGB, respectively. Given fire scars are sensitive to the SWIR and RED, the output image is expected to represent fire scars as reddish pixels while healthy vegetation would be in shades of green. Note, bare soils can be confused with burnt fields.

```Javascript
Map.addLayer(s2, {bands:["B12","B8", "B4"], min:0, max:3000}, 'False Colour Composite for fire scars')
```

```JavaScript
var nbr = s2 
.normalizedDifference(["B8", "B12"]) //nbr is computed
.rename("NBR") //renames the band

print(nbr)
````

Visualise the output

```JavaScript
Map.addLayer(nbr, {min:-1, max:1}, 'NBR layer in greyscale')
Map.addLayer(nbr, {min:-1, max:1, palette:[ 'red', 'green', 'yellow', 'purple', 'black']}, 'NBR layer in pseudocolour')
```

The figures below show the NBR in greyscale and pseudocolour.


![image](https://github.com/user-attachments/assets/8f4b2a2b-d59b-4465-8839-062ce2472d7a)    ![image](https://github.com/user-attachments/assets/50e1c6b5-f880-4685-ac35-4274807d37cb)


### Compute vegetation indices using a user-defined function


```JavaScript

//select bands to be used for the computation
var blue = s2.select(["B2"])
var green = s2.select(["B3"])
var red = s2.select(["B4"])
var nir = s2.select(["B8"])
var swir = s2.select(["B12"])
//2.5 * ((NIR – RED) / ((NIR) + (C1 * RED) – (C2 * BLUE) + L))  C1=6, C2=7.5, and L=1
//user-defined function
var vegIndices = function(image){
var ndvi = nir.subtract(red).divide(nir.add(red)).rename('NDVI')
var ndmi = green.subtract(nir).divide(green.add(nir)).rename('NDMI')
var nbr = nir.subtract(swir).divide(nir.add(swir)).rename('NBR')
var evi = ((nir.subtract(red)).divide((nir.add(C1.multiply(red)).subtract(C2.multiply(blue)).add(L)).multiply(2.5)
return image.addBands(ndvi).addBands(ndmi).addBands(nbr).addBands(evi)
}

```

//apply/call the function to create additional bands (vegetation indices) to the imagery

```JavaScript
var veg_indices = vegIndice(s2)

//print the result to the Console
print(veg_indices)

```

Your result in the Console may look like this:


![image](https://github.com/user-attachments/assets/6ceefd10-3176-4061-a879-2f86bb397630)



## DIY

Using the same image as the used in the demo above, write a function that adds the following spectral indices as new bands to the original image

NDWI <br>
SAVI <br>
EVI 





**The End**






