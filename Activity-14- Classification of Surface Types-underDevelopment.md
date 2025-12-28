# Activity 12: Classification of Surface Types (Under Review for 2026 offering)

Usually, there are many surface types within a remotely sensed imagery- water, vegetation, baresoil, built-up areas, roads, etc. Users may not be be interested in all of these surface types. In fact, a map user would be more interested in just one surface type and would like to know where in the image this surface type is located. Because of this, making a thematic map out of the imagery would be more helpful for the user. In this activity thematic map was created using image classification methods, including k-means clustering and CART. The activity assumes you have covered all the activities preceding this one, especially activities 7 and 8, and thus participants can create feature collections.

## Introduction

Image classification involves grouping similar pixels into a theme or category, such as grouping all pixels representing water under a single theme. The pixels are grouped according to their spectral similarity, taking into account the spectral distance between pixels. The pixel grouping can be performed by the computer or the analyst, who teaches the computer to do this. If the computer predominantly performs the classification of pixels, this is described as **unsupervised**. In contrast, when the analyst teaches the computer to perform the classification, it is referred to as **supervised**. Image classification, whether supervised or unsupervised, can either be pixel-based or object-based. The amalgamation of similar pixels into objects constitutes the object-based approach, which is beyond the scope of this unit. We will only cover pixel-based image classification, specifically exploring k-means clustering and decision trees (i.e., CART and Random Forest) to explain unsupervised and supervised classifiers, respectively. 

## Learning Outcomes

At the end of this activity, you should be able to: <br>

- collect point features
- perform k-means clustering (unsupervised classification)
- perform CART classification (supervised technique)
- perform Random Forest classification (supervised technique)
- qualitatively assess a classified image (via image visualisation)
- export imagery (the classified iimagery) to google drive




## Activity Begins

### Task

