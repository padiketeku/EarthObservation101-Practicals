# Activity 13: Classification of Surface Types: Unsupervised Method

Usually, there are many surface types within a remotely sensed imagery- water, vegetation, baresoil, built-up areas, roads, etc. Users may not be be interested in all of these surface types. In fact, a map user would be more interested in just one surface type and would like to know where in the image this surface type is located. Because of this, making a thematic map out of the imagery would be more helpful for the user. In this activity thematic map was created using image classification methods, including k-means clustering and CART. The activity assumes you have covered all the activities preceding this one, especially activities 7 and 8, and thus participants can create feature collections.

## Introduction

Image classification is collecting similar pixels into a group or theme. For instance, grouping all water pixels under a theme. The pixels are grouped according to their spectral similarity and taking into account spectral distance between pixels. The pixel grouping can be done by the computer or the analyst teaching the computer to do this. If the classification of pixels is predominantly performed by the computer this is described as **unsupervised** while where the analyst teaches the computer to perform the classification is referred to as **supervised**. The activity explores k-mean clustering and CART to explain unsupervised and supervised classifiers, respectively.


## Learning Outcomes

At the end of this activity, you should be able to: <br>

- randomly sample pixels to use as a training data
- train a k-means algorithm
- organise pixels into clusters using the k-means method
- qualitatively assess the performance of the k-means classification
- collect point features
- perform k-means clustering (unsupervised classification)
- perform CART classification (supervised technique)
- qualitatively assess a classified image (via image visualisation)
- export imagery (the classified iimagery) to google drive




## Activity Begins

### Task

A Sentinel-2 image surface reflectance product obtained from the Earth Engine catalog is given as: **COPERNICUS/S2_SR_HARMONIZED/20240428T013701_20240428T013722_T52LGL**. A client interested in identifying cleared lands would like to understand the dsitribution of the dominant land cover classes. Classify the image into six land cover classes, including cleared-land, burnt-land, forest, farm-land, water, and mines, using k-means clustering the CART algorithms. 

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


#### Qualitative evaluation of the k-means
What colour is water? Yes, you are right it is indigo.

Visually compare the classified image against the true colour imagery that was classified to qualitatively assess the performance of the classifier.






## DIY


A Sentinel-2 image surface reflectance product obtained from the Earth Engine catalog is given as: **COPERNICUS/S2_SR_HARMONIZED/20240428T013701_20240428T013722_T52LGL**. A client interested in identifying cleared lands would like to understand the dsitribution of the dominant land cover classes. Classify the image into six land cover classes, including cleared-land, burnt-land, forest, farm-land, water, and mines, using k-means clustering. 


## Conclusion

In this activity, unsupervised algorithm, k-means clustering was explored. A Sentinel-2 surface reflectance product was analysed, while visually assessing the performance of the classifiers. Next, supervised classification methods will be carried out. 

