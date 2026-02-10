# Activity 15 Classification Accuracy Assessment

Activity 15 is a continuation of Activity 14, Supervised Image Classification. So, make sure you have completed the last activity before this one, and you may need to continue from where you left off rather than starting a new script. This activity evaluates the performance of the Random Forest classifier using an 'unseen' stratified random sampling data via an error matrix. Accuracy metrics, such as producer's accuracy, user's accuracy, and overall accuracy, are computed and explained.


## Introduction

Image classification is conducted by using training data point to teach the model. However, the performance of the model must be measured to assess its usability. This is done by allowing the model to predict a set of data that is unknown to the model. The observed labels and the predicted labels can then be compared in a 2D matrix, referred to as error matrix, to determine the performance of the model. The definitions for overall accuracy, user's accuracy and producer's accuracy are given below by [Olofsson et al. 2013](https://www.sciencedirect.com/science/article/pii/S0034425712004191)

***Overall accuracy (OA)** is the proportion of the area mapped correctly. It provides the user of the map with the probability that a randomly selected location on the map is correctly classified.* <br>
***User's accuracy (UA)** is the proportion of the area mapped as a particular category that is actually that category “on the ground” where the reference classification is the best assessment of ground condition. UA is the complement of the probability of commission error* The UA is the correctness of the map in the perspective of the map user, and is often referred to data scientists as the recall.

***Producer's accuracy (PA)** is the proportion of the area that is a particular category on the ground that is also mapped as that category. PA is the complement of the probability of omission error.* The PA is the correctness of the map in the perspective of the map maker, and is normally referred to data scientists as the precision.

The UA and PA are for class-wise assessment of the model. The harmonic average of the UA and PA produces the F1-score. Kappa is used to measure the agreement between the observed and expected samples based on a random data. However, we will not consider this as [Foody (2020)](https://doi.org/10.1016/S0034-4257(01)00295-4) and [Pontius and Millones, 2011](https://doi.org/10.1080/01431161.2011.552923) have challenged the validity of Kappa statistics for image classification accuracy. We will only compute and report the UA, PA, and OA from an error matrix table. <br>

Ideally, the values of the accuracy assessment metrics range between 0-1 (or 0-100%).


## Learning Outcomes

At the end of this activity, you should be able to:

- Perform stratified random sampling of points or pixels
- Validate a classification model using error matrix
- Adjust error matrix table
- Determine the spatial extent of cover classes using adjusted error matrix
- Interpret accuracy metrics




## Activity Begins


### Tasks

1, Validate the Random Forest classification in Activity 14, proudce an error matrix and use this to compute the following metrics. 

- producer's accuracy <br>

- user's accuracy <br>

- overall accuracy <br>

2, Compute the spatial extent of the cover classes in hectares 




#### Perform stratified random sampling using the classified image 

Stratified random sampling is recommended for collecting validation data points/pixels for land cover and land use mapping or change detection tasks [Olofsson et al., (2014)](https://doi.org/10.1016/j.rse.2014.02.015). Here, we used the RF classified image to perform the sampling. The cover classes served as strata, with 10 pixels sampled from each. The script below performs the stratified random sampling.

```JavaScript
//perform stratified sampling
var randomStratifiedPoints = s2ClassifiedRF.stratifiedSample({
  numPoints: 10, // number of points per stratum
  classBand: 'classification',
  region: projectArea,
  scale: 20,
  seed: 000,
  geometries: true
});

```


The sampling points/pixels are called validation points and were displayed alongside the ones used to build the model (i.e., calibration) to ensure that the validation and calibration points/pixels were not the same or spatially close to each other. This must be done to avoid or minimise spatial autocorrelation, which can make the model over-optimistic. Thus, purposive sampling was applied after stratified sampling, carefully selecting or adding validation points/pixels that were neither identical to nor spatially close to the calibration points. The entire sampling approach can be implemented programmatically, but this is beyond the scope of the unit. So, the purposive sampling was done manually, and through this, the validation points/pixels for NTV were 31, AS = 15, CTV = 10, and water = 3. More samples could indeed have been collected, but time did not allow it. Feel free to collect more 'good' validation points/pixels if you have the time.

The script below visualises the calibration and validation points/pixels.

```JavaScript

//visualise the validation points that have been corrected for spatial dependence
Map.addLayer(randomStratifiedPoints, {color: 'yellow'}, 'Validation Points');

//visualise the calibration points. remember, this was created in the last activity. if you are not continuing from the last activity this line of the script may not work
Map.addLayer(coverTypes, {color: 'red'}, 'Calibration Sites')

```

The result from the visualisation script is below.







<img width="472" height="484" alt="image" src="https://github.com/user-attachments/assets/b3a412ec-c584-4c6e-96c7-40e845d0313c" />










The red and yellow points/pixels are the calibration and validation samples, respectively, displayed over the RF classified imagery. The points may appear overlapping or close to each other. You may need to zoom in to appreciate the effect of the purposive sampling in tackling spatial dependence between the data points.



#### Label the validation points/pixels and merge them

Once the validation points/pixels have been sampled, that analysts or experts would have to label these. Follow the labelling approach discussed or demonstrated in the last activity to complete this section. Different feature collections will be created through the labelling process, merge the feature collections into one. Again, you must follow the approach used in the last activity.


#### Assign spectral values to the validation points

At the moment, the validation data consists only of latitude and longitude points with no spectral data. You must assign the spectral values of the pixels to the points that match the pixels to create reference areas. The line of script below sample the reference areas using the validation data.


```JavaScript
var validationPoints = s2ProjectArea.sampleRegions({
  collection: validationSet, // a feature collection
  properties: ['label'],
  scale: 20 //pixel size for the output image
});

```


#### Create an error matrix

Compare the validation data (i.e., observed) against the mapped data (predicted), creating a 2D error matrix table. To do this, run the line of script below. 

```JavaScript
var validation = validationPoints
      .classify(rfClassification)
      .errorMatrix('label', 'classification')

//print the error matrix to the Console
print(validation, "error matrix")
```

The 2D error matrix should be printed to the console and when you click the expander your result may be like this below.



<img width="737" height="252" alt="image" src="https://github.com/user-attachments/assets/d1447bd7-7eb1-4a36-b01a-b41da317c838" />






The cover classes are 0, 1, 2, 3, 4 representing water, vegetation, bareland and agriculture, respectively. The columns are the predicted class and the rows are the observed clas. But, I swapped this in consistence with [Olofsson et al., (2014)](https://doi.org/10.1016/j.rse.2014.02.015) to create a 2D table as shown below. You will get the same result if you keep to the original format of the error matrix by GEE.





<img width="1256" height="385" alt="image" src="https://github.com/user-attachments/assets/28131449-7a3e-4ab0-b4ad-77010f1c03bf" />





The error matrix table is simple but powerful, as it provides insights into the model's performance class-wise and overall. The columns are the observed, and the rows are the predicted classes. We would express the accuracy metrics as percentages for easy interpretation. The entries in the cells of the error matrix represent the pixel counts for the classes, so reading along the columns lets you assess the observed pixels, appreciate the number omitted, and obtain an estimate of the producer's accuracy (PA). Similarly, reading the error matrix row-wise provides counts of predicted pixels and the number of pixels wrongly assigned to a class, and is used to compute the user's accuracy (UA). Going down the columns, all three water pixels were water; none were omitted, hence the PA is 100%. For vegetation, 18 of the 31 observed pixels were identified as 'vegetation,' yielding a PA of 58.1%. Compute the PA for bareland and agriculture. Going row-wise, all 3 water pixels were correctly mapped, yielding a UA of 100%. Vegetation 18 pixels out of 22 were correctly mapped, so the UA is 81.8%. The UA is not 100% because a bareland pixel was incorrectly mapped as vegetation, and 3 agriculture pixels were incorrectly mapped as vegetation. Compute the UA for bareland and agriculture, taking note of the error of commission. The overall accuracy (OA) is estimated by summing the correctly classified pixels and dividing by the total number of validation samples. In the error matrix, the correctly classified samples are the diagonal entries, highlighted in green. The total validation samples are 59. Hence, OA = 40/59, which is 67.8%. The error matrix table below includes estimates for the UA, PA, and OA.








<img width="1612" height="563" alt="image" src="https://github.com/user-attachments/assets/d515569f-fc71-4623-9b86-48ebad979e14" />









#### Compute the extent of cover classes


We know that cover classes are not equal, some have more pixels than others. We would compute the spatial coverage of the cover class using the following scripts. 



```JavaScript
//area estimates
var areaImage = ee.Image.pixelArea().addBands(s2ClassifiedRF);
// Calculate sum of area per class
var classAreas = areaImage.reduceRegion({
  reducer: ee.Reducer.sum().group({
    groupField: 1, // The class band
    groupName: 'label',
  }),
  geometry: projectArea,
  scale: 20, // Adjust to your image resolution
  maxPixels: 1e10,
  bestEffort: true
});
print(classAreas, 'classAreas')


```

The results would be in square metre but we want this to be in hectare. The line of scripts would convert from square metres to hectare.

```JavaScript
//convert the area in m2 to hectare  
//in other words divide the current values by 10,000
var areasList = ee.List(classAreas.get('groups'));
var area_in_ha = areasList.map(function(item) {
  var areaDict = ee.Dictionary(item);
  var label_ID = areaDict.get('label');
  var areaHa = ee.Number(areaDict.get('sum')).divide(1e4);
  return ee.Dictionary({
    'Label': label_ID,
    'Area_km2': areaHa
  });
});

```

The line of script below prints the results to the console.

```JavaScript
print('Areas per class (sq km):', area_in_ha)
```

When you click the expanders in the console you may see the areal coverage for each cover class. Your result may be as shown below. 





<img width="790" height="670" alt="image" src="https://github.com/user-attachments/assets/93161e59-e1b6-40b8-a1ef-e11982de5aa1" />





#### Adjusting the error matrix to account for the different spatial extents of class covers






#### Classification accuracy assessment

The script below tests the Random Forest classifier, printing the overall accuracy, user's accuracy, and producer's accuracy to evaluate the model's performance. Note, given the randomness associated with sampling design, etc., the results you would see may not be the same as the ones presented in this manual.



```JavaScript

var testCARTclassifier = testSample
      .classify(cartClassifier)
      .errorMatrix('label', 'classification')

//print the error matrix to the Console
print(testCARTclassifier, "testCARTclassifier")

//print the accuracy metrics to the console
print('Validation Overall accuracy: ', testCARTclassifier.accuracy())
print('Validation Consumer accuracy: ', testCARTclassifier.consumersAccuracy())
print('Validation Producer accuracy: ', testCARTclassifier.producersAccuracy())
print('Validation Kappa: ', testCARTclassifier.kappa())
print('Validation fscore: ', testCARTclassifier.fscore(1))

```


Error Matrix below:


![image](https://github.com/user-attachments/assets/24c25a5b-aed8-442a-ba48-32db334a40e0)



Validation accuracies below:


![image](https://github.com/user-attachments/assets/1bc4eb7a-c587-46da-8557-daec9bd8fe26)



Using your lecture notes on accuracy metrics, interpret the performance of the classifier. How can the performance of the model be improved or made more believable by users?


#### Spatial extent of a surface type

The spatial extent of a cover class can be computed by:

1, selecting the class of interest

2, create an image with a pixel value equal to the pixel of the image used for the classification. The function `ee.Image.pixelArea()` is used to achieve this 

3, compute the total area using `reduceRegion()` method




Compute the spatial coverage of cleared land in square kilometers. <br> 

The script below computes the spatial extent for the cleared land.


```JavaScript

//select the cover class of interest; in this case, water.
var clearedLand = s2Classified.select('classification').eq(4)


// the area of a pixel
var pixelArea = ee.Image.pixelArea()

//multiply the class layer by pixel area
var areaLand = clearedLand.multiply(pixelArea)

//compute the total area covered by water
var total_areaLand = areaLand.reduceRegion({
  reducer: ee.Reducer.sum(),
  scale: 20,
  maxPixels: 1e13
  })
  
print(total_areaLand, 'total area of cleared land')  


//convert the area in m2 to km2  
var landArea_km2 = ee.Number(
  total_areaLand.get('classification')).divide(1e6).round()
print(landArea_km2)

```

What is the extent of cleared land? It was 1563 km2 for me. It is not surprising if your observation is different, though. 


## DIY

Assess the performance of the CART classification model in Activity 10. Interpret all the accuracy metrics, including producer's accuracy, user's accuracy, overall accuracy, kappa, and F1-score.




## Conclusion

In this activity, we explored image classification accuracy metrics, including producer's and user's accuracies. Also, overall accuracy, kappa and F1-score were computed. The accuracy estimates showed that the classification model requires improvement.
