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

