# Activity 14: Classification of Surface Types: Supervised Method
Usually, there are many surface types within a remotely sensed imagery- water, vegetation, baresoil, built-up areas, roads, etc. Users may not be be interested in all of these surface types. In fact, a map user would be more interested in just one surface type and would like to know where in the image this surface type is located. Because of this, making a thematic map out of the imagery would be more helpful for the user. In this activity thematic map was created using image classification methods, including k-means clustering and CART. The activity assumes you have covered all the activities preceding this one, especially activities 7 and 8, and thus participants can create feature collections.

## Introduction

Image classification involves grouping similar pixels into a theme or category, such as grouping all pixels representing water under a single theme. The pixels are grouped according to their spectral similarity, taking into account the spectral distance between pixels. The pixel grouping can be performed by the computer or the analyst, who teaches the computer to do this. If the computer predominantly performs the classification of pixels, this is described as **unsupervised**. In contrast, when the analyst teaches the computer to perform the classification, it is referred to as **supervised**. Image classification, whether supervised or unsupervised, can either be pixel-based or object-based. The amalgamation of similar pixels into objects constitutes the object-based approach, which is beyond the scope of this unit. We will only cover pixel-based image classification, specifically exploring k-means clustering and decision trees (i.e., CART and Random Forest) to explain unsupervised and supervised classifiers, respectively. 

## Learning Outcomes

At the end of this activity, you should be able to: <br>

- understand the land cover classification systems
- manually sample and label pixels
- perform CART classification (supervised technique)
- perform Random Forest classification (supervised technique)
- qualitatively assess a classified image (via image visualisation)
- export classified iimagery to Google drive




## Activity Begins

### Task

