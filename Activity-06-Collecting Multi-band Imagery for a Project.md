# Actvity 6: Collecting Multi-band Imagery for a Project

## Introduction

In the previous chapter, we worked through single-band and multi-band imagery through a method that allows us to load our own (external) image into the Code Editor. In many real-life cases, you might be required to use data existing within the Earth Engine repository. The Earth Engine data catalogue contains many multi-band imagery collected using popular satellite sensors, including Landsat, Sentinel-2, and MODIS. While some imagery requires pre-analysis post-processing, others are ready for analysis. 
The satellite sensors would usually take a photo of the target at regular intervals, e.g., every six days. The repeat measurement of the target by the satellite means that a stack of images becomes available for a scene. Depending upon the satellite’s repeat time and length of time in operation, the volume of data can be greatly large. Although you might not need all of the data, including the entire scene, for a project, you may need to filter the catalogue to be sure you are using the data that meets your requirements. The chapter aims to show how to search data, filter data collection, and explore a stack of images.
The activity assumes you are able to produce true colour composite and false colour composite images.


## Learning Outcomes

At the end of the session, you should be able to:

- Explain image collection
- Define a region of interest 
- Filter image collection
- Visualise image collection


## Activity Begins

### Use Data in Earth Engine

Once you have created a new script file, the **Search place and datasets…** bar, shown below, is available to explore the data catalogue for data. 


