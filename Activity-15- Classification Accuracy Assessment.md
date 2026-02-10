# Activity 15 Classification Accuracy Assessment

Activity 15 is a continuation of Activity 14, Supervised Image Classification. So, make sure you have completed the last activity before this one. This activity evaluates the performance of the Random Forest classifier using an 'unseen' stratified random sampling data via an error matrix. Accuracy metrics, such as producer's accuracy, user's accuracy, and overall accuracy, are computed and explained.

## Introduction

Image classification is conducted by using training data point to teach the model. However, the performance of the model must be measured to assess its usability. This is done by allowing the model to predict a set of data that is unknown to the model. The observed labels and the predicted labels can then be compared in a 2D matrix, referred to as error matrix, to determine the performance of the model. The definitions for overall accuracy, user's accuracy and producer's accuracy are given below by [Olofsson et al. 2013](https://www.sciencedirect.com/science/article/pii/S0034425712004191)

***Overall accuracy (OA)** is the proportion of the area mapped correctly. It provides the user of the map with the probability that a randomly selected location on the map is correctly classified.* <br>
***User's accuracy (UA)** is the proportion of the area mapped as a particular category that is actually that category “on the ground” where the reference classification is the best assessment of ground condition.User's accuracy is the complement of the probability of commission error* The user's accuracy is the correctness of the map in the perspective of the map user, and is often referred to data scientists as the recall.

***Producer's accuracy (PA)** is the proportion of the area that is a particular category on the ground that is also mapped as that category. Producer's accuracy is the complement of the probability of omission error.* The producer's accuracy is the correctness of the map in the perspective of the map maker, and is normally referred to data scientists as the precision.

The UA and PA are for class-wise assessment of the model. The harmonic average of the UA and PA produces the F1-score. Kappa is used to measure the agreement between the observed and expected samples based on a random data. However, we will not consider this as [Foody G. (2020), and Pontius and Millones, 2011](https://doi.org/10.1016/S0034-4257(01)00295-4)(https://doi.org/10.1080/01431161.2011.552923)  have challenged the validity of Kappa statistics for image classification accuracy. We will only compute and report the UA, PA, and OA from an error matrix table. <br>

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
1, Validate the CART classification in Activity 10, producing the following metrics to describe the accuracy of the model:

- producer's accuracy <br>

- consumer's accuracy <br>

- overall accuracy <br>

- kappa <br>

- F1-score <br>



2, Compute the spatial extent of cleared land in square kilometers


#### Classification accuracy assessment

The below script tests the CART classifier printing out the overall accuracy, consumer accuracy, producer accuracy, kappa and F1-score values for the evaluation of the performance of the model. Note, given the randomness associated with data partitioning, etc., the results you would see may not be the same as the one presented in this manual.


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
