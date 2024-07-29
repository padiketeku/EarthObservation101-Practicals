# Activity 4: Visualization of Single-band Imagery

## Introduction
Light, mainly from the sun, is used by remote sensing detectors, including the human eye, to create pictures of the environment. In Remote Sensing, light, which is electromagnetic radiation, is described using a range of wavelengths referred to as a band. Solar radiation and the cone and rod cells of the human eye make objects in our environment visible to us. The human eye has blue, green, and red cone cells making us see objects around us as red, green, blue (RGB), or a mixture of the RGB in a good lighting condition. At night or in a low-lighting environment, the rod cells are more active, and thus, we see objects in shades of black and white (i.e., greyscale). 

![image](https://github.com/user-attachments/assets/aef50f75-2c68-44b0-8660-82b7abee7886) |
|:--:|
| *Fig.1 The human eye as a natural remote sensing detector through the cone and rod cells of the retina. The blue cone cell, green cone cells, and red cone cells give humans colour vision. The rod cells are only turned on at night or in a poor lighting environment to visualise objects in a black-white range.*|

A mechanical remote sensing detector replicates human vision in that solar radiation with a wavelength ranging between 400-700 nm is employed for humans to capture objects in RGB and/or a mixture of the RGB. 


![image](https://github.com/user-attachments/assets/68ddccbb-f337-48f3-929d-6f9f09a7fd62)




## Learning Outcomes
- Create a new script file
- Search through the Earth Engine repository for data
- Load an image into the Code Editor
- Visualisation of images
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
print(srtm_elevation );
```
Re-run the code and pieces of information about the data will be available in the **`Console`**. You may need to click the expander arrows to explore the full metadata (i.e., information given about the data). The data **type** is image with data ID and version given. The **bands** information is crucially important for various reasons, including knowing the number of bands and band names which is useful for image visualisation purposes. The coordinate referencing system (**crs** and **crs_transform**) is given to understand how objects have been displayed in two dimensions. The **dimensions** show the number of pixels column-wise and row-wise. 


