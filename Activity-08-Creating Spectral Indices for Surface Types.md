# Activity 8: Creating Spectral Indices for Surface Types
In this practical activity, you will explore Landsat 8 and Sentinel-2 imagey to consolidate your knowledge on spectral indices. A range of spectral indices can be computed from the imagery, but in this activity frequentyly used indices, such as the normalised difference vegetation index (NDVI), normalised difference water index (NDWI), enhanced vegetation index (ENVI), soil adjusted vegetation index (SAVI), and normalised burn ratio (NBR). The activity assumes knowledge of previous actvities and that you can create spectral profiles of surface types, inclduing vegetation, water, and bare soil.

## Introduction

Spectral indices are explored to improve our understanding of the spectral characterics of environmental surface types. The environment comprise a range of different surface types, usually represented in remotely sensed imagery. A satellite imagery may contain several surafce types, such as water, vegetation, background soils, and artificial surfaces. However, usually, the user may be interested in only one of the surface types, say vegetation. Spectral indices is obtained to discriminate the surface types with an objective of detecting the surface type of interest.  Spectral indices can be more sensitive to a surface than others. While there are numerous spectral indices, the ones more sensitive to vegetation condition are referred to as vegetation indices. 



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

1, construct spectral response curves for the following surface types

    water
    crop fields
    urban
    forest

2, explain the variations in spectral characteristics between the surface types





|:--:|
| *Fig. 1. TThe range of pixel values for a mean Sentinel-2 Level-2A image.*|

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
The function requires a lists of band names as the input. Two bands, at a time, are required in that the second band is subtracted or added to the first as shown below:
 (first − second) / (first + second) <br>
 The output image band is given the name is **'nd'** by default.


```JavaScript
var ndvi = s2 //this is the multiband image to create the ndvi layer from
.normalizedDifference(["B8", "B4"]) //ndvi is computed

print(ndvi)
````
Expand the result in the Console, revealing the band name; this should be as the figure below:

![image](https://github.com/user-attachments/assets/8d89f376-304d-4776-9c22-01b6ef61525c)


The band name is "nd". The band can be renamed to a more meaningful name. In this case rename the band as "NDVI". To do this, re-write the code as:

```JavaScript
var ndvi = s2 //this is the multiband image to create the ndvi layer from
.normalizedDifference(["B8", "B4"]) //ndvi is computed
.rename("NDVI") //renames the band

print(ndvi)
````
## DIY






**The End**






