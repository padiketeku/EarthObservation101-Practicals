# Activity 11 Classification Accuracy Assessment

Activity 11 expands on the Activity 10, Image Classification. This activity evaluates the performance of the classifier for an 'unseen' data using error matrix.
Accuracy metrics, such as producer's accuracy, consumer's accuracy, overall accuracy, kappa, and F1-score are computed and explained.

## Introduction

Image classification is conducted by using training data point to teach the model. However, the performance of the model must be measured to assess its usability. This is done by allowing the model to predict a set of data that is unknown to the model. The observed labels and the predicted labels can then be compared in a 2D matrix, referred to as confusion matrix, to determine the performance of the model. The accuracy metrics are probability measures. Producer's accuracy is referenced to predicted cases in that this measures the proportion of the predicted cases that are true. Thus, the producer's accuracy is the correctness of the map in the perspective of the map maker. In contrast, user's/consumer's accuracy is referenced to the osberved class in that this is the probability that the observed pixels are correctly assigned to the true class.  The harmonic average of the user and producer accuracies produces the F1-score. The overall accuracy represents the proportion of correcly classified pixels, while Kappa is used to measure the agreement between the observed and expected samples based on a random data. Foody G. (2020) has challenged the validity of Kappa statistics for image classification accuracy. Ideally, the values of the metrics range between 0-1.


## Learning Outcomes

At the end of this activity, you should be able to:

- Validate a classification model using error matrix
- Interpret accuracy metrics



## Activity Begins


### Task
1, Validate the CART classification in Activity 10, producing the following metrics to describe the accuracy of the model:

a, producer's accuracy <br>
b, consumer's accuracy <br>
c, overall accuracy <br>
d, kappa <br>
e, F1-score <br>

2, Compute the spatial coverage of cleared land


```JavaScript

var testCARTclassifier = testSample
      .classify(cartClassifier)
      .errorMatrix('class', 'classification')

//print the variable to the Console
print(testCARTclassifier, "testCARTclassifier")

//Print the user's accuracy to the console
print('Validation Overall accuracy: ', testCARTclassifier.accuracy())
print('Validation Consumer accuracy: ', testCARTclassifier.consumersAccuracy())
print('Validation Producer accuracy: ', testCARTclassifier.producersAccuracy())
print('Validation Kappa: ', testCARTclassifier.kappa())
print('Validation fscore: ', testCARTclassifier.fscore(1))

```


## DIY

Assess the performance of the CART classification model in Activity 10. Interpret all the accuracy metrics, including producer's accuracy, user's accuracy, overall accuracy, kappa, and F1-score.




## Conclusion


