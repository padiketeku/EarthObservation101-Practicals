# Activity 9: Working with Temperature Data

In this practical, you will explore MODIS land surface temperature (LST) to understand temperature of surface types. Also, you will explore the relationship between temperature and vegetation by plotting MODIS NDVI against MODIS LST. The MODIS NDVI will be derived from the surface reflectance product. The hypothesis behind this activity is that high vegetation areas can be shady and would have lower temperatures compared to deserts or low vegetation areas. 
Assumes knowledge of previous actvities and that you can create spectral indices, including NDVI.

## Introduction
Temperature is an environmental parameter useful for the monitoring of the biosphere, hydrosphere, atmosphere, and lithosphere. In-field measurement of temperature can be expensive and time-consuming, while thermal remote sensing provides a more cost-efficient method for a rapid collection of temperature data. Passive remote sensing systems utilising visible, near-infrared, and microwave sensors record reflected energy. In contrast, thermal sensors detect the temperature of materials by recording the emitted energy. The energy emitted is a product of the incident radiation the materials absorbed. Ideally, the more energy the materials absorb (hotter materials) the more energy they can emit.


![image](https://github.com/user-attachments/assets/c808f19c-dca0-4ab1-b787-62d953088403)




The materials we see in our environments, such as rocks, vegetation, buildings do not emit all the absorbed radiation as a theoretical blackbody (absorbs all incident radiation and emits all radiation) would. Surface types emit a fraction of the radiation emitted by a blackbody at the equivalent temperature. The fraction is called `emissivity` (ε); which ranges between 0 and 1. The imaginary blackbody has an emissivity of 1, while real-world materials have emissivity less than 1. The temperature a remote sensing detector records is not only driven by the **temperature** of the object but also **emissivity**.


E =  εσT<sup> 4</sup>  (Stefan-Boltzmann Law); 

where E = total energy radiated per unit surface area of a black body across all wavelengths per unit time, σ =  Stefan-Boltzmann Constant of 5.6697 x 10-8 W m<sup> -2</sup> K<sup>- 4</sup>, and T = absolute temperature (K). 

The temperature we measure in the field with thermometers is the true temperature, aka, kinetic temperature. The remote sensing system detects radiant temperature, a descriptive measure of radiation regarding the assumption that the temperature of a blackbody emits an identical amount of radiation at the same wavelength. There is a strong correlation between kinetic temperature and radiant temperature, which makes remote sensing an important technology for monitoring the temperature of surface materials. The temperature of surface materials, referred to as land surface temperature (LST), is a product from the remote sensing measurement. The remote sensing detector records radiance, which is converted to radiant temperature using Planck’s equation. The radiant temperature is converted to LST (in Kelvin) by accounting for emissivity and atmospheric effects.
Satellite remote sensing systems that collect temperature data for public use include Landsat, Moderate-resolution Imaging Spectroradiometer (MODIS), and Advanced Spaceborne Thermal Emission and Reflection Radiometer (ASTER).


## Learning Outcomes

At the end of this activity, you should be able to: <br>
- Define a polygonal region of interest using a list of coordinates
- Display temperature data for visualisation
- Convert absolute temperature to celsius 
- Change the projection and scale of images
- Perform correlation analysis
- Produce image histograms
- Produce image scatter plot


## Activity Begins

Thermal remote sensing has a wide application areas, including archaeology (e.g., https://weather.ndc.nasa.gov/archeology/chaco_compare.html), drought monitoring (e.g., https://lpdaac.usgs.gov/resources/data-action/two-sensors-are-greater-one-observing-drought-smap-and-modis/), and urban heat islands. 

In this practical you are required to use publicly available satellite imagery to confirm this hypothesis by exploring the relationship between vegetation and temperature. The region of interest is given by the list of coordinates below. <br>
[142.06594410835254,-17.94898241151206, <br>
145.38930836616504,-17.94898241151206, <br>
145.38930836616504,-16.005194143426337, <br>
142.06594410835254,-16.005194143426337, <br>
142.06594410835254,-17.94898241151206]

Also, you are required to restrict the analysis to September 2021 as the study area is on record for experiencing extremely high temperatures on this date.

### Search and import into the Code Editor the following MODIS products:
- MOD11A2 - land surface temperature
- MOD09A1 - surface reflectance

### Rename the image collections and print each to the Console

Your code snippet should be similar to ones below:

```JavaScript 
print(sr, 'surface reflectance')
print(lst, 'temp')
````

## MODIS Land Surface Temperature


### Click the LST collection and explore the metadata, take note of units and scale

```JavaScript

//a list of coordinates to define a region of interest

var listCoords = [142.06594410835254,-17.94898241151206,
145.38930836616504,-17.94898241151206,
145.38930836616504,-16.005194143426337,
142.06594410835254,-16.005194143426337,
142.06594410835254,-17.94898241151206]

```

### Create a polygon to define a region of interest

```JavaScript
var roi = ee.Geometry.Polygon(listCoords)
```

### Display the ROI to the Console

```JavaScript
Map.addLayer(roi, {}, 'ROI')
```

### Filter the lst collection

```JavaScript

var daytimeTemp = lst

// filter by date
.filterDate('2021-09-01', '2021-09-30')

//select the band
.select('LST_Day_1km')

//average of the collection
.mean()

```


### Print the variable, expand the image in the Console, and explore the metadata


```JavaScript
print( daytimeTemp)
```

Question: what is the name of the band?

### Rename the band name as 'daytimeLST'

```JavaScript

var daytimeTemp = daytimeTemp.rename('daytimeLST')
print(daytimeTemp, ' daytimeLST')

```

### Display the single-band image for visualisation

```JavaScript
Map.addLayer( daytimeTemp,{}, 'daytimeLST')

```

### DIY

Navigate to your ROI; use the Inspector tool to randomly inspect pixel values. <br>
Questions: <br>
- What is the range of pixel values you observed? <br>
- What is the unit of measurement? 


### Re-scale the pixel values and convert to Celsius

You may have observed that the pixel values are large (not normal) and require re-scaling; so now let’s rescale the data and convert the unit from K to Celsius.

```JavaScript
var daytimeTemp_rescaled = daytimeTemp.multiply(0.02).subtract(273.15)
```

After re-scaling the image, the temperature values may show high precision, but we would round the values up or down to minimise precision. To be sure this transformation does not alter the spatial reference of the output data you must specify the coordinate reference system and scale. In this case, we have maintained the spatial resolution of the LST product as the scale.


### Round the temperature values and keep the projection

```JavaScript
var daytimeTemp = daytimeTemp_rescaled.round().reproject({crs:'EPSG:4326', scale:1000.0})
print(daytimeTemp, "daytimeTemp_rescaled")
```


### Display the rescaled LST as a pseudocolour image

```JavaScript
Map.addLayer( daytimeTemp,{min:10, max:45, palette:['blue', 'peachpuff', 'yellow', 'orange', 'red']}, 'daytimeLST')
```

![image](https://github.com/user-attachments/assets/2f384da7-7111-4f1d-8808-e0cd646284a6) |
|:--:|
| *Fig. 1. MODIS land surface temperature map of Australia for Sep 2021. Shades of blue show low temperature areas, while red pixels show hot areas. Other pixels fall between this range.*|






**`Task`**: Describe the spatial distribution of Australia's temperature on this date




## MODIS Surface Reflectance


This is another MODIS global data that provides surface reflectance from channels including red and near-infrared. A pixel size is approximately 0.5 km. We would use this data to compute NDVI to evaluate vegetation conditions. 


### Explore the MODIS surface reflectance product

Click the surface reflectance collection in *Imports* of the **Code Editor**; in the **`Console`** explore BANDS. There are seven surface reflectance bands. Make a list of these bands, including the wavelengths. Which bands would be RED and NIR for NDVI computation?

### Filter the surface reflectance data

```JavaScript
var sr = sr.filterDate('2021-09-01', '2021-09-30') // filter by date
//select bands required for the computation of NDVI
.select(["sur_refl_b01", "sur_refl_b02"])

//select first image in the pack
.mean()

//print the variable to the Console
print(sr, 'SR1')
```

### Match the scale and projection to the LST data

The pixel size for the reflectance data is 0.5km, so to compare this data with the temperature we would have to make the pixel size 1km.

```JavaScript
var sr = sr.reproject({crs:'EPSG:4326', scale:1000.0})
print(sr, 'SR')
```

### Selecting bands and compute NDVI

There are more bands than we need to compute the NDVI, so let’s select the required bands for NDVI.

```JavaScript
//select the red and nir bands to compute NDVI
var red = sr.select("sur_refl_b01")
var nir = sr.select("sur_refl_b02")
print(red, ' red')

//compute NDVI. take note of this method of computing NDVI
var ndvi = nir.subtract(red).divide(nir.add(red)).rename('NDVI')

```


### Visualisation

You may like to visualise the NDVI layer, but the problem is that this is global data and you may run out of space. It is ideal to clip this layer to your region of interest, use the clip() function to resize. Before we do this let’s use a histogram to see the range of NDVI values.


#### Image Histogram


```JavaScript

//plot a histogram for the NDVI layer. region is the region of interest, so specify the geometry
var chartNDVI =ui.Chart.image.histogram({image:ndvi, region: roi})
print (chartNDVI, ' HistogramNDVI')

```

![image](https://github.com/user-attachments/assets/4ae3c699-ba1c-43ba-b74e-bc71824e1d4d) |
|:--:|
| *Fig. 2. A histogram for an NDVI layer.*|

```JavaScript

//display NDVI layer of the study area for visualisation
Map.addLayer(ndvi.clip(roi), {min:0, max:0.5, palette:['darkred', 'peachpuff','yellow', 'green']}, " NDVI")
```

Your result should be similar to the one below.



![image](https://github.com/user-attachments/assets/5705db71-f8b1-4527-8147-9705c5a348e9) |
|:--:|
| *Fig. 3. NDVI layer of the study area.*|


## Image Correlation


Now that we have the LST and NDVI layers for the study area, we can determine the correlation between temperature and vegetation. To do this, firstly merge the temperature and NDVI layers to have a single image with two bands, rather than two separate images.

### Merge the temperature and NDVI layers

```JavaScript
var temp_ndvi = daytimeTemp.addBands(ndvi)
print(temp_ndvi)
```


### Determine the correlation between NDVI and temperature


```JavaScript

//sample data for the correlation test, in this case we are sampling no more than 5000 features to avoid breaching the Earth Engine limit.  
var sample = temp_ndvi.sample({region:roi, numPixels: 5000})

//print the variable to the Console and identify the number of features available for the correlation test
print(sample, 'sample')

```

#### Start the correlation by first calling the required method

```JavaScript
var correl = ee.Reducer.pearsonsCorrelation();
```

```JavaScript

//Correlation test- choose the two properties for the analysis 
var correlation = sample.reduceColumns(correl, ['daytimeLST', 'NDVI'])

//print the correlation results to the Console
print(correlation, 'Correlation Results')

```

Your result may be as shown below <br>

**Correlation: -0.87; p-value: 0**


#### Interpret Correlation

This result means that there was a strong negative correlation between temperature and NDVI. Does this make sense? Does this confirm the hypothesis? Post your comments to the forum. Alternatively, share your thoughts in class.


#### Scatter Plot


We can graphically support this result through a scatter plot.

```JavaScript

//plot a scatterplot of NDVI against temperature and print the result to the Console
var scatterPlot = ui.Chart.feature.byFeature(sample, 'NDVI', 'daytimeLST')
  .setChartType('ScatterChart')
  .setOptions({ pointSize: 2, pointColor: 'blue', width: 300, height: 300, titleX: 'NDVI', titleY: 'Temperature (C)' })
   
print(scatterPlot) 

```

The scatter plot is shown below.

![image](https://github.com/user-attachments/assets/744866a9-a8c0-48f9-a5af-6d03a2be87dc) |
|:--:|
| *Fig. 4. A scatter plot showing the relationship between temperature and NDVI.*|


## DIY


Produce a scatter plot explaining the relationship between temperature and NDVI (use MODIS LST and MODIS surface reflectance products) for the region of interest below. Limit the temporal scale to December 2019 (a meteorological drought year in Australia).

[150.765,-33.88876442217107, 150.998,-33.889,
150.998,-33.692,150.765,-33.693,
150.765,-33.889]

Given the correlation test and scatter plot, explain your results in plain English. Post this to the discussion forum.


## Conclusion

In this chapter, we have explored the concepts of thermal remote sensing through an activity that demonstrates the use of MODIS products, including land surface temperature and surface reflectance, to test the relationship between temperature and vegetation. Through this activity, students can create image histograms, scatter plots and perform correlation between bands








