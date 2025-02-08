# Activity 4: Visualization of Single-band Imagery

## Introduction
Light, mainly from the sun, is used by remote sensing detectors, including the human eye, to create pictures of the environment. In Remote Sensing, light, an electromagnetic radiation, is described using a range of wavelengths referred to as **band**. Solar radiation and the cone and rod cells of the human eye make objects in our environment visible to us. The human eye has blue, green, and red cone cells making humans see objects around as red, green, blue (RGB), or a mixture of the RGB in a good lighting condition. At night or in a low-lighting environment, the rod cells are more active, and thus, we see objects in shades of black and white (i.e., greyscale). 


![image](https://github.com/user-attachments/assets/aef50f75-2c68-44b0-8660-82b7abee7886) |
|:--:|
| *Fig.1 The human eye as a natural remote sensing detector through the cone and rod cells of the retina. The blue cone cell, green cone cells, and red cone cells give humans colour vision. The rod cells are only turned on at night or in a poor lighting environment to visualise objects in a black-white range.*|

A mechanical remote sensing detector replicates human vision in that solar radiation with a wavelength ranging between 400-700 nm is employed for humans to capture objects in RGB and/or a mixture of the RGB. 


## Learning Outcomes

- Create a new script file
- Search through the Earth Engine repository for data
- Load an image into the Code Editor
- Visualise the imagery
- Use the Inspector tool to understand data
- Differentiate between pseudo-colour image, true colour image, and false colour image
  

### Create a new script file
To start a new script, click the `Scripts` tab, **NEW** (the drop-down triangle), and select **File**. Fig. 2 shows the steps.

![image](https://github.com/user-attachments/assets/53d233fa-5bdc-4d01-b716-74f0daf30fcc) |
|:--:|
| *Fig. 2. Selecting a new script file.*|

A `Create file' window pops up for you to specify the file name. In this example, the file name is prac1002. Make sure new script files are saved to the same repository. Once the file is created you would see it (highlighted in grey) under the repository, as shown below (Fig. 3).


![image](https://github.com/user-attachments/assets/672e686f-1491-4200-937a-3482a5bd75e1) |
|:--:|
| *Fig. 3. A new script is created and visible in the repository.*|

If you are not happy with a script file you created you can delete this using the delete icon highlighted with a red cross mark in Fig. 3. You can rename the file using the `Rename` command, the icon just before the delete icon.
Click the new script file, `prac1002`, and a fresh Code Editor would be available. 


### Search through the Earth Engine repository for data

To find data from the Earth Engine repository for a project you would use the `Search places and datasets…` bar, shown below.

![image](https://github.com/user-attachments/assets/aa0eef08-a187-48db-91d1-9f670e53bb76)

The search bar can be used to search places and datasets, but in this activity, we focused on datasets, specifically images (also known as rasters in Earth Engine). For instance, if you are interested in precipitation data, you may type precipitation into the search bar to see a list of precipitation data in the repository. In this activity, we explored elevation data given by [NASA](https://science.jpl.nasa.gov/projects/srtm/) and [USGS](https://www.usgs.gov/centers/eros/science/usgs-eros-archive-digital-elevation-shuttle-radar-topography-mission-srtm). Type SRTM into the `Search places and datasets…` bar, the result would be a list of rasters and tables data. Click **NASA SRTM Digital Elevation 30m** to view the result, as shown below (Fig. 4). Note that you must click the `BANDS` tab as it is not the default tab. Your result must be the same as the figure below.

![image](https://github.com/user-attachments/assets/3459956a-164e-4aac-b53a-da5314a60309) |
|:--:|
| *Fig. 4. The band description of NASA SRTM digital elevation.*|


The default tab is the DESCRIPTION tab, which gives detailed information about the data. The next tab is the BANDS tab, which lists the number of bands in the image plus other information about a band. In this example, the image has a single band, which provides global elevation data. The estimated minimum and maximum elevation values are -10 and 6500 m, respectively. The details of the **Dataset Provider** and **Data Availability** are given. 


### Load image into the Code Editor

You can load an image from the Earth Engine Data Catalog or your own image in Earth Engine Assets (this will be discussed in a later chapter) to the Code Editor in multiple ways. In this activity, we used the image constructor, `ee.Image`, which accepts various arguments (Fig. 5), including a string indicating the image file ID.

![image](https://github.com/user-attachments/assets/9a23a3c4-a9d3-44e4-9bb7-bdfef27e1e7a) |
|:--:|
| *Fig. 5. The image constructor and the valid arguments.*|

We continued to use the NASA elevation data by copying the collection snippet (Fig. 6). Return to the Code Editor and paste the snippet. 

![image](https://github.com/user-attachments/assets/28c5d1e9-9a29-42b9-b9b3-c7b29de0668c) |
|:--:|
| *Fig. 6. The image constructor to retrieve the SRTM data.*|

Assign the snippet to a variable; and call this strm_elevation, as shown below.

```JavaScript
var srtm_elevation = ee.Image("USGS/SRTMGL1_003");
```
Click **Run** to run the code, but nothing will be available in the **`Console`**. Print the variable.

```JavaScript
print(srtm_elevation);
```
Re-run the code and pieces of information about the data will be available in the **`Console`**. Fig. 7 shows the result.

![image](https://github.com/user-attachments/assets/1b5a0b9e-94ff-4f92-9568-b4e4688fddff)|
|:--:|
| *Fig. 7. A metadata of the SRTM elevation data.*|


You may need to click the expander arrows to explore the full metadata (i.e., information given about the data). The **data type** is image with data ID and version given. The **bands** information, including knowing the number of bands and band names, is useful for visualisation purposes. The coordinate referencing system (**crs** and **crs_transform**) is given to understand how objects have been displayed in two dimensions. The **dimensions** show the number of pixels column-wise and row-wise. The **properties** provide more details about the data, including the minimum and maximum elevation values for visualisation. Make sure you explore all items under properties. 


### Visualisation of a single-band image

A digital image is made of a row and column of picture elements (i.e., pixels). Each pixel has a brightness value (also known as digital number) which frequently represents the intensity of return energy recorded by the detector but may also represent temperature, land cover type, or elevation. Rather than using a multi-band (e.g., RGB) detector, it is possible that a single-band sensor can be used to collect information about the target, in which case the target will appear as a grayscale image (just as human vision from the rod cells) with brighter and darker pixels. The brighter pixels of the image have high brightness values, while darker pixels have low brightness values. 

![image](https://github.com/user-attachments/assets/e6a73ffb-fa00-4983-847c-19f2de49b2fb) |
|:--:|
| *Fig. 8. Rows and columns of pixel brightness values for a single band gray-scale image. The remote sensing system used to generate this data is 8-bit (i.e., 28), so the brightness values can range between 0 (black pixel: lowest intensity possible), and 255 (white pixel: highest intensity possible).*|


The single-band image can be displayed on a computer specifying/using the minimum and maximum brightness values, so all other pixel values are spread within this range to achieve a degree of contrast. The minimum and maximum pixel value can be obtained from the metadata.

In this activity, we will continue to work through the NASA elevation data by displaying this to the Earth Engine base layer. 

`Map.addLayer` API (Fig.9) is the required to display a layer to the base map.

![image](https://github.com/user-attachments/assets/3ed6a36e-7890-485c-999c-42f869c0aab7) |
|:--:|
| *Fig. 7. The Map.addLayer API.*|


The `Map.addLayer` is the work horse for data visualisation and requires an Earth Engine object as an argument. The other arguments are optional, and the function returns a new map layer. In this example, for the optional arguments we specified the **visParams** and **name** to the achieve the code below. Through the **visParams** the range of pixel value is specified, so the computer can display the image features in a range of black and white (i.e., grayscale).

```JavaScript

//Add the layer to the base map
Map.addLayer(srtm_elevation, //this is the eeObject

{//visParams
bands:['elevation'], //the band to display; we used [] to show that the band is from a list
min: -10,
max: 6500 //the min-max gives the data range: values were sourced from the image properties
},

//name of the layer
'STRM Elevation 1'
);
```
Click **Run** to run the script; you may need to zoom out the base map to see the grayscale image of the elevation data. Darker pixels connote low elevation while lighter pixels mean high elevation areas. In the base map panel find the layer name under the widget **Layers**. This is a global data but zoom in on Australia and identify the high elevation areas. 
It is possible to have an image with low contrast, so you might need to manipulate a grayscale image for visual enhancement. You can do this by changing the transparency or contrast of the image or applying a colour palette (aka look up table) to groups of pixels. The transparency of the image can be modified manually or programmatically. For the manual approach, hover the mouse over the **Layers** widget and you will see a slider by the layer name. Slide the slider to see how the transparency of the map layer changes in the base map. Programmatically, you can alter the layer transparency, as shown below. The code is similar to the one above, but the ‘shown’ and ‘opacity’ arguments are now specified. Also, a new layer name is specified (i.e., ‘SRTM Elevation 2’).

```JavaScript

//Add the layer to the base map
Map.addLayer(srtm_elevation, //this is the eeObject

{//visParams
bands:['elevation'], //the band to display; we used [] to show that the band is from a list
min: -10,
max: 6500 //the min-max gives the data range: values were sourced from the image properties
},

//name of the layer
'STRM Elevation 2',
1, //it is a boolean: 1=show map layer, and specify 0 if you do not want to dsiplay the map layer
0.7 //opacity value; this ranges between 0 (transparent) and 1(opaque)
);
```
The opacity value is set to 0.7. You will have two map layers in the **Layers** manager, uncheck the bottom layer to evaluate the impact of the new opacity value. 
Contrast enhancement. We will focus on Australia to explain min-max contrast enhancement. The data shows that in Australia the highlands are concentrated in south-east. However, the contrast is low for west Australia. This is possible if the data is naturally skewed to a range. To improve image contrast, you must specify the min-max values to this natural range capturing the more useful information while leveraging the dynamic range of the computer. Specifying the natural range of the data so is stretched to align with the dynamic range of the computer is referred to as *min-max contrast enhancement*. To do this, change the min-max values to 0 and 1000, respectively, as Australia is generally flat. Label the layer ‘SRTM Elevation 3’. The code snippet is shown below.

```JavaScript

//Add the layer to the base map
Map.addLayer(srtm_elevation, //this is the eeObject

{//visParams
bands:['elevation'], //the band to display; we used [] to show that the band is from a list
min: 0,
max: 1000 //range specified to suit a region of interest (Australia in this case)
},

//name of the layer
'STRM Elevation 3'
);
```

The image contrast is enhanced, revealing the variability in elevation, especially in West Australia. The result can be seen below. 

![image](https://github.com/user-attachments/assets/b8628737-db0d-4c8b-a44f-9025154134a2) |
|:--:|
| *Fig. 8. The NASA SRTM elevation data of Australia. This image is enhanced using the min-max contrast method. The darker pixels are low elevation while the brighter pixels are high elevation.*|

Compare this result (Fig. 8) and the ‘SRTM Elevation 1’ layer to describe the impact of contrast enhancement.


### Create a pseudo-colour image
The human eye is more sensitive to features displayed in RGB than black/white. To visually enhance a grey-scale image, the native colour can be manipulated using RGB palette to produce a **pseudocolour image**. A pseudocolour image is a single band but a brightness value is sent to three different lookup tables (i.e., RGB palette), usually resulting in a rainbow colour (violet, indigo, blue, green, yellow, orange and red). This is achieved by slicing up the pixels of the single-band image into user defined groups and assigning unique RGB to each group of pixels (Fig. 9). 

![image](https://github.com/user-attachments/assets/e8ab05f5-8b89-4085-8f72-ea0676417ad7) |
|:--:|
| *Fig. 9. Steps to produce a pseudocolour image. The input grayscale image is sliced up into groups with each group assigned to a discrete colour to produce a pseudocolour image.*|

In Earth Engine, a colour palette is specified to produce the pseudocolour image. The following code specifies a palette with three colours (i.e., RGB). The output image would be interpreted as low elevation being blue and red for high elevation. 

```JavaScript

//Add the layer to the base map
Map.addLayer(srtm_elevation, //this is the eeObject

{//visParams
bands:['elevation'], //the band to display; we used [] to show that the band is from a list
min: 0,
max: 1000, //range specified to suit a region of interest (Australia in this case)
palette:['blue','green', 'red']// assigning a palette; low elevation areas would be blue while high elevation would be red
},

//name of the layer
'STRM Elevation 4'
);
```
Run the above code to produce the pseudocolour image below. Note that we have zoomed in on Australia for emphasis. You can zoom in on any continent or country of your preference.

![image](https://github.com/user-attachments/assets/828999d7-b87c-408f-b437-05d267da6f63) |
|:--:|
| *Fig. 10. A pseudocolour image displaying the NASA SRTM elevation data. The region of interest is Australia. The blue pixels are low elevation areas and red pixels are high elevation areas. Green is the middle range of elevation values.*|

Zoom in on Australia, you may notice that high-elevation areas are in red while low-elevation areas are in blue.

****The Inspector Tool
You can further explore the displayed map layer using the `Inspector` tool, the tab left to the **`Console`** tab. When you are in the `Inspector` tab, click anywhere of the map or a region of interest to view information on the selected location: Point, Pixels and Objects (Figure 2.14). Note that your selected location may be different to the one used in this book, so it is OK if your result is not same as reported here. 

![image](https://github.com/user-attachments/assets/398c5b33-4dfe-48e7-ba4e-16fd1367b830) |
|:--:|
| *Fig. 11. The Inspector tool reporting elevation data about a selected location.*|

**Point** provides the longitude and latitude of the cursor location (i.e., your ROI) plus the zoom level and scale of the displayed map layer. **Pixels** gives attributes about the pixel, and in this example, if you expand the arrow, the layer name (including the data type and number of bands) and the elevation of the selected location are displayed. **Objects** provides data about the source dataset.


### DIY
Using the NASA SRTM data, produce a pseudocolour image with a rainbow palette: violet, indigo, blue, green, yellow, orange, and red. Compare your result with the one in Fig. 10 and share your observation on the discussion forum.


## Conclusion
Up to this point, you have learned about single-band image visualisation, including enhancement techniques such as modifying transparency, min-max contrast stretching, and pseudocolour image. Next activity, we will focus on the visualisation of a multi-band image using a true colour band combination and a false colour band combination.