A Sentinel-2 image surface reflectance product obtained from the Earth Engine catalog is given as: **COPERNICUS/S2_SR_HARMONIZED/20240428T013701_20240428T013722_T52LGL**. A client interested in identifying cleared lands would like to understand the dsitribution of the dominant land cover classes. Classify the image into the eight major land cover classes from the FAO Land Cover Classification Systems (LCCS) ([Gregorio and Jansen, 2000](https://www.fao.org/4/x0596e/X0596e01f.htm#TopOfPage)), described by the table below.


| Category      | Description |
| ----------- | ----------- |
| A11 (Cultivated and Managed Terrestrial Areas)      | This class refers to areas where the natural vegetation has been removed or modified and replaced by other types of vegetative cover of anthropogenic origin. This vegetation is artificial and requires human activities to maintain it in the long term. In between the human activities, or before starting crop cultivation, the surface can be temporarily without vegetative cover. Its seasonal phenological appearance can be regularly modified by humans (e.g., tillage, harvest, and irrigation). All vegetation that is planted or cultivated with an intent to harvest is included in this class (e.g., wheat fields, orchards, rubber and teak plantations).      |
| A12 (Natural and Semi-Natural Vegetation)  | Natural vegetated areas are defined as areas where the vegetative cover is in balance with the abiotic and biotic forces of its biotope. Semi-natural vegetation is defined as vegetation not planted by humans but influenced by human actions. These may result from grazing, possibly overgrazing the natural phytocenoses, or else from practices such as selective logging in a natural forest whereby the floristic composition has been changed. Previously cultivated areas which have been abandoned and where vegetation is regenerating are also included. The secondary vegetation developing during the fallow period of shifting cultivation is a further example. The human disturbance may be deliberate or inadvertent. Hence semi-natural vegetation includes vegetation due to human influences but which has recovered to such an extent that species composition and environmental and ecological processes are indistinguishable from, or in a process of achieving, its undisturbed state. The vegetative cover is not artificial, in contrast to classes A11 and A24, and it does not require human activities to be maintained in the long term.       |
|A23 (Cultivated Aquatic or Regularly Flooded Areas)   | This class includes areas where an aquatic crop is purposely planted, cultivated and harvested, and which is standing in water over extensive periods during its cultivation period (e.g., paddy rice, tidal rice and deepwater rice). In general, it is the emerging part of the plant that is fully or partly harvested. Other plants (e.g., for purification of water) are free-floating. They are not harvested but they are maintained. This class excludes irrigated cultivated areas.      |
| A24 (Natural and Semi-Natural Aquatic or Regularly Flooded Vegetation)  | This class describes areas which are transitional between pure terrestrial and aquatic systems and where the water table is usually at or near the surface, or the land is covered by shallow water. The predominant vegetation, at least periodically, comprises hydrophytes. Marshes, swamps, bogs or flats where drastic fluctuations in water level or high concentration of salts may prevent the growth of hydrophytes are all part of this class. The vegetative cover is significantly influenced by water and dependent on flooding (e.g., mangroves, marshes, swamps and aquatic beds). Occasionally-flooded vegetation within a terrestrial environment is not included in this class. Natural Vegetated Aquatic habitats are defined as biotopes where the vegetative cover is in balance with the influence of biotic and abiotic forces. Semi-Natural Aquatic vegetation is defined as vegetation that is not planted by humans but which is influenced directly by human activities that are undertaken for other, unrelated purposes. Human activities (e.g., urbanization, mining and agriculture) may influence abiotic factors (e.g., water quality), affecting species composition. Furthermore, this class includes vegetation that developed due to human activities but which has recovered to such an extent that it is indistinguishable from its former state, or which has built up a new biotope which is in balance with the present environmental conditions. A distinction between Natural and Semi-Natural Aquatic Vegetation is not always possible because human activities distant to the habitat may create chain reactions which ultimately disturb the aquatic vegetative cover. Human activities may also take place deliberately to compensate for effects as noted above with the aim of keeping a "natural" state.        |
| B15 (Artificial Surfaces and Associated Areas)   |    This class describes areas that have an artificial cover as a result of human activities such as construction (cities, towns, transportation), extraction (open mines and quarries) or waste disposal.     |
| B16 (Bare Areas)   | This class describes areas that do not have an artificial cover as a result of human activities. These areas include areas with less than 4% vegetative cover. Included are bare rock areas, sands and deserts.        |
| B27 (Artificial Waterbodies, Snow and Ice)   | This class applies to areas that are covered by water due to the construction of artefacts such as reservoirs, canals, artificial lakes, etc. Without these the area would not be covered by water, snow or ice.        |
| B28 (Natural Waterbodies, Snow and Ice)   | This class refers to areas that are naturally covered by water, such as lakes, rivers, snow or ice. In the case of rivers, the lack of vegetation cover is often due to high flow rates and/or steep banks. In the case of lakes, their geological origin affects the life conditions for aquatic vegetation. The following circumstances might cause water surfaces to be without vegetation cover: depth, rocky basins, rocky and/or steep shorelines, infertile washed-in material, hard and coarse substrates.       |


The LCCS standardises land cover/land use and change mapping, and it is a good practice to follow.



Although the task requires a supervised classification be used, prior to the supervised classification, we would explore an unsupervised classifier in k-means clustering for additional experience. In real practice, sometimes, pixels clustering may be conducted prior to a supervised classification. The clustering algorithms show the number of distinct spectral classes in the data. 


### Prepare the imagery for the task

```JavaScript
var s2 = ee.Image('COPERNICUS/S2_SR_HARMONIZED/20240428T013701_20240428T013722_T52LGL')

//select the required bands
var s2 = s2.select(["B2","B3","B4","B5","B6","B7","B8","B11", "B12"])

```

Display a true colour of the imagery.

```JavaScript
Map.addLayer(s2, {bands:["B4", "B3", "B2"], min:200, max:2000}, "S2 True Colour Composite")
```

### Unsupervised classification: kmean clustering

Unsupervisied classification is usually referred to as clustering as the computer is left alone to identify pixels with similar spectral characteristics and group them into one *homogeneous* cluster.  The analyst has two major roles to play: (1) specify the initial parameters and (2) label the output spectral clusters with meaningful themes. In Earth Engine, the unsupervised classification is under the **ee.Clusterer**. The **ee.Clusterer.wekaKMeans** is suited for the k-means clustering.

#### Random sampling of pixels

The pixels sampled would be used to train the classifier.


```JavaScript

var samplePixels = s2.sample({
scale: 20,
numPixels:50000,
tileScale: 4 //a scale factor to enable computations that may otherwise run out of memory
})

print(samplePixels.size())
```

#### Train the k-means clustering

The algorithm takes many input paramters, but here only two input parameters (i.e., nClusters and maxIterations) would be specified leaving the others to default. The number of clusters (nCluster) is an important parameter as this shows the number of distinct spectral classes based upon statistics and this can be objectively or subjectively pre-determined. In this activity it would make more sense to specify 6 for the nClusters, however, in the interest of variability we would specify 10 for the nCluster. 

```JavaScript
var kmeanClusters = ee.Clusterer.wekaKMeans({
nClusters: 10,
maxIterations: 10
})

//train the cluster using the sampled pixels
var kmeanClusters =kmeanClusters.train(samplePixels)
```

#### Apply the k-means algorithm to the image and visualise this

```JavaScript
var clusters = s2.cluster(kmeanClusters)

//visualise the clusters within the classified image
Map.setCenter(131.3815, -12.9111, 10);
Map.addLayer(clusters, {min:1, max:10, palette:['violet','purple', 'indigo', 'blue', 'cyan', ' green', 'yellow', 'orange', 'magenta', 'red']}, "K-means Classified Image")
```

The classification image may be as shown in the figure below.



![image](https://github.com/user-attachments/assets/07c5eb36-6ece-4cd1-ad88-366bc1da057d) |
|:--:|
| *Fig. 1. k-means clustering, including ten clusters.*|


What colour is water? Yes, you are right it is indigo.

Visually compare the classified image against the true colour imagery that was classified to qualitatively assess the performance of the classifier.




### Supervised classification: Classification and Regression Tree (CART)


#### Sample surface types using point feature collections 

   In many real-world applications, the analyst conducts field surveys to identify the various dominant surface types in the study area. GPS is used to collect the locations of the sampled land cover or land use. In the absence of field data, an image (usually a higher resolution) can be used for desktop sampling of the land cover types. This approach was used in this activity. <br>
   
   We would collect sample points for water. First, visualise the image as a true colour composite.

    

     


![image](https://github.com/user-attachments/assets/28164b46-97c2-4b21-bbbd-f6dc8783314c)







Waterbodies appear dark, randomly sample 20 pixels using the "Add a marker" tool. Before you use the "Add a marker" tool, under **Geometry Imports** click "+new layer" (see the figure below for help) to create a new geometry. Note, the **Geometry Imports** may not be readily available if you did not complete Activity 7. 





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

In this activity, unsupervised and supervised classification algorithms, k-means clustering and the classification and regression decision tree algorithms, were explored. A Sentinel-2 surface reflectance product was analysed, while visually assessing the performance of the classifiers. Next, quantitative assessment evaluation of the classifer will be carried out. Also, the spatial extent of the surface types will be computed.
