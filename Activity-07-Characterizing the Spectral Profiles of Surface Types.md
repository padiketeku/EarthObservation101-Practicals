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


### Visualise the image
You now have a single image to produce spectral response curves, but before you start, you would want to visualise this image to identify the major surface types in the image.
Ideally, you would prefer to display the image to your study area. Thus, you must first set the centre of the map to the coordinates of your study area (i.e., ROI). When you expand the coordinates of the ROI under Imports , as shown below, you might be able to select one of these coordinates to use. An example is highlighted in red. We would use this coordinate.


![image](https://github.com/user-attachments/assets/aa047ec8-25c3-4fa2-adcc-be869b8c5698)


The `Map.setCenter` (Fig. 2) is used to centre the map view. 


![image](https://github.com/user-attachments/assets/1648547e-cc55-4d61-8cec-368242d976a4)
|:--:|
| *Fig. 2. The method to centre the map view.*|


Three number arguments are required to run this method: longitude, latitude, and zoom. The zoom level ranges from 0 to 24. To visualise a true colour composite of the image the code should be as shown below. 

```JavaScript
//first, centre the map to the coodinates of the region of interest
Map.setCenter(130.955, -12.240, 9); //the first tow inputs are the coordinates and the third is zoom level

//visualise a true colour composite of the average image
Map.addLayer(sen2sr_mean, {
min:0,
max:3000,
bands:['B4','B3','B2'],
});
```

When you display your layer over the base map, make sure you turn off the ROI polygon for improved image visualisation. You turn off the ROI polygon by hovering your mouse over the **Geometry Imports** and untick the box to the geometry name.


### Clip an image to a region of interest (ROI)

Through the visualisation, you can see that your ROI is just a fraction of the image. To confine your analysis to your ROI, you must clip the image to the ROI. In the **Docs** tab, locate the image object and the clip method under this object, as shown below.


![image](https://github.com/user-attachments/assets/07d91ff6-a8a6-49be-9311-bb464a5afeb5)


The clip method requires a geometry as an input argument. The ROI polygon is the input Additionally, the image comprises 23 bands, so you would like to select the relevant bands to investigate the surface types. In this activity, we explored B2, B3, B4, B5, B6, B7, B8, and B11. The code thus becomes:

```JavaScript

//select the required bands and clip image to ROI
var sen2sr_mean= sen2sr_mean.select('B2', 'B3', 'B4', 'B5', 'B6', 'B7', 'B8','B11' ).clip(roiDarwin); 

//first, centre the map to the coodinates of the region of interest
Map.setCenter(130.955, -12.240, 9); //the first tow inputs are the coordinates and the third is zoom level

//visualise a true colour composite of the average image
Map.addLayer(sen2sr_mean, {
min:0,
max:3000,
bands:['B4','B3','B2'],
});
```

If you run the code your result might be similar to Fig. 3 below.




![image](https://github.com/user-attachments/assets/03a57bab-6b46-40c6-923c-5b63ab580efe) 
|:--:|
| *Fig. 3. A Sentinel-2 Level-2A image clipped to a region of interest.*|


### Feature collection- create polygons for surface types


In exploring the image, you can see many features including trees interspersed with bare soils. We would limit the analysis to these surface types: water, urban, crop field, and riparian forest. We must create a polygon for each surface type. Again, we used the geometry tools. Hover your mouse over the **Geometry Imports**, click ‘new layer’, select the polygon geometry (aka “draw a shape”) and define a polygon over water pixels. In the Code Editor rename the "geometry" to "water". For spectral response curves, a feature is required. Thus, change the geometry you have created to a feature by clicking on the cog icon (![image](https://github.com/user-attachments/assets/806efca7-d1f5-4357-9a8e-42311a18e139)) next to **water** under **Geometry Imports**. A **Configure geometry import** window immediately pops up (Fig. 4), similar to the figure below.



![image](https://github.com/user-attachments/assets/7532515a-5465-4d1d-8816-9d088fb11090)
|:--:|
| *Fig. 4. Edit layer properties window.*|



Note that the colour assigned to your surface type might be different to the one used here. But you can change the colour using colour editor (Fig. 4). A feature collection, stacking features together, is required for the spectral response curve. Thus, change the **'Import as'** from **'Geometry'** to **'FeatureCollection'** using the dropdown triangle. This makes it possible to edit the properties. Click the '+ property' and define Property as 'label' and ‘value’ as the name of the class (e.g. water, urban, forest). Be consistent in the use of upper and lower cases, while typing into ‘value’. When done the **Configure geometry import** is updated as follows. 


![image](https://github.com/user-attachments/assets/d2afd36b-83fa-4afd-8dd9-099b80b6d994)



You can use the colour palette to edit the colour for a surface type. Finally, click OK to save the changes. Repeat the steps to creating a feature collection using the geometry tool to create feature collection for the remaining surface types. Remember you create a new geometry by clicking the **“+new layer”** in **Geometry Imports**. Also, make sure you save your changes. At the end, you should have the features for the surface types defined as follows (Fig. 5). Water is blue, urban is cyan, crop field is green, and riparian forest is red.

![image](https://github.com/user-attachments/assets/ebba018e-6323-41a5-95d9-e03b1fb3e884)
|:--:|
| *Fig. 5. Features created for surface types using the geometry tool.*|

## Merge the feature collections
We have four feature collection items; we would merge them into a single feature collection. This gives us a feature collection of feature collections. A line of code for this feature collection was then added to the existing code.

```JavaScript
//merge the feature collections
var featureCollection = water.merge(urban).merge(field).merge(forest);
```

### Produce spectral response curves

To do this, we used the `ui.Chart.image.regions` (Fig. 6). To explain this algorithm, the ui = user interface, Chart.image means you want to create a chart from an image, while there are many methods including histogram the one required for a spectral response curve is the **regions**. Hence, we used `ui.Chart.image.regions`, which extracts and plots the value of each band in one or more regions. The x-axis would be the band name, and y-axis would be reflectance.


![image](https://github.com/user-attachments/assets/d9189c13-427e-4d51-a96d-8019f61c7df9)
|:--:|
| *Fig. 5. Details on the method useful for creating spectral response curves.*|


The lines of code to produce the spectral response curves have been added to the existing, which is now as shown below.


```JavaScript
//create the reflectance chart
var spectralCurve = ui.Chart.image.regions({
image: sen2sr_mean, //the image to provide the reflectance data
regions: featureCollection, //the regions within the image to sample from
reducer: ee.Reducer.mean(), //this computes mean reflectance for the y-aixs
scale: 10, //the pixel size in metres
seriesProperty: 'label'}); //uses the lable property we defined using the geometry tool

//print the chart to the Console
print(spectralCurve)
```

Given this is a chart, you need to print it to the **`Console`** using the print command. Make sure your Console has enough space to accommodate the chart. Your result in the **`Console`** should be similar to the one below.  You might have to click the expander icon, highlighted in Figure 4.15, to open the chart in a new tab for a better view of the chart.


![image](https://github.com/user-attachments/assets/e24df55d-c170-4b65-9815-3e08e8884ab5)
|:--:|
| *Fig. 5. A spectral response curve for a Sentinel-2 Level-2A image. The curves for water, urban, crop fields and a riparian forest are given. The x-axis shows the spectral bands, while the y—axis shows the mean reflectance values. This plot needs further improvements, which will be made by updating the chart.*|


It is important to assess your charts and maps to be sure they make sense. The x-axis is the spectral bands, labelled as “Band”, and the y-axis is the mean spectral reflectance, labelled as “Band mean”. The figure caption is “Image band values in 4 regions”. These are default labels and must be edited if they do not make sense. Also, on the x-axis the bands are not in correct order. We would need to edit the code to obtain a standard spectral reflectance curve, which is more intuitive and interpretable. Rather than using the band names, we would specify the actual wavelength for the bands to be on the x-axis. Identify the centre wavelength for the Sentinel-2 bands from this [link](https://sentinels.copernicus.eu/web/sentinel/user-guides/sentinel-2-msi/resolutions/spatial). We defined a new variable for the wavelengths and another variable to edit default labels and the colours for the curves. The new/additional lines of code to edit the spectral response curves for a more intuitive one is shown below. 

### Edit the spectral response curve

```JavaScript

//create a list of wavelengths (in nanometer), which corresponds to the Sentinel-2 bands used
var wavelengths = [490,560,665,705,740,783, 842,1610];

//create a variable that edits the x-and-y-axes labels, title, and the colour of the curves
var editChart = {
title: 'A spectral response cuve from a Sentinel-2 image' //this is the title of the chart
hAxis: {title:'Wavelength (nanometer)'},//the horizontal axis title
vAxis: {title:'Reflectance'},//the vertical axis title
lineWidth: 1,//width or thicknes of the curves
pointSize: 4,//displays the reflectance values using a point, and the size of this point 4
series: {//edits the colour of the curves
0:{color: 'blue'},//blue trace for water
1:{color: 'magenta'},//magenta trace for urban
2:{color: 'green'},//green trace for crop fields
3:{color: 'purple'},//purple trace for riparian forest
}
};

```

  - Reproduce the spectral response curves

```JavaScript
var spectralCurve2 = ui.Chart.image.regions({
image: sen2sr_mean,//the image to provide the reflectance data
regions: featureCollection,//the regions within the image to sample from
reducer: ee.Reducer.mean(),//this computes mean reflectance for the y-aixs
scale: 10, //the pixel size in metres
seriesProperty: 'label',//uses the label property we defined using the geometry tool
xLabels:wavelengths,//defines the wavelengths on the x-axis
}).setOptions(editChart);//a method to edit the chart object created earlier

//print the chart to the Console
print(spectralCurve2);
```


![image](https://github.com/user-attachments/assets/1f20b22b-11ec-4c97-a46e-adb14abafd3f) |
|:--:|
| *Fig. 6. Spectral response curves for water, urban, crop fields and a riparian forest. Sentinel-2 Bottom of Atmosphere product was used, the centre wavelengths for only band 2 (blue light), band 3 (green light), band 4 (red light), band 5 (red edge), band 6 (red edge), band 7 (red edge), band 8 (near-infrared), and band 11 (shortwave infrared) were used.*|



In the top right corner, you have the options to download the chart as a CSV, SVG or PNG file.

### Interpret the spectral response curves


It is important to interpret your result. The urban cover shows the highest reflectance for all the light employed. This makes sense as built-up areas comprise a mixture of materials. Water generally has low reflectance with the highest reflectance observed for green light. The green reflectance for water is even higher than green reflectance for crop field and riparian forest. This is not surprising as it was evident in the imagery. Could this be that the water pixels selected had significant amount of photosynthetic materials? Crop fields and forest showed similar spectral profile, which matches textbook example of a healthy vegetation in that NIR is the peak reflectance while the reflectance decreases with increasing wavelengths. The crop field shows higher reflectance across all bands. 



## DIY


Below is a Sentinel-2 Bottom of Atmosphere image ID obtained from the Earth Engine catalogue
**`COPERNICUS/S2_SR_HARMONIZED/20240428T013701_20240428T013722_T52LGL`** <br>

Using this image,
1, construct spectral response curves for the following surface types
- water
- crop fields
- urban
- forest

2, Explain the variations in spectral characteristics between the surface types


**The End**
