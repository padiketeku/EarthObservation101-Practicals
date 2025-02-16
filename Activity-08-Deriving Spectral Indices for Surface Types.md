# Activity 8: Deriving Spectral Indices for Surface Types

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

Spectral indices are explored to improve our understanding of the spectral characteristics of environmental surface types. The environment comprises a range of different surface types, usually represented in remotely sensed imagery. A satellite imagery may contain several surface types, such as water, vegetation, background soils, and artificial surfaces. However, usually, the user may be interested in only one of the surface types, say vegetation. Spectral indices is obtained to discriminate the surface types with an objective of detecting the surface type of interest.  Spectral indices can be more sensitive to one surface than another. While there are numerous spectral indices, the ones more sensitive to vegetation condition are referred to as vegetation indices. 



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
Run and click the expander to reveal the number of bands in the image. Your Console should appear as the figure below.


![image](https://github.com/user-attachments/assets/932324de-74e4-4a81-98d1-fec6264b1970)



### Select bands
The image has 26 bands, but not all of these bands are approriate for the computation of vegetation indices. Thus, select the relevant bands.

```Javascript
var s2 = s2.select(['B2', 'B3', 'B4','B8','B11','B12'])
print(s2, 'Sentinel-2 with selected bands') 
```


### Compute vegetation indices using built-in function

The function, **normalizedDifference(bandNames)**, would be used. This function can be found under **ee.Image** in `Docs`.
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
The bands required for this FCC are SWIR, NIR, Red, and are assigned to RGB, respectively. Given fire scars are sensitive to the SWIR and RED, the output image is expected to represent fire scars as reddish pixels while healthy vegetation would be in shades of green. Note, bare soils can be spectrally similar to burnt fields.

```Javascript
Map.addLayer(s2, {bands:["B12","B8", "B4"], min:0, max:3000}, 'False Colour Composite for fire scars')
```



![image](https://github.com/user-attachments/assets/1c0a1e3e-6bc9-4e01-86d3-4789d300ff8f)


A Sentinel-2 false colour band combination (B12, B8, B4) to display fire scars. Examples of fire scar pixels are highlighted with a red circle.


Given we can see fire scars in the image above, using a more robust technique to delineate burnt areas has been applied below. The normalised burn ratio is computed and visualised for fire scar analysis. 


```JavaScript
var nbr = s2 
.normalizedDifference(["B8", "B12"]) //nbr is computed
.rename("NBR") //renames the band

print(nbr)
````

Visualise the output

```JavaScript
Map.addLayer(nbr, {min:-1, max:1}, 'NBR layer in greyscale')
Map.addLayer(nbr, {min:-1, max:1, palette:[ 'red', 'magenta', 'purple', 'yellow', 'green', 'darkgreen']}, 'NBR layer in pseudocolour')

```

The figures below show the NBR in greyscale and pseudocolour. Theoretically, the NBR values range between -1 and 1,  with values close to 1 indicating healthy vegetation and values near -1 or sub 0 representing burnt or unproductive vegetation.


![image](https://github.com/user-attachments/assets/fe70a91f-edd0-4e00-9713-4a397569d4cd) ![image](https://github.com/user-attachments/assets/42c4e29f-1773-434e-9bf7-c0f2ec6f19d5)
  



The figure to your right is the pseudocolour NBR image with the pixels in shades of red potentially burnt vegetation.





### Compute vegetation indices using a user-defined function


```JavaScript

//select bands to be used for the computation
var blue = s2.select(["B2"])
var green = s2.select(["B3"])
var red = s2.select(["B4"])
var nir = s2.select(["B8"])
var swir = s2.select(["B12"])

//user-defined function
var vegIndices = function(image){
var ndvi = nir.subtract(red).divide(nir.add(red)).rename('NDVI')
var ndmi = green.subtract(nir).divide(green.add(nir)).rename('NDMI')
var nbr = nir.subtract(swir).divide(nir.add(swir)).rename('NBR')
return image.addBands(ndvi).addBands(ndmi).addBands(nbr)
}

```

//apply the function to create additional bands (vegetation indices) to the imagery

```JavaScript
var veg_indices = vegIndices(s2)

//print the result to the Console. note that the layer name is set to "Vegetation Indices"
print(veg_indices, 'Vegetation Indices')

```

Your result in the Console may look like this:


![image](https://github.com/user-attachments/assets/6e5bdd10-bb05-40aa-8b68-d4d56656d55c)




The vegetation indices, the additional spectral bands, are identified using the red polygon.



## DIY

Using the same image as the one used in the demo above, produce the following spectral indices as new bands to the original image. You may look up (google search) for the equations for the indices below. Note that you are using Sentinel-2 data.

Normalised Difference Water Index (NDWI) [Gao, 1996](https://www.sciencedirect.com/science/article/pii/S0034425796000673) <br>
Soil Adjusted Vegetation Index (SAVI) [Huete, 1988](https://www.sciencedirect.com/science/article/abs/pii/003442578890106X) <br>
Enhanced Vegetation Index (EVI) [Huete et al., 2002](https://www.sciencedirect.com/science/article/pii/S0034425702000962?via%3Dihub) 





**The End**






