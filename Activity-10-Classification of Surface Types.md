# Activity 10: Classification of Surface Types

Usually, there are many surface types within a remotely sensed imagery: water, vegetation, baresoil, built-up areas, roadss, etc. Despitee the varied surface types, users may not be be interested in all of these surface types. In fact, a map user would be more interested in just one surface type and would like to know where in the image this surface type is located. Because of this, making a thematic map out of the imagery would be more helpful for the user. In this activity thematic map was created using image classification methods, including k-means clustering and minimum distance. The activity assumes you have covered all the activities preceding this one, especially activities 7 and 8, and thus participants can create feature collections.

## Introduction
Image classification is collecting similar pixels into a group or theme. For instance, grouping all water pixels under a theme. The pixels are grouped according to their spectral similarity and taking into account spectral distance between pixels. The pixel grouping can be done by the computer or the analyst teaching the computer to do this. If the classification of pixels is predominantly performed by the computer this is described as **unsupervised** while where the analyst teaches the computer to perform the classification is referred to as **supervised**. The activity explores k-mean clustering and minimum distance to explain unsupervised and supervised classifiers, respectively.


## Learning Outcomes

At the end of this activity, you should be able to: <br>

- collect point features
- perform k-means clustering (unsupervised classification)
- perform minimum distance classification (supervised technique)
- visualisation classification map
- export the classification map to google drive




## Activity Begins

### Task

You are required to map the cleared-land, burnt-land, forest, farm-land, water, and mines. 

|:--:|
| *Fig. 2. A histogram for an NDVI layer.*|



### Supervised classification: minimum distance classifier

#### Sample surface types using point feature collections create

   In real-world practice the analyst conducts field surveys to identify the various dominant surface types in the study area. GPS is used to collect the locations of the sampled land cover or land use. In the absence of field data, a higher resolution image can be used for desktop sampling of the land cover types. The latter approach was used in this activity. The **geometry tool** was used to creat point feature collections for the surface type. The more samples the better, but in the interest of time and for demonstration purposes 20 pixels were sampled for each surface types. Possibly, spread out the samples across the image. Do not concentrate the points to a region. 

   - Sample farmland, forest, water
     Visualise the image as true  colour composite. Waterbodies appear dark, randomly sample 20 pixels using the "Add a marker" tool. This is the tear-drop icon in the geometry tool. Hint, forest pixels are usually dark green with rough texture, while farmlands are bright/smooth green patchwork.

     
   - Sample clear-land, burnt-land, mines

     Visualise the image as a false colour by combining band 12 (SWIR), band 8 (NIR) and band 4 (Red) to accentuate burnt-land and bare-land. Sample surface types.

     After the edits, the **Geometry Imports** and base map you should be similar to the one in the figure below.


     ![image](https://github.com/user-attachments/assets/62d3f3a4-d0d1-4731-ae93-308f91bea2c6)

     

## DIY




## Conclusion
