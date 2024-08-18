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

A Sentinel-2 image surface reflectance product obtained from the Earth Engine catalog is given as: **COPERNICUS/S2_SR_HARMONIZED/20240428T013701_20240428T013722_T52LGL**. A client interested in identifying cleared lands in a project area wants to understand the dsitribution of the dominant land cover classes. Classify the image into six land cover classes, includingg cleared-land, burnt-land, forest, farm-land, water, and mines. 

|:--:|
| *Fig. 2. A histogram for an NDVI layer.*|

### Prepare the imagery for the task

```
var s2 = ee.Image('COPERNICUS/S2_SR_HARMONIZED/20240428T013701_20240428T013722_T52LGL')

//select the required bands
var s2 = s2.select(['B2','B3','B4','B5','B6','B7','B8','B11', 'B12'])

```
### Unsupervised classification: kmean clustering

Unsupervisied classification is usually referred to as clustering as the computer is left alone to identify pixels with similar spectral characteristics and group them into one homogeneous cluster.  The analyst has two major roles to play: (1) specify the initial parameters and (2) label the output spectral clusters with meaningful themes. In Earth Engine, the unsupervised classification is under the **ee.Clusterer**. The **ee.Clusterer.wekaKMeans** is suited for the k-means clustering.

#### Random sampling of pixels

The pixels sampled would be used to train the classifier.


```JavaScript

var samplePixels = s2.sample({
//region:s2.geometry(),
scale: 20,
numPixels:50000,
tileScale: 4
})

print(samplePixels.size())
```

#### Train the k-means clustering

The algorithms takes many input paramters, but here only two input parameters (i.e., nClusters and maxIterations) would be specified leaving the others to default.

```JavaScript
var kmeanClusters = ee.Clusterer.wekaKMeans({
nClusters: 10,
maxIterations: 10
})

//train the cluster using the sampled pixels
var kmeanClusters =kmeanClusters.train(samplePixels)
```


### Supervised classification: minimum distance classifier

#### Sample surface types using point feature collections create

   In real-world practice the analyst conducts field surveys to identify the various dominant surface types in the study area. GPS is used to collect the locations of the sampled land cover or land use. In the absence of field data, a higher resolution image can be used for desktop sampling of the land cover types. The latter approach was used in this activity. The **geometry tool** was used to creat point feature collections for the surface type. The more samples the better, but in the interest of time and for demonstration purposes 20 pixels were sampled for each surface types. Possibly, spread out the samples across the image. Do not concentrate the points to a region. 

   - Sample farmland, forest, water
     Visualise the image as true  colour composite. Waterbodies appear dark, randomly sample 20 pixels using the "Add a marker" tool. This is the tear-drop icon in the geometry tool. Hint, forest pixels are usually dark green with rough texture, while farmlands are bright/smooth green patchwork.

     
   - Sample clear-land, burnt-land, mines

     Visualise the image as a false colour by combining band 12 (SWIR), band 8 (NIR) and band 4 (Red) to accentuate burnt-land and bare-land. Sample surface types by creating point feature collection for each class. 

     After the edits, the **Geometry Imports** and base map you should be similar to the one in the figure below.


     ![image](https://github.com/user-attachments/assets/62d3f3a4-d0d1-4731-ae93-308f91bea2c6)

     
#### Merge feature collection

Next, merge the six feature collections into one big featuure collection.

```JavaScript
var coverTypes = farmland.merge(forest).merge(water).merge(burntland).merge(clearland).merge(mines)

//print the sampleClass to the Console

print(coverTypes, 'Cover Types')

```

#### Sample the image using the feature collection (i.e., regions of interest)


```JavaScript
var sample = s2.sampleRegions({
  collection: coverTypes, //the feature collection
  properties: ['class'],
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


#### Create the minimum distance classification model

Go to the `ee.Classifier` toolboox under **Docs** and select the **ee.Classifier.minimumDistance(metric, kNearest)**
This classifier requires two input parameters: the distance metric, which you have four different types to select from. The default is the euclidean distance and this is used. The other parameter is the kNearest to specify the moving window size to use. The window size should be an odd number (e.g., 3, 5, 7, etc), the pixel size and degree of variability in the image should inform the window size. In this case, the kNearest was set to 5.


```JavaScript

var minDistanceClassifier = ee.Classifier.minimumDistance("euclidean", 5) //sets up the classifier

//trains the classifier 
.train({
  features: trainingSample,
  classProperty: 'class',
  inputProperties: s2.bandNames() //this is the predictor variables
});

```


#### Classify the imagery

```JavaScript

//apply the classifier to the image
var s2Classified = s2.classify(minDistanceClassifier);
```

#### Visualise the classified imagery

```JavaScript

//define the palettes for the cover classes
var classColours = {
  min: 0,
  max: 5,
  palette: ['006400' ,'ffbb22', 'ffff4c', 'f096ff', 'fa0000', 'b4b4b4']
};

//display the original imagery in true colour combination
Map.setCenter(131.3815, -12.9111, 10);
Map.addLayer(s2, {bands: ['B4', 'B3', 'B2'], min: 0, max: 3000}, 'Original S2 imagery');

//display the classified image
Map.addLayer(s2Classified, classColours, 'Classified');

```



## DIY




## Conclusion