![image](https://github.com/user-attachments/assets/80bcc647-a5bd-4852-971f-057cc0277332) |
|:--:|
| *Fig. 1. The Earth Engine search bar to explore data catalogue.*|


You can type a random name into the search bar to see what data is available, but your search becomes much easier if you know the name of the data and type this into the bar. In this activity, we will use the Sentinel-2 data. The Sentinel-2 is a satellite mission operated through the European Space Agency’s Copernicus Programme to provide global satellite imagery at high spatial and temporal resolutions. The Sentinel-2 mission comprises a constellation of two satellites (A and B) that function together to achieve a 5-day revisit period. Also, the Sentinel-2 has 13 spectral bands with varied native pixel sizes. The native pixel size for the Sentinel-2 band 2 (blue), band 3 (green), band 4 (red), and band 8 (near infrared) is 10 m, while the native pixel size for the other bands is either 20 m or 60 m. Further information about the Sentinel-2 might be found [here](https://www.esa.int/Enabling_Support/Operations/Sentinel-2_operations). 

Type ‘Sentinel-2’into the **Search place and datasets…** bar. Immediately, a window pops up with a list of rasters related to Sentinel-2. We would use the Sentinel-2 surface reflectance product (aka bottom of atmosphere) as this data has been corrected for atmopsheric effects and usually preferred for serious studies, but firstly let’s explore the Sentinel-2 Level-1C whose reflectance values might include reflectance from atmospheric constituents. This Sentinel-2 product is known as the Top of Atmosphere or Level-1C in Sentinel-2 parlance.

Under **RASTERS** click Harmonized Sentinel-2 MSI: MultiSpectral Instrument, Level-1C and click the BANDS tab to explore the band description. Your result should be as shown below. You must slide down the slider, highlighted with a red polygon in Figure 4.2, to view all the bands, including the quality assessment layers.



![image](https://github.com/user-attachments/assets/8591d960-c28e-4ccc-9e01-b6136c86a822) |
|:--:|
| *Fig. 2. The Sentinel-2 bands, including quality assessment layers.*|


Fig. 2 shows the Sentinel-2 band name, description, resolution, and centre wavelength. For example, the B1 describes a centre wavelength of 443.9 nm for S2A and 442.3 nm for S2B, which measures atmospheric aerosols. The pixel size for this band is 60 m. The pixel reflectance value for the thirteen bands are scaled using a factor of 0.0001.

Go back to **RASTERS** and click Harmonized Sentinel-2 MSI: MultiSpectral Instrument, Level-2A for the bottom of the atmosphere data. Make sure you select the BANDS tab; your results should be as Fig. 3. If you need to use this data, you would have to click IMPORT (see Fig. 3) to import a code into the Code Editor that will retrieve all the Sentinel-2A images in the catalogue. Alternatively, copy the snippet ee.ImageCollection("COPERNICUS/S2_SR") (Fig. 3) and use this in the Code Editor. 


![image](https://github.com/user-attachments/assets/056bd765-b081-4b19-86a9-f9d964b5effa)
|:--:|
| *Fig. 3. Import the Sentinel-2 Level-2A imagery.*|


In this activity, we copied the snippet and assigned to a variable sen2sr as shown below.

```JavaScript
var sen2sr = ee.ImageCollection("COPERNICUS/S2_SR_HARMONIZED")
```

The `ee.ImageCollection`, as the name suggests, creates a collection of all the Sentinel-2 bottom-of-atmosphere images, including both satellite 1A and 1B observations. Given Sentinel-2 data has been available since 2015 and the sensor has a repeat cycle of 5-10 days, you should be keen to know the total number of Sentinel-2A images available with the Earth Engine repository. You can do this by exploring the metadata using the `print` command, as shown below.

```JavaScript
//print the collection to the Console
print(sen2sr);
```

Run the print command. You may receive an error message, which suggests there are more than 5000 images in the collection. In Earth Engine, the maximum number of elements you can print to the **`Console`** is 5000. 


![image](https://github.com/user-attachments/assets/b8ec4020-c397-4f52-83ff-68a9270c2fa1)




Well, in many cases, you do not need all of the images in the image collection for a project. Thus, to avoid receiving this error message associated with printing such a large image collection to the Console is to filter the image. In filtering you select only the images you require. To retrieve the right images, you may filter the collection by date, region of interest, cloud coverage, and other image properties. Given that the analyst would almost always have a region of interest, we defined a region of interest using the drawing tools. The drawing tool is identifiable by the icons below.


![image](https://github.com/user-attachments/assets/93de6d68-5e93-40e3-bf8b-62a4c160df60)



The first icon from the left is not for drawing but for panning. Also, useful when you want to stop drawing or delete a geometry. The second icon is required for creating a point ROI, the third icon is required for a line ROI, the fourth icon is for creating polygons, and the fifth icon is for creating a square/rectangular ROI. We used the fourth icon to create a polygon ROI. Assuming Darwin, which is at the top end of Northern Australia is the study area you would navigate to the map of Australia and identify Darwin (at the top end of Northern Australia). Once you find Darwin, click on the polygon icon. Immediately, the **geometry** and **Polygon drawing** widgets become active; this is shown below.


![image](https://github.com/user-attachments/assets/eade8550-b0fe-477e-8fa6-e8d41146d4f2)


Also, in the **Code Editor**, a new variable for the geometry is added to the Imports items. The name of the variable is given a default name **geometry**, you can change this variable name. In this case, rename the variable to **roiDarwin**. The polygon tool is active, define a polygon for Darwin, as shown below. Note that the size of your polygon might be different to the one used in this activity.


![image](https://github.com/user-attachments/assets/f7736f3f-7166-49fd-8dac-6add7506151a)
|:--:|
| *Fig. 4. A polygon defined over Darwin, Northern Australia, using the geometry tool.*|


If you are not happy with a geometry you created you can delete this by selecting the hand icon and clicking this geometry once; the Editing widget would pop up, as shown below.

![image](https://github.com/user-attachments/assets/a4b3ce69-5329-46cb-b591-1c0a11e26b1f)



Now click **Delete** to remove the geometry as you do not need it. 
Once you have defined an ROI for your study area and know the period you want to investigate, you can go back to the code and filter the collection. This might be a solution to the error message regarding printing all the elements in the image collection to the **`Console`**.
To filter a collection by date, varied methods can be used. In this activity we used `filterDate()`. Remember, you can find this method and the arguments required in the Docs tab as shown below. The `filterDate()` is highlighted with a red polygon.




![image](https://github.com/user-attachments/assets/46f7d93f-0cd3-470d-9e76-2163db71f21e)
|:--:|
| *Fig. 5. The Docs tab showing the filterDate() method for an image collection.*|



The `filterDate` requires two string arguments, which are a **start date** and **end date**. For start date we specified **‘2022-01-01’** and end date is **‘2023-12-31’**. Next, filter the collection using the ROI. This is done using the `filterBounds`, which requires **geometry** as the only input argument (Fig. 5). Using these pieces of information, we can modify the code to be as shown below.


