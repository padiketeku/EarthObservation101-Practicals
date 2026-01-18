# Activity 1: Google Earth Pro

## Introduction

Google Earth Pro (GEP) is a free desktop software that enables the viewing of a large set of remotely sensed imagery and other spatial data that has been made freely available. GEP has recent and historical imagery of landscapes worldwide, making it a valuable spatial database. The GEP images can be multisource, combining low and high-resolution data to achieve wall-to-wall coverage. High-resolution images are typically available for urban locations with large populations. The GEP has functionalities and tools for importing and exploring vector data (including waypoints, tracks, and routes from a global positioning system unit) and raster data, as well as creating attribute tables. GEP allows users to connect their GPS unit for real-time visualisation or the collection of spatial data. With GEP, users can add features such as placemarks, paths, and polygons. These capabilities make GEP an important resource for environmental and geospatial scientists. GEP provides a quick reference for visualising an area of interest, allowing an overview of the landscape features. GEP is particularly valuable during the planning stages of projects requiring fieldwork and for collecting reference data to calibrate and validate land cover maps.
Additionally, GEP offers users the opportunity to explore historical imagery of the landscape, enabling easy observation of environmental changes. Despite the benefits GEP offers, its images are for only visualisation; users cannot download them for analysis. Moreover, GEP lacks an environment for processing and analysing spatial data. Users must resort to other geospatial software, such as Google Earth Engine and ENVI, to apply the spatial data quantitatively.


## Learning Outcomes


At the end of this activity you should be able to:
- Download and install Google Earth Pro if you do not have this already
- Google Earth’s 3D viewing system
- Change image coordinates
- Explore historical imagery



## Activity

### Install GEP

1. Open your web browser and go to https://www.google.com/earth/versions/. <br>

Scroll to the bottom of this page and click “Download Earth Pro on Desktop” (see the figure below for a guide), and install the latest version of Google Earth Pro on your computer. 



<img width="1540" height="640" alt="image" src="https://github.com/user-attachments/assets/4974e9f8-c8fe-4932-b2e0-81f1a97f4ee6" /> <br>


### The 3D viewing <br>

Once you have the GEP installed, open it to see the Earth in 3-dimensions. Your result may be like this:





<img width="1939" height="1041" alt="image" src="https://github.com/user-attachments/assets/d1e60e26-08d0-4550-878a-97350ac21f8d" /> <br>





The software is easy to use with rapid and intuitive navigation. Explore its functionality by navigating to different locations on Earth, zooming in and out, and changing the aspect. The 3-D view is an unprojected viewing system that mimics what you would see if you were actually in the air or in space above the ground. At a tiny scale, when you can see the whole Earth, for example, its curvature becomes apparent. At larger scales, when you zoom into an area with some significant topographical features (hills or mountains), the terrain clearly exhibits a 3D effect, especially if you move the image around with the mouse.
A flat map, on the other hand, typically uses a projection that flattens Earth's curvature, leading to spatial distortion. The smaller the scale of the map, the larger the level of distortion. Flat maps will use contour lines to represent topography.



### Change image coordinates <br>

The image coordinates are at the foot of the Google Earth application window. For unprojected imagery, it is best to use geographic coordinates, i.e., longitude and latitude. From the menu, select Tools -> Options... In the 3D View tab there is an option box that allows you to switch between coordinate systems. We will be using Decimal Degrees. You can change to Minutes and Seconds or Decimal Minutes options; these are similar to Decimal Degrees, but which split each degree up into 60 minute intervals, and, in the case of the Minutes and Seconds option, further split each minute into 60 seconds. You may still see coordinates given in these systems, but the decimal degrees system is becoming the standard.

Another option is “Universal Transverse Mercator” or UTM for short. This is a projected coordinate system better suited for when you are dealing with a smaller area and is often used in relatively larger-scale maps. Select this option and then OK. The coordinates at the foot of the Google Earth window will change. Its units are in metres and are given in Northings and Eastings. In the UTM system, there are 60 longitudinal zones. The zone number is given at the start of the coordinates. Make sure you turn on the "Grid" by going to Menu, select "View", and tick "Grid". This displays the Northings and Eastings.

What zones does Australia take in? You are right, 49 through to 56.

The letter to the right of the zone number indicates the latitudinal band starting at C at 80°S to X, which extends to 84°N.
What latitudinal bands does Australia take in? G through L?

Crossing over boundaries between zones or bands can cause problems with the UTM coordinate system and it is often better to revert to geographic coordinates if this is the case.
Google Earth uses the WGS84 geodetic datum, the coordinate origin is at the centre of the Earth and is therefore used worldwide. Tectonic plate movement, however, means that Australia, like other continents, moves slightly relative to the centre of the Earth (about 70 mm per year NNE). Local datums are therefore in use across the world that have a continental reference point, which increases accuracy and is very important when, say, you need cm accuracy to perfectly ascertain property boundaries. In Australia, we currently use the GDA2020 datum for increased accuracy. The absolute current difference between GDA2022 and WGS84 is negligible, so for most applications, either datum can be used.

The available image data and the level of analysis in Google Earth are limited.

Google Earth only displays “true colour” imagery, i.e., imagery that is equivalent to how we would see the real world with our eyes if we were flying over. Imagery using wavelengths of electromagnetic radiation outside the visible part of the spectrum greatly extends the range of remote sensing products, but is not available through Google Earth. The image data are sourced from various organisations, which are listed in the centre of the image being viewed; several image sources can be used for any one scene. Note how they change when you zoom in and out to different sets of imagery with different spatial resolutions.

The spatial resolution of the imagery varies depending on the data that has been made available. Where you can see fine detail, the imagery has a high spatial resolution. You can see individual trees, cars, and even people. Most populated areas now have high-resolution imagery. For example, Darwin (type Darwin into the Search box), is covered by imagery with a very high spatial resolution. See the figure for a high-resolution GEP imagery of Darwin.




<img width="1282" height="906" alt="image" src="https://github.com/user-attachments/assets/446cf7c7-0861-495e-a6fa-a51ee2e97027" />






The temporal aspect of the data is limited to what has been historically made available by Google. Generally, the older the dataset, the lower the spatial resolution. This historical data availability remains a valuable feature for tracking changes over time; however, the user has limited control over which time periods can be viewed. To view historical data in Google Earth, click the icon in the toolbar that has a clock with a green arrow above it.


### Explore historical imagery

Visualisation of landscapes over time is possible in GEP through the historical imagery. Click <img width="41" height="38" alt="image" src="https://github.com/user-attachments/assets/e25f99ad-0894-4001-b935-ad859c927c56" /> to bring up a time slider, which may look like this: <img width="338" height="63" alt="image" src="https://github.com/user-attachments/assets/fec9e0b5-7391-4de2-9187-1b45fa7eace7" />

Locate Katherine and use the slider (slide back and forth) to view older imagery. Note the generally lower spatial resolution of older imagery. You can also view changes in the landscape of Palmerston city liding through the various images. Below is the images of Palmerston city in 2006 (left image) and 2025 (right image). Spot the differences in the land cover or use.



<img width="1929" height="625" alt="image" src="https://github.com/user-attachments/assets/0b78f98f-dad2-444a-aeeb-a2aac6b6a90a" />








<br> No image processing functions are available in Google Earth. Many of the manipulations of remote sensing imagery that are necessary for remote sensing image processing and analysis are not present. We will utilise Google Earth Engine for the image processsing and analysis. 







