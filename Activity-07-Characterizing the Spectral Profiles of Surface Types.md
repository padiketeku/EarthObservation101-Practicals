# Activity 7: Characterizing the Spectral Profiles of Surface Types


## Introduction

A spectral response curve is plotting the amount of return energy recorded by the sensor for each waveband of the remote sensing device for a target of interest. A waveband is plotted against reflectance in a two-dimensional plot. Ideally, the curve shows a unique profile of the target, which might be used to discriminate the target from other targets in the image. The curve is usually characterised by peaks and troughs with the peaks representing reflectance. The troughs would be low reflectance, but high absorption of energy by the target. A single image is required to produce a spectral reflectance curve. Although it is possible, we are not going to create spectral response curve for each of the images in the collection. We demonstrated a couple of approaches to retrieve a single image from the collection to produce a spectral response curve. This activity builds on the activity 6 with the aim of identifying major surface types and produce their spectral response curves. It is assumed that you are able to retrieve an image collection from the Earth Engine catalog and filter the collection to the requirements of your project. The region of interest created in Activity 6 is used.


## Learning Outcomes
At the end of this activity, you should be able to:

- Visualize an image collection
- Create feature collections
- Clip an image
- Produce and interpret spectral response curves



## Activity Begins


### Get image for the project 


```JavaScript
var sen2sr = ee.ImageCollection("COPERNICUS/S2_SR_HARMONIZED");

//call the image collection and keep the same variable name; you may rename the variable if you want
var sen2sr = sen2sr

//filter by the study peeriod
.filterDate(‘2022-01-01’, ‘2023-12-31’)

//filter by region of interest
.filterBounds(roiDarwin)

//filter by cloud cover percentage
.filter(ee.Filter.lte('CLOUD_COVERAGE_ASSESSMENT', 10))

//select the first image in the collection as this is the image with the lowest cloud coverage
.first();

//print the collection to the Console
print(sen2sr);
```

This code returns a single image with 23 bands. The downside of this approach is that the information in the other images would not be assessed. To leverage the information of the entire collection then you might want to compute the average image using all the images. The code would then be as the following. 

```JavaScript
var sen2sr = ee.ImageCollection("COPERNICUS/S2_SR_HARMONIZED");

//call the image collection and keep the same variable name; you may rename the variable if you want
var sen2sr = sen2sr

//filter by the study peeriod
.filterDate(‘2022-01-01’, ‘2023-12-31’)

//filter by region of interest
.filterBounds(roiDarwin)

//filter by cloud cover percentage
.filter(ee.Filter.lte('CLOUD_COVERAGE_ASSESSMENT', 10));

//average image using all the images in the collection
var sen2sr_mean =sen2sr.mean();

//print the collection to the Console
print(sen2sr_mean);
```

Run the code and explore the result in the Console, take note of the bands and the range of pixel values. The figure below highlights the range of pixel values for the standard bands.

![image](https://github.com/user-attachments/assets/dc908a03-bf3a-4f1b-9671-47f5885ddf7f)
|:--:|
| *Fig. 1. TThe range of pixel values for a mean Sentinel-2 Level-2A image.*|


You now have a single image to produce spectral response curves, but before you start, you would want to visualise this image to identify the major surface types in the image.
Ideally, you would prefer to display the image to your study area. Thus, you must first set the centre of the map to the coordinates of your study area (i.e., ROI). When you expand the coordinates of the ROI under Imports , as shown below, you might be able to select one of these coordinates to use. An example is highlighted in red. We would use this coordinate.


![image](https://github.com/user-attachments/assets/aa047ec8-25c3-4fa2-adcc-be869b8c5698)


The `Map.setCenter` (Fig. 2) is used to centre the map view. 


![image](https://github.com/user-attachments/assets/1648547e-cc55-4d61-8cec-368242d976a4)
|:--:|
| *Fig. 2. The method to centre the map view.*|


Three number arguments are required to run this method: longitude, latitude, and zoom. The zoom level ranges from 0 to 24. To visualise a true colour composite of the image the code should be as shown below. 

