# Activity 10: Working with SAR Data

Synthetic Aperture Radar (SAR) is the manipulation of the movement of the remote sensing platform to produce high resolution imagery. In this practical, you will search for, visualise and interpret Sentinel-1 SAR imagery. You will validate the Sentinel-1 SAR imagery using optical imagery. 


## Introduction


Satellite-based synthetic aperture radar (SAR) data complements multispectral data. However, the SAR data becomes more important for study areas that are frequently covered by clouds. The SAR systems have active sensors that employ microwave energy, which readily penetrates through cloud cover and provides data regardless of the time of the day. A spaceborne SAR system has an antenna(s) that transmits and receives artificial microwave energy. The microwave energy used by SAR systems is fully controlled or polarised. Currently the spaceborne SAR systems operate a single band of microwave light, but measure the different polarisations of the light waves. A linear polarisation detected by the SAR system is denoted by the transmitted and received orientations of the wave such as VV, HH, VH, or HV. The first letter is the orientation of the transmitted energy while the latter denotes  the orientation of the received energy. The figure below explains the four linear polarisation data a SAR system may produce.



![image](https://github.com/user-attachments/assets/7ab09a24-e2de-443e-a2c2-a1a9a71abd65)




For instance, a VH polarisation record means the system transmitted the microwave energy in a vertical orientation while the return energy was received horizontally. The intensity of the return energy is measured by the receiving antenna, and is computed as a *backscatter coefficient* usually transformed to decibels (dB) for the purposes of visualisation (contrast enhancement).



## Learning Outcomes

At the end of this acitivity you should be able

- Collect Sentinel-1 SAR imagery

- Visualise Sentinel-1 SAR imagery

- Validate/interppret Sentinel-1 SAR imagery



## Activity Begins


Because SAR is a single-band system, the images are grayscale with brighter and darker pixels showing high and low backscatter values, respectively. In this task you would visualise Sentinel-1 SAR image of Darwin city and your result is expected to be similar to the one below. 




![image](https://github.com/user-attachments/assets/bdf4748e-6d4d-4a98-9ed7-7eec95fb15b0)





Note, variation in results is expected as your region of interest geometry might not be the same as the one used to make this practical. Nevertheless, make sure Darwin city can be found in the SAR image you choose.


### Define a region of interest

1, Use the geometry tool, *Draw a rectangle*, to define a region of interest including Darwin city

2, Rename the geometry "darwin"


### Search, explore and import data

3, Search the data catalog using "Sentinel-1" to find the **Sentinel-1 SAR GRD** collection. The first data item in the pack. Click this data file to see the metadata as shown below.




![image](https://github.com/user-attachments/assets/580d8b2d-cb72-4661-990e-4e30bdfebdc3)





4, Explore the description, bands and image properties. Make notes on the bands, and image properties: "orbitProperties_pass", "instrumentMode"


5,  Import the collection into the Code Editor and rename the collection as "sent1"


### Filter the image collection


6, Filter the collection to retrieve wet season images only. The code below achieves this task.

```JavaScript
var sentinel_1= sent1

//filter by ascending orbit mode
.filter(ee.Filter.eq('orbitProperties_pass', 'ASCENDING'))

//filter by instrument mode; best to use IW = interferometric wide
.filter(ee.Filter.eq('instrumentMode', 'IW'))

//filter by a date range- wet season images only
.filterDate("2015-01-01", "2021-12-31")

//restrict the image acquisition date to specific months
.filter(ee.Filter.calendarRange(12, 3, 'month'))

//filter by study area; this means select scenes that overlap the study area
.filterBounds(darwin)

//select the VV and VH polarisation bands
.select(['VV', 'VH']);
```

Print the collection to the Console.

```JavaScript
print (sentinel_1);
```


7, How many images for the ascending pass?


8, Modify the code to explore the descending pass images, including the number of images available


### Select an image


9, Select the first descending pass image in the pack. The below code performs this task.

```JavaScript
var sentinel_1= sent1

//filter by descending orbit mode
.filter(ee.Filter.eq('orbitProperties_pass', 'DESCENDING'))

//filter by instrument mode; best to use IW = interferometric wide
.filter(ee.Filter.eq('instrumentMode', 'IW'))

//filter by a date range- wet season images only
.filterDate("2015-01-01", "2021-12-31")

//restrict the image acquisition date to specific months
.filter(ee.Filter.calendarRange(12, 3, 'month'))

//filter by study area; this means select scenes that overlap the study area
.filterBounds(darwin)

//select the VV and VH polarisation bands
.select(['VV', 'VH'])

//select an image---in this case the first image in the pack
.first();
```

Print the image metadata to the Console.

```JavaScript
print(sentinel_2, 'A descending pass image')
```



### Visualisation of Sentinel-1 SAR imagery


10,  Visualise the VV polarisation. The below code tackles this.

```JavaScript
Map.addLayer(sentinel_2.select('VV'), {min:-20, max:1}, 'Sentinel SAR')
```

11, Turn off the study area geometry and zoom in on Darwin city centre


### Validating the SAR using Google optical imagery

12, Turn on the 'Satellite' base map (see icon below) to interpret the SAR image



![image](https://github.com/user-attachments/assets/48acf018-ea8a-43f5-b296-cd8acc0ed4b3)









## DIY


Answer the following questions. Make sure you explain your observations for all questions <br>


13, What is the difference in brightness between water and land features? And why this difference? <br>


14,  Which of the land features show the highest brightness values? <br>


15, Does the SAR image information align with the Google satellite optical imagery? 




## Conclusion


In this practical we collected a Sentinel-1 SAR imagery, visualised and validated this using the Earth Engine’s optical satellite imagery. 





