A Sentinel-2 image surface reflectance product obtained from the Earth Engine catalog is given as: **COPERNICUS/S2_SR_HARMONIZED/20240428T013701_20240428T013722_T52LGL**. A client interested in identifying cleared lands would like to understand the dsitribution of the dominant land cover classes. Classify the image into the six (6) major land cover classes from the FAO Land Cover Classification Systems (LCCS) ([Gregorio and Jansen, 2000](https://www.fao.org/4/x0596e/X0596e01f.htm#TopOfPage)), described by the table below. Make sure your class labels align with the one adapted by Australia. The polygon delimiting the study area is given by this list of coordinates: [[[130.85, -13.39],[131.29, -13.40],[131.31, -12.76],[130.86, -12.77]]]



### Standard cover classes

Reference data obtained through field surveys or higher resolution imagery are critically important for image classification tasks. The reference data Labelling surface types can be easy, especially if you are familiar with the study area or relying on existing data from experts. Nevertheless, the labeling process can highly subjective and usually difffer between places and persons, potentially causing vagueness and ambiguity. To remove or minimise issues related to the subjectiveness of labelling, the FAO proposed the Land Cover Classification Systems (LCCS) ([Gregorio and Jansen, 2000](https://www.fao.org/4/x0596e/X0596e01f.htm#TopOfPage)) for country to follow as much as practicable. This is meant to standardise the labels we assign to surface types and hence creating land cover classes that are observable by satellites. The LCCS standardises land cover and land use and change mapping, and it is a good practice to follow.

Countries can modify the LCCS to align with their environments, but the variation should be minor. In Australia, the LCCS is modified to reflect their physical conditions. For instance the Digital Earth Australia Land Cover used the Australian-specific LCCS. See the details in this [link](https://knowledge.dea.ga.gov.au/data/product/dea-land-cover-landsat/?tab=description), also available below. Read through the descriptions under "Classifications of Level 3", as we will apply this Australian-specific LCCS in the given task.
 

**Cultivated Terrestrial Vegetation (CTV)**

Cultivated Terrestrial Vegetation (CTV) is associated with agricultural areas where active cultivation has been observed. In version 2.0, only herbaceous cultivation is shown and describes vegetation of strongly varying cover, ranging from bare (e.g. ploughed) areas to fully developed crops. Whilst the continental product describes land cover, interpretation is complicated as the same terminology is used to report on land use.

The definition of cultivated, and the difference from natural or semi-natural land covers, can be contentious, particularly as much of the Australian landscape is used for agricultural food production. This includes areas of Natural Terrestrial Vegetation (NTV) and Natural Aquatic Vegetation (NAV) that are grazed by stock and which can be regarded as either semi-natural or cultivated.

CTV in the DEA Land Cover map is associated with areas where management practices aimed at cultivation (including for grass production) are actively performed during the year being shown. These practices include crop planting and harvesting, fertilisation, and ploughing. These practices often lead to highly dynamic spectral signals within and between years but also regular transitions between vegetation of different cover amounts as well as bare soil. This also means that agricultural areas will transition between natural and cultivated covers as management practices transition an area between actively cropped or grazed, to areas left fallow, areas reduced to low cover due to climate effects such as drought, or to other covers depending on what the predominant conditions are through the year being shown.

**Natural Terrestrial Vegetation (NTV)**

Natural Terrestrial Vegetation (NTV) represents areas that have all or most of the characteristics of natural or semi-natural herbaceous or woody vegetation (based primarily on floristics, structure, function, and dynamics). These areas are identified as primarily vegetated, with either a photosynthetic vegetation fraction (PV) or non-photosynthetic fraction (NPV) greater than the bare soil fraction (BS) for at least two consecutive months. This approach considers that vegetation can exist in, and transition between PV and NPV states during the year. In effect, this approach classifies the landscape as primarily vegetated where the vegetated fraction of a pixel is greater than 30 %. Where the proportion of the landscape is less than 30 % vegetated, it is regarded as a sparsely vegetated or Natural Bare Surface, but the cover proportions can still be quantified. Urban areas that are vegetated (e.g. suburbs with trees) are associated with NTV if the pixel is at least 30 % vegetated but as Artificial Surfaces (AS) otherwise. The implementation allows areas of semi-natural vegetation (e.g. native grassland and pastureland) to be included in the NTV class.

**Natural Aquatic Vegetation (NAV)**

Natural Aquatic Vegetation (NAV) is associated primarily with wetlands that are dominated by woody or herbaceous vegetation (as with NTV). NAV is generally associated with swamps, fens, flooded forests, saltmarshes, or mangroves. Only mangroves are included in the current release.

**Artificial Surfaces (AS)**

Artificial Surfaces (AS) are areas of non-vegetated land cover created by human activities and are primarily represented by impervious surfaces (e.g. urban and industrial buildings, roads, and railways). These can be more readily identified when the area is larger than the spatial resolution (30 m) provided by the sensor. Open cut extraction sites are often included in AS. However, there is considerable misclassification of NS as AS in areas where vegetated cover is very low and very consistent through the year.

**Natural Bare Surfaces (NS)**

Natural Bare Surfaces (NS) are comprised primarily of unconsolidated (often pervious, e.g. mudflats and saltpans) or consolidated (e.g. bare rock or bare soil) materials. In Australia, the proportional area of Natural Bare Surfaces is relatively low and primarily confined to the deserts and semi-arid areas, river channels (e.g. dry riverbeds) and the coastline (e.g. mudflats and sand dunes). Much of the interior of Australia is sparsely vegetated and can be dominated by herbaceous (annual or perennial) or woody lifeforms.

**Water**

The Water class captures terrestrial and coastal open water such as dams, lakes, large rivers, and the coastal and near-shore zone.




### Workflows

#### Define the bounds for the study aarea

```JavaScript
var projectArea = 
    ee.Geometry.Polygon(
        [[[130.85, -13.39],
          [131.29, -13.40],
          [131.31, -12.76],
          [130.86, -12.77]]]);
```



#### Prepare the imagery for the task

```JavaScript
var s2 = ee.Image('COPERNICUS/S2_SR_HARMONIZED/20240428T013701_20240428T013722_T52LGL')

//select the required bands
var s2 = s2.select(["B2","B3","B4","B5","B6","B7","B8","B11", "B12"])

```


#### Select the required bands

```JavaScript
var s2 = s2.select(["B2","B3","B4","B5","B6","B7","B8","B11", "B12"])
```


#### Trim the S2 image to the study area and display it as a true colour composite

```JavaScript
var s2ProjectArea = s2.clip(projectArea)
```

#### Sample surface types using point feature collections 

   In many real-world applications, the analyst conducts field surveys to identify the various dominant surface types in the study area. GPS is used to collect the locations of the sampled land cover or land use. In the absence of field data, an image (usually a higher resolution) can be used for desktop sampling of the land cover types. This approach was used in this activity, combining the Sentinel-2 imagery and high resolution Google satellite base imagery obtained from Airbus and other sources . <br>
   
   We would collect sample pixels or points for the cover classes in the imagery using randomisation approach. This is to ensure every pixel has equal chance of selection, and once the pixels are selected the analysts or experts (in this case you) will manually label them. However, before we start the random sampling of reference pixels or points, let's first visualise the image as a true colour composite.  Your results may be similar to the one below. The Google base layer is not part of the image; it's just a background map. 

##### Visualise the true colour composite of S2 imagery

   ```Javascript
Map.addLayer(s2ProjectArea, {bands:["B4", "B3", "B2"], min:200, max:2000}, "S2 TCC Project Area")
```



    
     

<img width="483" height="486" alt="image" src="https://github.com/user-attachments/assets/fb6cda25-916f-4dd9-93f1-a3bb6215bb46" />




##### Explore the image to determine the dominant land cover classes

Zoom in and out, explore the image to identify the different surface types. You may see water, forest, roads, mines, cropping lands, burnt land, and bare ground. Let's categorise these surface types into the six major land cover classes. You may notice that the study area is dominated by NTV. However, you may also see CTV in the top north, scattered waterbodies, NS, and AS. Once you understand the surface types and cover classes in your study area, collect reference features and label them.


##### Randomly select reference points for the classes

Given the size and heterogeneity of the study area, I would recommend 500 pixels be randomly sampled. However, in the interest of time, only 120 pixels were selected to complete the demonstration. Feel to sample more than 120 pixels if you want to improve the accuracy of the model. 

```JavaScript
//note, the s2ProjectArea is the image

var samp1 =s2ProjectArea.sample({
  region:projectArea, // this is your study area or region of interest
  scale:20, //this is the spatial resolution of the S2
  numPixels:120, // total number of pixels to sample, change this to 500 if that's what you want to do
  seed: 111, //randomisation seed to ensure the results are consistent regardless of the many times you run the script. The 111 can be changed to any number at all of your choice. Only make sure you keep to this number for this line of the script 
  geometries: true}) //set this to have the lat/lon of the selected points

//print the result
print(samp1, 'samples 1') //the result is a feature collection of points

//visualise the selected points/pixels
Map.addLayer(samp1, {color: ' red'}, 'samp1') //the sampling points will be painted red.
```


The randomly selected 120 sampling points or pixels are shown in the figure below. Note, it is possible that your selected points are not exactly same as the ones below, as the sampling is random. So, please do not fret if your result is different to the one reported below.




<img width="456" height="489" alt="image" src="https://github.com/user-attachments/assets/adb5e7ed-32c3-41a9-8abf-3ea5fe6a6047" />





##### Label the sample points or pixels

We have 120 pixels or points to label as NTV, NAV, CTV, AS, NS, and water. First, let's find and label the selected water pixel. Waterbodies appear dark, so they are easy to identfiy and label. Using the "Add a marker" tool. Before you use the "Add a marker" tool: <img width="36" height="45" alt="image" src="https://github.com/user-attachments/assets/f9480963-4367-4f13-b6cd-77feeaced155" />
, under **Geometry Imports** click "+new layer" (see the figure below for help) to create a new geometry. Note, the **Geometry Imports** may not be readily available if you did not complete THE Activity 7. 





![image](https://github.com/user-attachments/assets/a0856ba1-6fa9-4e68-9c1d-8cfcb43bbe91)






Select the "Add a marker" (see figure below). This is the tear-drop icon in the geometry tool. 




![image](https://github.com/user-attachments/assets/2d8345a0-4bb5-4f9b-82df-623279de0050)


    









   

     
     
     
Once you click the icon the tool becomes active, go to the image and select 20 pixels that represent water. Your result may be like the figure below.





![image](https://github.com/user-attachments/assets/fe0fce6f-ce45-40ce-ba4f-57e50ab82be2)






The layer (see the figure below) is named as "geometry". 




![image](https://github.com/user-attachments/assets/9d0a97c7-bfd5-40de-a81e-04c4a7389345)




You would like to edit this to name the layer appropriately. The layer name should be water, so let's edit the layer properties by clicking the cog icon (see figure below)




![image](https://github.com/user-attachments/assets/b9efd590-4ecb-4a0f-a22d-623402f7ee06)


     
     

The "Configure geometry import" window appears, see the figure below.






![image](https://github.com/user-attachments/assets/c1ce9c77-9cc5-4a93-a764-7ecb3711b469)




Edit the "Configure geometry import" for each cover type. The one for water is given here (see the figure below).





![image](https://github.com/user-attachments/assets/0b26cabb-4373-4106-a593-8b908e235f20)



To achieve the above figure, edit the "Configure geometry import" this way:
Make the layer name "water"; import as "FeatureCollection"; click the "+Property" and type "label" into "Property"; and for "Value" assign 0 as this is the first feature collection. If you are repeating this step for the next cover type (e.g., urban), you would assign 1 for "Value", 2 for the third cover type, and so forth.

Once you are happy with the details in the geometry, click OK (see figure below) to save the results.



![image](https://github.com/user-attachments/assets/c90d6715-ca16-44f3-bac6-246cdf065cbd)






Repeat the steps to collect reference samples for the other cover types.  Hint, in the true colour image, forest pixels are usually dark green with rough texture, while farmlands are bright, smooth green patchwork. 

The clearedland, burntland, and mines can be spectrally similar if using true colour images. Thus, it is advisable to display the image as a false colour composite (FCC), combining band 12 (SWIR), band 8 (NIR) and band 4 (Red) to accentuate burnt-land and bare-land. Use this FCC image to select the pixels for burntland. After that, you may revert to the true colour composite display to select pixels for mines and bareland.


Once you have collected samples for all the cover types, the **Geometry Imports** and base map may look similar to the figure below.





![image](https://github.com/user-attachments/assets/62d3f3a4-d0d1-4731-ae93-308f91bea2c6)










     
#### Merge feature collection

Next, merge the six feature collections into one big feature collection.

```JavaScript
var coverTypes = farmland.merge(forest).merge(water).merge(burntland).merge(clearland).merge(mines)

//print the sampleClass to the Console

print(coverTypes, 'Cover Types')

```

#### Sample the image using the feature collection (i.e., regions of interest) 

```JavaScript
var sample = s2.sampleRegions({
  collection: coverTypes, //the feature collection
  properties: ['label'],
  scale: 20 //pixel size for the output image
});
```


#### Partition the sample into training and test sets

Given the classification model must be assessed, the sampling pixels would be partitioned into training sample and test sample. Conventionally, more of the samples is required to teach the model. In this activity, 80% of the sample pixels was used for model training and the remaining 20% used for the testing of the model.

```JavaScript
var coverTypes2 = sample.randomColumn() //adds a random value field to the sample and is used for the partitioning 
var trainingSample = coverTypes2.filter('random <= 0.8') //80% of the data for model training
var testSample = coverTypes2.filter('random > 0.8')  //20% of data for model testing
```


#### Create the CART classification model

Go to the `ee.Classifier` toolboox under **Docs** and select the **ee.Classifier.smileCart(maxNodes, minLeafPopulation)**
This classifier requires two input parameters.


```JavaScript

var cartClassifier = ee.Classifier.smileCart() //sets up the classifier

//trains the classifier 
.train({
  features: trainingSample,
  classProperty: 'label',
  inputProperties: s2.bandNames() //this is the predictor variables
});

```


#### Classify the imagery

```JavaScript

//apply the classifier to the image
var s2Classified = s2.classify(cartClassifier);
```

#### Qualitatively assess a classification 


This would be achieved by visually comparing the classified imagery against the pre-classified imagery.

```JavaScript

//define the palettes for the cover classes
var classColours = {
  min: 0,
  max: 5,
  palette: ['lightgreen', 'darkgreen', 'blue', 'purple','pink', 'magenta']
};

//display the original imagery in true colour combination
Map.setCenter(131.3815, -12.9111, 10);
Map.addLayer(s2, {bands: ['B4', 'B3', 'B2'], min: 0, max: 3000}, 'Original S2 imagery');

//display the classified image
Map.addLayer(s2Classified, classColours, 'Classified S2 imagery');

```

Turn off all the feature collections under **Geometry Imports** and compare the classified image against the original image. Your result may be like the figure below. Don't fret if your result seems to be different to the one below. This is expected as your training pixels may not be the same as what was used to prepare this manual. 


![image](https://github.com/user-attachments/assets/6cc80708-9082-4632-bd67-5f0946d9a39e)  ![image](https://github.com/user-attachments/assets/efd45ab6-918f-45bb-ab9e-96e28e5f7d57)
|:--:|
| *Fig. 2. Left: unclassified Sentinel-2 imagery and Right: CART classificed image. Farmland is light-green; forest is dark-green, water is blue, burnt-land is purple, cleared-land is pink, and mines is magenta *|


 
 This is a subjective assessment of the performance of the model, so it is not expected that the intrepretations of results would be the same for every participant.



#### Export the classification image to Google Drive

```JavaScript

// create a visualisation parameter, where colours represent the six surface types
var vizParam = {min: 0, max: 5, palette: ['lightgreen', 'darkgreen', 'blue', 'purple','pink', 'magenta']};

// Export the CART classification image to Google Drive
var exportClassifiedImage= ee.Image(s2Classified);
Export.image.toDrive({
    image: s2Classified.visualize(vizParam), //keep colours for the cover classes
    description: 'CART_Classification', //file name, so you can readily identify this in your Google Drive
    scale: 20, //pixel size; this maybe increased if the file size is large 
    maxPixels: 1e13 //to increase the number of pixels allowed by default
});


```


Go too the **Tasks** tab and you may find the task under **UNSUBMITTED TASKS**. It is described as **CART_Classification**. The figure below shows this.



![image](https://github.com/user-attachments/assets/568b74e3-4759-476b-ab02-6e86d29cbd58)



Click **RUN** and a window (shown below) to initiate export pops up. Again click **RUN** to export the image to your Google Drive.



![image](https://github.com/user-attachments/assets/7dd0c59e-4edd-479e-a4a5-c651ece7a938)



It may take a few minutes (depends on file size) for this file to be available in your Google Drive. Always make sure you have sufficient space to host your exports. Go to your Google Drive to download the file to your local computer.

## DIY


A Sentinel-2 image surface reflectance product obtained from the Earth Engine catalog is given as: **COPERNICUS/S2_SR_HARMONIZED/20240428T013701_20240428T013722_T52LGL**. A client interested in identifying cleared lands would like to understand the dsitribution of the dominant land cover classes. Classify the image into six land cover classes, including cleared-land, burnt-land, forest, farm-land, water, and mines, using k-means clustering and the k-nearest neighbor algorithm (k-NN) algorithms. 


## Conclusion
