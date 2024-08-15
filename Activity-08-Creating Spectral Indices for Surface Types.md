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


## DIY






**The End**






