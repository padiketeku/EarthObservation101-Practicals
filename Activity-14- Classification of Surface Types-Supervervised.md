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

A Sentinel-2 image surface reflectance product obtained from the Earth Engine catalog is given as: **COPERNICUS/S2_SR_HARMONIZED/20240428T013701_20240428T013722_T52LGL**. A client interested in identifying cleared lands would like to understand the dsitribution of the dominant land cover classes. Classify the image pixels into major land cover classes according to the FAO Land Cover Classification Systems (LCCS) ([Gregorio and Jansen, 2000](https://www.fao.org/4/x0596e/X0596e01f.htm#TopOfPage)). Make sure your class labels align with the one adapted by Australia (see below for the descriptions). The polygon delimiting the study area is given by this list of coordinates: [[[130.85, -13.39],[131.29, -13.40],[131.31, -12.76],[130.86, -12.77]]]



### Standard cover classes

Reference data obtained through field surveys or higher resolution imagery are critically important for image classification tasks. The reference data Labelling surface types can be easy, especially if you are familiar with the study area or relying on existing data from experts. Nevertheless, the labeling process can highly subjective and usually differ between places and persons, potentially causing vagueness and ambiguity. To remove or minimise issues related to the subjectiveness of labelling, the FAO proposed the Land Cover Classification Systems (LCCS) ([Gregorio and Jansen, 2000](https://www.fao.org/4/x0596e/X0596e01f.htm#TopOfPage)) for country to follow as much as practicable. This is meant to standardise the labels we assign to surface types and hence creating land cover classes that are observable by satellites. The LCCS standardises land cover and land use and change mapping, and it is a good practice to follow.

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

Zoom in and out, explore the image to identify the different surface types. You may see water, forests, roads, mines, croplands, burnt land, and bare ground. Let us categorise these surface types into the six major land cover classes. You may notice that NTV dominates the study area. However, you may also see CTV in the top north, scattered waterbodies, NS, and AS. Once you understand the surface types and cover classes in your study area, collect reference features and label them.


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

We have 120 pixels or points to label as NTV, NAV, CTV, AS, NS, and water. First, let's find and label the selected pixels for NTV. Click the "Add a marker" tool: <img width="36" height="45" alt="image" src="https://github.com/user-attachments/assets/f9480963-4367-4f13-b6cd-77feeaced155" />
You may notice that a **geometry** checked box pops up: <img width="319" height="53" alt="image" src="https://github.com/user-attachments/assets/559d61fc-e020-4767-abc9-a7236e6d6986" />
If you hover over the "**geometry** you may see **Geometry Imports** , click the cog icon for the **Configure geometry import** window to appear. Edit this window by changing the Name from "geometry" to "ntv", click "Geometry" under "Import as" to select "FeatureCollection" from the dropdown menu, and under "Properties" click  the "+Property" and type "label" and "0" for the "Property" and "Value" respectively. Note, you can start a text with a lower or upper case, but you must be consistent to avoid running into issues down the line. You may modify the colour assigned to the sample in the **Configure geometry import** by moving the ring icon on the colour palette to any colour of your choice and this will change the colour code for the sample. See the ring icon (identified with an arrow) in the colour palette below.  



<img width="1286" height="933" alt="image" src="https://github.com/user-attachments/assets/b5f7944f-8545-425b-a652-941ce60a82cd" />





The ring is on red with a colour code of #d63000. Click and drag the ring to green and slide down the intensity slider that pops up (right to the palette) to dark green so the NTV is identifed with dark green markers.

Your **Configure geometry import** window once completed for this sample may look like the figure below.




<img width="843" height="604" alt="image" src="https://github.com/user-attachments/assets/b64e6c48-2d33-405f-8c7a-e5e3cb9e9f2c" />







Click "OK" to have your first NTV sample appear in the code editor as this:



<img width="699" height="98" alt="image" src="https://github.com/user-attachments/assets/b0b01cbe-de7b-4abd-8f21-233b4bb2be45" />




You may notice that you have zero (0) elements in your feature collection, and this is because you are yet to drop markers to the NTV samples.


Now zoom in to a sampling point for NTV and add a marker: <img width="32" height="37" alt="image" src="https://github.com/user-attachments/assets/d6208536-809a-4cf6-952e-861999325be5" />





Repeat this step (i.e., adding markers) until you have dropped markers to all the NTV samples. Your result may look like the figure below.





<img width="457" height="507" alt="image" src="https://github.com/user-attachments/assets/7a9659ea-19e9-4f6f-ac2d-4dc148cbbb70" />













**Sampling for the other classes**

Once you completed labelling the samples for one cover class, e.g., NTV, you will move to the next class until you have labelled the samples for all classes. Our next class is the AS. 



In **Geometry Imports** click "+new layer" (see the figure below for help) to create a new geometry. Click the cog icon and edit the **Configure geometry import**, repeating the steps for the NTV. Make sure you choose a different colour to represent this cover class. Keep creating feature collections through the "new layer" and editing the **Configure geometry import** until all the six cover classes are covered. Your final label layer for the cover classes may be like the figure below. Each cover class is represented with a unique colour. Dark green for NTV, light green for CTV, blue for water, magenta for AS, orange for NS, and blue-green for NAV. 



<img width="484" height="517" alt="image" src="https://github.com/user-attachments/assets/35e2af51-e55c-4a3d-b29a-9d35cae71709" />




Note that you may need to use Google satellite imagery to support the labelling process. In fact, Google satellite imagery was used as much as possible to assign labels to the points.

It is worth flagging that you may see that majority of the 120 pixels or points sampled were NTV. When this was experience purposive sampling of additional pixels or points was performed for the rare classes, resulting in 183 samples: 63 more than the original. NTV was 105, AS 24, NS 17, CTV 18, NAV 10, and water 9 sampling points.  [Olofsson et al. (2014)](https://doi.org/10.1016/j.rse.2014.02.015) recommended a minimum of 50 samples for rare classes. We did not apply this recommendation in this demonstration, you may want to follow this recommendation in future work.


  
##### Merge feature collection

You may have a feature collection for each cover class. Merge the six feature collections into one big feature collection.

```JavaScript
var coverTypes = ntv.merge(as).merge(ns).merge(ctv).merge(nav).merge(water)

//print the sampleClass to the Console

print(coverTypes, 'Cover Types')

```



##### Re-label the cover classes

We have observed that the study area has rare classes, and to optimise the sampling points, the rare classes could be merged. To this end, spectrally similar classes were merged into a single class. NTV and NAV were merged into a single class, and AS and NS were merged as well, resulting in four land cover classes for further analysis. Assuming the original labels for water = 0, NTV = 1, AS = 2, NS = 3, NAV = 4 and CTV = 5, we can merge and re-label using the script below.

```JavaScript
//merge and relabel classes, as some classes are spectrally similar and rare in the image
var coverTypes2 = coverTypes.remap([0, 1,2,3,4,5],[0,1,2,2,1,3], 'label')
```


##### Create reference areas, assigning the spectral values to sample points

We know our sample points, and these are lat and lon coordinates of the pixels, but not yet integrated with the Sentinel-2 imagery. The spectral observations for the pixel or point in the feature collection must be provided to create reference areas with known spectral characteristics.

```JavaScript
var referencePoints = s2ProjectArea.sampleRegions({
  collection: coverTypes2, //the feature collection
  properties: ['label'],
  scale: 20 //pixel size for the output image
});

//print result to console
print(referencePoints, 'referencePoints')
```

This produces reference data that can be used to teach supervised machine learning algorithms. We would explore Classification and Regression Tree (CART) and Random Forest (RF), as they belong to the same family and are easy to train. 

##### Train CART and classify the pixels 

```JavaScript
//create a CART model
var cartClassifier = ee.Classifier.smileCart() //sets up the classifier

//trains the classifier 
.train({
  features: referencePoints,
  classProperty: 'label',
  inputProperties: s2ProjectArea.bandNames() //this is the predictor variables
});
```


```JavaScript
//apply the trained CART classifier to the image
var s2ClassifiedCART = s2ProjectArea.classify(cartClassifier);

```

##### Visualise the original and classified images for qualitative assessment of the CART

```JavaScript
//define the palettes for the cover classes
var classColours = {
  min: 0,
  max: 3,
  palette: ['blue', 'green', 'purple', 'lightgreen'] //BLUE = water, greeen = NTV, purple = bareland (AS+NS), lightgreen = CTV
};

//display the original imagery in true colour combination
Map.setCenter(131.3815, -12.9111, 10);
Map.addLayer(s2ProjectArea, {bands: ['B4', 'B3', 'B2'], min: 0, max: 3000}, 'Original S2 imagery');

//display the classified image
Map.addLayer(s2ClassifiedCART, classColours, 'Classified S2 CART imagery');
```


##### Train RF and classify the pixels 

```JavaScript
var rfClassification = ee.Classifier.smileRandomForest({
  numberOfTrees: 500, //number of trees to grow 
  bagFraction: 0.6 // proportion of training data to use per iteration
}).train({
  features: referencePoints,  
  classProperty: 'label',
  inputProperties: s2ProjectArea.bandNames()
});
```

Default values were used for all other hyperparameters. The best approach to ensure the optimal hyperparameter values are used is through tuning, but this can be computer intensive to achieve within GEE without issues. We have kep it simple here, but you may want to explore hyperparameter tuning to obtain best values in future work.


```JavaScript

//apply the RF classifier to the image
var s2ClassifiedRF = s2ProjectArea.classify(rfClassification);
print(s2ClassifiedRF, "s2Classified Using RF")

```

```JavaScript
//display the RF classified image to compare with the CART
Map.addLayer(s2ClassifiedRF, classColours, 'Classified S2 imagery-RF');
```





The original (top left), CART classified (top right) and RF classified (bottom left) images are provided below for qualitative assessment of the models. Turn off all the feature collections under **Geometry Imports** and compare the classified images against the original image. Your result may be like the figure below. Don't worry if your result seems to be different to the one below. This is expected as your training pixels may not be the same as what was used to prepare this manual. 






<img width="371" height="442" alt="image" src="https://github.com/user-attachments/assets/93e07c22-e19b-40ad-811b-22c315af983e" />   <img width="331" height="449" alt="image" src="https://github.com/user-attachments/assets/17f080f8-f32d-471e-82ad-254bc434872b" />  <img width="315" height="444" alt="image" src="https://github.com/user-attachments/assets/45ba0722-2918-44b7-bbbe-2535ffd24945" />








The CART and RF performed quite averagely, suggesting the result can be improved with more training data and predictor variables. Compared to RF, the CART image is more pixelated and noisier. This is a subjective assessment of the performance of the models, so it is not expected that the intrepretations of results would be the same for every participant. An objective approach will be investigated in the next activity. 




##### Export the classification image to Google Drive

Once you are happy with your classified image, you may want to export this to your own local computer for further use. The script below explains how to export an image to Google Drive so you can download the image onto your local computer.


```JavaScript

// Export the RF classification image to Google Drive
Export.image.toDrive({
    image: s2ClassifiedCART.visualize(classColours), //keep colours for the cover classes
    description: 'CART_Classification', //file name, so you can readily identify this in your Google Drive
    scale: 20, //pixel size; this maybe increased if the file size is large 
    maxPixels: 1e13 //to increase the number of pixels allowed by default
});


```


Go to the **Tasks** tab and you may find the task under **UNSUBMITTED TASKS**. It is described as **CART_Classification**. The figure below shows this.



![image](https://github.com/user-attachments/assets/568b74e3-4759-476b-ab02-6e86d29cbd58)



Click **RUN** and a window (shown below) to initiate export pops up. Again click **RUN** to export the image to your Google Drive.



![image](https://github.com/user-attachments/assets/7dd0c59e-4edd-479e-a4a5-c651ece7a938)



It may take a few minutes (depends on file size) for this file to be available in your Google Drive. Always make sure you have sufficient space to host your exports. Go to your Google Drive to download the file to your local computer.




## Complete Script


```JavaScript

//define reference sites for the different surface types
var water = 
    /* color: #1632d6 */
    /* shown: false */
    ee.FeatureCollection(
        [ee.Feature(
            ee.Geometry.Point([131.01846171207885, -12.855782326729976]),
            {
              "label": 0,
              "system:index": "0"
            }),
        ee.Feature(
            ee.Geometry.Point([131.16562632220047, -12.95892029716426]),
            {
              "label": 0,
              "system:index": "1"
            }),
        ee.Feature(
            ee.Geometry.Point([130.99542406695144, -12.854677274080622]),
            {
              "label": 0,
              "system:index": "2"
            }),
        ee.Feature(
            ee.Geometry.Point([131.25472698676577, -12.770812479298637]),
            {
              "label": 0,
              "system:index": "3"
            }),
        ee.Feature(
            ee.Geometry.Point([131.26031868531373, -12.775125356508365]),
            {
              "label": 0,
              "system:index": "4"
            }),
        ee.Feature(
            ee.Geometry.Point([130.99837239953166, -13.041878925473462]),
            {
              "label": 0,
              "system:index": "5"
            }),
        ee.Feature(
            ee.Geometry.Point([131.12257910355288, -12.857504600298544]),
            {
              "label": 0,
              "system:index": "6"
            }),
        ee.Feature(
            ee.Geometry.Point([131.22064107481435, -12.877510313604946]),
            {
              "label": 0,
              "system:index": "7"
            }),
        ee.Feature(
            ee.Geometry.Point([131.05311663235173, -13.052716749221885]),
            {
              "label": 0,
              "system:index": "8"
            })]),
    ntv = 
    /* color: #345700 */
    /* shown: false */
    ee.FeatureCollection(
        [ee.Feature(
            ee.Geometry.Point([130.99363962917334, -12.769842207885869]),
            {
              "label": 1,
              "system:index": "0"
            }),
        ee.Feature(
            ee.Geometry.Point([130.9885437831243, -12.806285082340436]),
            {
              "label": 1,
              "system:index": "1"
            }),
        ee.Feature(
            ee.Geometry.Point([131.03982462884247, -12.798382811011745]),
            {
              "label": 1,
              "system:index": "2"
            }),
        ee.Feature(
            ee.Geometry.Point([131.1117077600337, -12.810935630316678]),
            {
              "label": 1,
              "system:index": "3"
            }),
        ee.Feature(
            ee.Geometry.Point([131.02248223728654, -12.798853709646009]),
            {
              "label": 1,
              "system:index": "4"
            }),
        ee.Feature(
            ee.Geometry.Point([131.02987029970242, -12.823898056949623]),
            {
              "label": 1,
              "system:index": "5"
            }),
        ee.Feature(
            ee.Geometry.Point([130.9163241593801, -12.815833678928751]),
            {
              "label": 1,
              "system:index": "6"
            }),
        ee.Feature(
            ee.Geometry.Point([130.877339014024, -12.819254856249765]),
            {
              "label": 1,
              "system:index": "7"
            }),
        ee.Feature(
            ee.Geometry.Point([131.04551606878542, -12.830069819648594]),
            {
              "label": 1,
              "system:index": "8"
            }),
        ee.Feature(
            ee.Geometry.Point([130.99802113597593, -12.89307211967806]),
            {
              "label": 1,
              "system:index": "9"
            }),
        ee.Feature(
            ee.Geometry.Point([130.94671175908928, -12.870550202947822]),
            {
              "label": 1,
              "system:index": "10"
            }),
        ee.Feature(
            ee.Geometry.Point([131.1339416379988, -12.845525235362565]),
            {
              "label": 1,
              "system:index": "11"
            }),
        ee.Feature(
            ee.Geometry.Point([131.26011424729023, -12.815973697309664]),
            {
              "label": 1,
              "system:index": "12"
            }),
        ee.Feature(
            ee.Geometry.Point([131.22113820679144, -12.84450496684996]),
            {
              "label": 1,
              "system:index": "13"
            }),
        ee.Feature(
            ee.Geometry.Point([131.18175951622231, -12.852304164521064]),
            {
              "label": 1,
              "system:index": "14"
            }),
        ee.Feature(
            ee.Geometry.Point([131.15747890327177, -12.870135481319128]),
            {
              "label": 1,
              "system:index": "15"
            }),
        ee.Feature(
            ee.Geometry.Point([131.17451639431758, -12.842805975879772]),
            {
              "label": 1,
              "system:index": "16"
            }),
        ee.Feature(
            ee.Geometry.Point([131.1044036915824, -12.896023137301771]),
            {
              "label": 1,
              "system:index": "17"
            }),
        ee.Feature(
            ee.Geometry.Point([131.08361546544808, -12.912419166612578]),
            {
              "label": 1,
              "system:index": "18"
            }),
        ee.Feature(
            ee.Geometry.Point([130.87153226522474, -12.887896375657883]),
            {
              "label": 1,
              "system:index": "19"
            }),
        ee.Feature(
            ee.Geometry.Point([130.85546321389646, -12.947686789703235]),
            {
              "label": 1,
              "system:index": "20"
            }),
        ee.Feature(
            ee.Geometry.Point([131.18114373800725, -12.915873517847164]),
            {
              "label": 1,
              "system:index": "21"
            }),
        ee.Feature(
            ee.Geometry.Point([131.24365090209545, -12.902154334576737]),
            {
              "label": 1,
              "system:index": "22"
            }),
        ee.Feature(
            ee.Geometry.Point([131.24483226247094, -12.89528976302271]),
            {
              "label": 1,
              "system:index": "23"
            }),
        ee.Feature(
            ee.Geometry.Point([131.28894501136034, -12.944362146756434]),
            {
              "label": 1,
              "system:index": "24"
            }),
        ee.Feature(
            ee.Geometry.Point([131.26832442888212, -12.944028376770246]),
            {
              "label": 1,
              "system:index": "25"
            }),
        ee.Feature(
            ee.Geometry.Point([131.21683483836154, -12.96006860730764]),
            {
              "label": 1,
              "system:index": "26"
            }),
        ee.Feature(
            ee.Geometry.Point([131.05385374785556, -12.99829864922671]),
            {
              "label": 1,
              "system:index": "27"
            }),
        ee.Feature(
            ee.Geometry.Point([130.9257159426634, -13.004386966838835]),
            {
              "label": 1,
              "system:index": "28"
            }),
        ee.Feature(
            ee.Geometry.Point([130.94676511841212, -13.004574724800626]),
            {
              "label": 1,
              "system:index": "29"
            }),
        ee.Feature(
            ee.Geometry.Point([130.89336971100852, -13.017008419529532]),
            {
              "label": 1,
              "system:index": "30"
            }),
        ee.Feature(
            ee.Geometry.Point([130.87332937679457, -13.030657883890196]),
            {
              "label": 1,
              "system:index": "31"
            }),
        ee.Feature(
            ee.Geometry.Point([131.06570978510797, -13.02765182438787]),
            {
              "label": 1,
              "system:index": "32"
            }),
        ee.Feature(
            ee.Geometry.Point([131.0041271281991, -13.065603467999813]),
            {
              "label": 1,
              "system:index": "33"
            }),
        ee.Feature(
            ee.Geometry.Point([131.04068616085337, -13.063832605240691]),
            {
              "label": 1,
              "system:index": "34"
            }),
        ee.Feature(
            ee.Geometry.Point([131.03608967923344, -13.08357654355783]),
            {
              "label": 1,
              "system:index": "35"
            }),
        ee.Feature(
            ee.Geometry.Point([130.87074897704295, -13.096022470115619]),
            {
              "label": 1,
              "system:index": "36"
            }),
        ee.Feature(
            ee.Geometry.Point([130.92496613339176, -13.090434401282186]),
            {
              "label": 1,
              "system:index": "37"
            }),
        ee.Feature(
            ee.Geometry.Point([130.95103725270093, -13.101820152094414]),
            {
              "label": 1,
              "system:index": "38"
            }),
        ee.Feature(
            ee.Geometry.Point([130.98377422719395, -13.118266074707513]),
            {
              "label": 1,
              "system:index": "39"
            }),
        ee.Feature(
            ee.Geometry.Point([131.06900597074485, -13.145213328305225]),
            {
              "label": 1,
              "system:index": "40"
            }),
        ee.Feature(
            ee.Geometry.Point([131.07143594021045, -13.15139415676052]),
            {
              "label": 1,
              "system:index": "41"
            }),
        ee.Feature(
            ee.Geometry.Point([130.89430156322618, -13.14803556060397]),
            {
              "label": 1,
              "system:index": "42"
            }),
        ee.Feature(
            ee.Geometry.Point([130.87646808045884, -13.174385931569116]),
            {
              "label": 1,
              "system:index": "43"
            }),
        ee.Feature(
            ee.Geometry.Point([130.9557091339194, -13.172562497310574]),
            {
              "label": 1,
              "system:index": "44"
            }),
        ee.Feature(
            ee.Geometry.Point([130.95212395422283, -13.205077341447524]),
            {
              "label": 1,
              "system:index": "45"
            }),
        ee.Feature(
            ee.Geometry.Point([130.91298487475183, -13.216068090762985]),
            {
              "label": 1,
              "system:index": "46"
            }),
        ee.Feature(
            ee.Geometry.Point([130.95054990733303, -13.237796571473647]),
            {
              "label": 1,
              "system:index": "47"
            }),
        ee.Feature(
            ee.Geometry.Point([131.0482654416414, -13.258938288894809]),
            {
              "label": 1,
              "system:index": "48"
            }),
        ee.Feature(
            ee.Geometry.Point([131.04284858046907, -13.265766063254583]),
            {
              "label": 1,
              "system:index": "49"
            }),
        ee.Feature(
            ee.Geometry.Point([131.0517849143788, -13.276117270969827]),
            {
              "label": 1,
              "system:index": "50"
            }),
        ee.Feature(
            ee.Geometry.Point([131.04844690525977, -13.278795655998481]),
            {
              "label": 1,
              "system:index": "51"
            }),
        ee.Feature(
            ee.Geometry.Point([130.85938941556253, -13.270900409687425]),
            {
              "label": 1,
              "system:index": "52"
            }),
        ee.Feature(
            ee.Geometry.Point([130.89350443202295, -13.302580367740449]),
            {
              "label": 1,
              "system:index": "53"
            }),
        ee.Feature(
            ee.Geometry.Point([130.92774910955757, -13.284854497803265]),
            {
              "label": 1,
              "system:index": "54"
            }),
        ee.Feature(
            ee.Geometry.Point([130.98293457088778, -13.279675864349588]),
            {
              "label": 1,
              "system:index": "55"
            }),
        ee.Feature(
            ee.Geometry.Point([130.96596504374946, -13.286390673598163]),
            {
              "label": 1,
              "system:index": "56"
            }),
        ee.Feature(
            ee.Geometry.Point([131.00987497987103, -13.29248755562205]),
            {
              "label": 1,
              "system:index": "57"
            }),
        ee.Feature(
            ee.Geometry.Point([130.94927495938816, -13.365662476755915]),
            {
              "label": 1,
              "system:index": "58"
            }),
        ee.Feature(
            ee.Geometry.Point([130.94811976477334, -13.382823015536436]),
            {
              "label": 1,
              "system:index": "59"
            }),
        ee.Feature(
            ee.Geometry.Point([131.01958708393465, -13.37688967503891]),
            {
              "label": 1,
              "system:index": "60"
            }),
        ee.Feature(
            ee.Geometry.Point([131.01688601439554, -13.328798697252461]),
            {
              "label": 1,
              "system:index": "61"
            }),
        ee.Feature(
            ee.Geometry.Point([131.03011477182184, -13.319337843171544]),
            {
              "label": 1,
              "system:index": "62"
            }),
        ee.Feature(
            ee.Geometry.Point([131.1070483343023, -13.366186710109291]),
            {
              "label": 1,
              "system:index": "63"
            }),
        ee.Feature(
            ee.Geometry.Point([131.27936607296638, -13.388066138217486]),
            {
              "label": 1,
              "system:index": "64"
            }),
        ee.Feature(
            ee.Geometry.Point([131.27875993278684, -13.369231495283094]),
            {
              "label": 1,
              "system:index": "65"
            }),
        ee.Feature(
            ee.Geometry.Point([131.25704070122669, -13.365000928271584]),
            {
              "label": 1,
              "system:index": "66"
            }),
        ee.Feature(
            ee.Geometry.Point([131.2648677823697, -13.347146810047121]),
            {
              "label": 1,
              "system:index": "67"
            }),
        ee.Feature(
            ee.Geometry.Point([131.28374302039032, -13.288540740691992]),
            {
              "label": 1,
              "system:index": "68"
            }),
        ee.Feature(
            ee.Geometry.Point([131.20493048320847, -13.248481844070348]),
            {
              "label": 1,
              "system:index": "69"
            }),
        ee.Feature(
            ee.Geometry.Point([131.1404458989241, -13.236196432549368]),
            {
              "label": 1,
              "system:index": "70"
            }),
        ee.Feature(
            ee.Geometry.Point([131.15452113487936, -13.225542244803881]),
            {
              "label": 1,
              "system:index": "71"
            }),
        ee.Feature(
            ee.Geometry.Point([131.13728267144484, -13.206633004147315]),
            {
              "label": 1,
              "system:index": "72"
            }),
        ee.Feature(
            ee.Geometry.Point([131.27927498666145, -13.230667823905918]),
            {
              "label": 1,
              "system:index": "73"
            }),
        ee.Feature(
            ee.Geometry.Point([131.25013341950824, -13.192179291838698]),
            {
              "label": 1,
              "system:index": "74"
            }),
        ee.Feature(
            ee.Geometry.Point([131.2430481937241, -13.18003028467777]),
            {
              "label": 1,
              "system:index": "75"
            }),
        ee.Feature(
            ee.Geometry.Point([131.2517060375266, -13.147162429268736]),
            {
              "label": 1,
              "system:index": "76"
            }),
        ee.Feature(
            ee.Geometry.Point([131.11293040562464, -13.130073768849813]),
            {
              "label": 1,
              "system:index": "77"
            }),
        ee.Feature(
            ee.Geometry.Point([131.1207937686382, -13.123989721297432]),
            {
              "label": 1,
              "system:index": "78"
            }),
        ee.Feature(
            ee.Geometry.Point([131.16868527479573, -13.12979368230822]),
            {
              "label": 1,
              "system:index": "79"
            }),
        ee.Feature(
            ee.Geometry.Point([131.20442034550115, -13.113161938362335]),
            {
              "label": 1,
              "system:index": "80"
            }),
        ee.Feature(
            ee.Geometry.Point([131.23325471928433, -13.134246226959617]),
            {
              "label": 1,
              "system:index": "81"
            }),
        ee.Feature(
            ee.Geometry.Point([131.2855409982942, -13.1020138089461]),
            {
              "label": 1,
              "system:index": "82"
            }),
        ee.Feature(
            ee.Geometry.Point([131.13318833976166, -13.072240346594054]),
            {
              "label": 1,
              "system:index": "83"
            }),
        ee.Feature(
            ee.Geometry.Point([131.19746332616037, -13.060694375414926]),
            {
              "label": 1,
              "system:index": "84"
            }),
        ee.Feature(
            ee.Geometry.Point([131.2035139909944, -13.044836071454357]),
            {
              "label": 1,
              "system:index": "85"
            }),
        ee.Feature(
            ee.Geometry.Point([131.22159756178212, -13.020508113430008]),
            {
              "label": 1,
              "system:index": "86"
            }),
        ee.Feature(
            ee.Geometry.Point([131.25920333748044, -13.010507413341704]),
            {
              "label": 1,
              "system:index": "87"
            }),
        ee.Feature(
            ee.Geometry.Point([131.2599748047424, -13.008896419499163]),
            {
              "label": 1,
              "system:index": "88"
            }),
        ee.Feature(
            ee.Geometry.Point([130.92976238728733, -12.769549542604961]),
            {
              "label": 1,
              "system:index": "89"
            }),
        ee.Feature(
            ee.Geometry.Point([131.27191170924547, -12.797523482987986]),
            {
              "label": 1,
              "system:index": "90"
            }),
        ee.Feature(
            ee.Geometry.Point([131.06176301493377, -13.107987249216334]),
            {
              "label": 1,
              "system:index": "91"
            }),
        ee.Feature(
            ee.Geometry.Point([130.86420574611427, -13.121766386301175]),
            {
              "label": 1,
              "system:index": "92"
            }),
        ee.Feature(
            ee.Geometry.Point([130.88513888474603, -13.188647040888739]),
            {
              "label": 1,
              "system:index": "93"
            }),
        ee.Feature(
            ee.Geometry.Point([131.1485425174015, -13.326237683564358]),
            {
              "label": 1,
              "system:index": "94"
            }),
        ee.Feature(
            ee.Geometry.Point([131.10142407759088, -13.36053759078196]),
            {
              "label": 1,
              "system:index": "95"
            }),
        ee.Feature(
            ee.Geometry.Point([131.22519049940115, -13.382377609320391]),
            {
              "label": 1,
              "system:index": "96"
            }),
        ee.Feature(
            ee.Geometry.Point([131.28190099256273, -13.309317243500209]),
            {
              "label": 1,
              "system:index": "97"
            }),
        ee.Feature(
            ee.Geometry.Point([131.0752917628069, -12.976070151067812]),
            {
              "label": 1,
              "system:index": "98"
            }),
        ee.Feature(
            ee.Geometry.Point([130.94483550303204, -13.041057620304695]),
            {
              "label": 1,
              "system:index": "99"
            }),
        ee.Feature(
            ee.Geometry.Point([130.879900082241, -13.011928625859342]),
            {
              "label": 1,
              "system:index": "100"
            }),
        ee.Feature(
            ee.Geometry.Point([130.88557857748145, -13.011972722647357]),
            {
              "label": 1,
              "system:index": "101"
            }),
        ee.Feature(
            ee.Geometry.Point([131.03206746246664, -13.044417422962935]),
            {
              "label": 1,
              "system:index": "102"
            }),
        ee.Feature(
            ee.Geometry.Point([130.94954418565982, -13.273008438681135]),
            {
              "label": 1,
              "system:index": "103"
            }),
        ee.Feature(
            ee.Geometry.Point([131.02152644831665, -13.286363868337402]),
            {
              "label": 1,
              "system:index": "104"
            })]),
    as = 
    /* color: #8b3319 */
    /* shown: false */
    ee.FeatureCollection(
        [ee.Feature(
            ee.Geometry.Point([130.92499311943445, -12.830216562854396]),
            {
              "label": 2,
              "system:index": "0"
            }),
        ee.Feature(
            ee.Geometry.Point([131.11403548002434, -12.786485790033002]),
            {
              "label": 2,
              "system:index": "1"
            }),
        ee.Feature(
            ee.Geometry.Point([130.93937229536266, -12.991890850582672]),
            {
              "label": 2,
              "system:index": "2"
            }),
        ee.Feature(
            ee.Geometry.Point([131.24473218024102, -13.074390890920522]),
            {
              "label": 2,
              "system:index": "3"
            }),
        ee.Feature(
            ee.Geometry.Point([131.1187306474289, -13.001316099681986]),
            {
              "label": 2,
              "system:index": "4"
            }),
        ee.Feature(
            ee.Geometry.Point([131.18386103263126, -12.923357579395338]),
            {
              "label": 2,
              "system:index": "5"
            }),
        ee.Feature(
            ee.Geometry.Point([131.19834512651713, -12.929281485991318]),
            {
              "label": 2,
              "system:index": "6"
            }),
        ee.Feature(
            ee.Geometry.Point([131.20411560701675, -13.088081334454237]),
            {
              "label": 2,
              "system:index": "7"
            }),
        ee.Feature(
            ee.Geometry.Point([131.21474471962466, -13.314812701897955]),
            {
              "label": 2,
              "system:index": "8"
            }),
        ee.Feature(
            ee.Geometry.Point([131.21742692863955, -13.311492619806799]),
            {
              "label": 2,
              "system:index": "9"
            }),
        ee.Feature(
            ee.Geometry.Point([131.21397224342837, -13.311722312486419]),
            {
              "label": 2,
              "system:index": "10"
            }),
        ee.Feature(
            ee.Geometry.Point([131.21498075401797, -13.317610723148762]),
            {
              "label": 2,
              "system:index": "11"
            }),
        ee.Feature(
            ee.Geometry.Point([131.22268405830874, -13.308130729294696]),
            {
              "label": 2,
              "system:index": "12"
            }),
        ee.Feature(
            ee.Geometry.Point([130.98737365102932, -12.995148653326343]),
            {
              "label": 2,
              "system:index": "13"
            }),
        ee.Feature(
            ee.Geometry.Point([130.9914935240762, -12.996235873289889]),
            {
              "label": 2,
              "system:index": "14"
            }),
        ee.Feature(
            ee.Geometry.Point([130.9927380690591, -12.994375051606996]),
            {
              "label": 2,
              "system:index": "15"
            }),
        ee.Feature(
            ee.Geometry.Point([130.99344617223903, -12.997427628164242]),
            {
              "label": 2,
              "system:index": "16"
            }),
        ee.Feature(
            ee.Geometry.Point([130.9896052489297, -12.998222128235142]),
            {
              "label": 2,
              "system:index": "17"
            }),
        ee.Feature(
            ee.Geometry.Point([130.9858930716531, -12.99884936333683]),
            {
              "label": 2,
              "system:index": "18"
            }),
        ee.Feature(
            ee.Geometry.Point([130.98325377798244, -12.996800389469884]),
            {
              "label": 2,
              "system:index": "19"
            }),
        ee.Feature(
            ee.Geometry.Point([131.03458189750657, -13.047095208940334]),
            {
              "label": 2,
              "system:index": "20"
            }),
        ee.Feature(
            ee.Geometry.Point([131.0310735681151, -13.048213556433561]),
            {
              "label": 2,
              "system:index": "21"
            }),
        ee.Feature(
            ee.Geometry.Point([131.02090263153062, -13.055331144800846]),
            {
              "label": 2,
              "system:index": "22"
            }),
        ee.Feature(
            ee.Geometry.Point([130.94233368395612, -12.773800576790546]),
            {
              "label": 2,
              "system:index": "23"
            })]),
    ns = 
    /* color: #ffc82d */
    /* shown: false */
    ee.FeatureCollection(
        [ee.Feature(
            ee.Geometry.Point([130.88354021048255, -12.93022823252853]),
            {
              "label": 3,
              "system:index": "0"
            }),
        ee.Feature(
            ee.Geometry.Point([130.9916173467434, -13.236767583361024]),
            {
              "label": 3,
              "system:index": "1"
            }),
        ee.Feature(
            ee.Geometry.Point([131.26824777711434, -13.321914325262272]),
            {
              "label": 3,
              "system:index": "2"
            }),
        ee.Feature(
            ee.Geometry.Point([131.21925514951909, -13.161309821663778]),
            {
              "label": 3,
              "system:index": "3"
            }),
        ee.Feature(
            ee.Geometry.Point([131.1783074171386, -13.093219222317138]),
            {
              "label": 3,
              "system:index": "4"
            }),
        ee.Feature(
            ee.Geometry.Point([131.05042334523364, -12.76886377015025]),
            {
              "label": 3,
              "system:index": "5"
            }),
        ee.Feature(
            ee.Geometry.Point([131.1601961005419, -13.157516428622664]),
            {
              "label": 3,
              "system:index": "6"
            }),
        ee.Feature(
            ee.Geometry.Point([131.22290168275362, -13.261761091123152]),
            {
              "label": 3,
              "system:index": "7"
            }),
        ee.Feature(
            ee.Geometry.Point([131.15961868503572, -13.092703727379053]),
            {
              "label": 3,
              "system:index": "8"
            }),
        ee.Feature(
            ee.Geometry.Point([131.28250919911244, -13.27238400033023]),
            {
              "label": 3,
              "system:index": "9"
            }),
        ee.Feature(
            ee.Geometry.Point([131.28529869648793, -13.27314628447919]),
            {
              "label": 3,
              "system:index": "10"
            }),
        ee.Feature(
            ee.Geometry.Point([131.2849875602422, -13.272394442595012]),
            {
              "label": 3,
              "system:index": "11"
            }),
        ee.Feature(
            ee.Geometry.Point([130.982525085958, -13.15789900719834]),
            {
              "label": 3,
              "system:index": "12"
            }),
        ee.Feature(
            ee.Geometry.Point([131.00876128073384, -12.982261818216562]),
            {
              "label": 3,
              "system:index": "13"
            }),
        ee.Feature(
            ee.Geometry.Point([131.01215159292866, -12.984331821062414]),
            {
              "label": 3,
              "system:index": "14"
            }),
        ee.Feature(
            ee.Geometry.Point([131.19086054461258, -12.967619191387925]),
            {
              "label": 3,
              "system:index": "15"
            }),
        ee.Feature(
            ee.Geometry.Point([131.18008879320877, -12.964189855929051]),
            {
              "label": 3,
              "system:index": "16"
            })]),
    nav = 
    /* color: #1bd6c7 */
    /* shown: false */
    ee.FeatureCollection(
        [ee.Feature(
            ee.Geometry.Point([131.25678713106134, -12.99257978300432]),
            {
              "label": 4,
              "system:index": "0"
            }),
        ee.Feature(
            ee.Geometry.Point([131.19256870861068, -13.177550328472982]),
            {
              "label": 4,
              "system:index": "1"
            }),
        ee.Feature(
            ee.Geometry.Point([131.1641158353807, -13.20132501438157]),
            {
              "label": 4,
              "system:index": "2"
            }),
        ee.Feature(
            ee.Geometry.Point([131.1183001711305, -13.22621980547928]),
            {
              "label": 4,
              "system:index": "3"
            }),
        ee.Feature(
            ee.Geometry.Point([131.0745532107756, -13.298723544424618]),
            {
              "label": 4,
              "system:index": "4"
            }),
        ee.Feature(
            ee.Geometry.Point([131.12898094584992, -12.860932200442624]),
            {
              "label": 4,
              "system:index": "5"
            }),
        ee.Feature(
            ee.Geometry.Point([130.9832726233249, -12.844332133439568]),
            {
              "label": 4,
              "system:index": "6"
            }),
        ee.Feature(
            ee.Geometry.Point([130.96211971356234, -12.991237262520045]),
            {
              "label": 4,
              "system:index": "7"
            }),
        ee.Feature(
            ee.Geometry.Point([130.98450154983178, -13.041853855071327]),
            {
              "label": 4,
              "system:index": "8"
            }),
        ee.Feature(
            ee.Geometry.Point([131.13091797553673, -13.01989292450605]),
            {
              "label": 4,
              "system:index": "9"
            })]),
    ctv = 
    /* color: #30ff3e */
    /* shown: false */
    ee.FeatureCollection(
        [ee.Feature(
            ee.Geometry.Point([130.94133799396843, -12.837484924655465]),
            {
              "label": 5,
              "system:index": "0"
            }),
        ee.Feature(
            ee.Geometry.Point([130.93728118148837, -12.835599458532075]),
            {
              "label": 5,
              "system:index": "1"
            }),
        ee.Feature(
            ee.Geometry.Point([130.94560639835575, -12.813784084119938]),
            {
              "label": 5,
              "system:index": "2"
            }),
        ee.Feature(
            ee.Geometry.Point([130.94789164043644, -12.813637621041616]),
            {
              "label": 5,
              "system:index": "3"
            }),
        ee.Feature(
            ee.Geometry.Point([130.94571368671635, -12.814903477691656]),
            {
              "label": 5,
              "system:index": "4"
            }),
        ee.Feature(
            ee.Geometry.Point([130.94865338779667, -12.81530101813692]),
            {
              "label": 5,
              "system:index": "5"
            }),
        ee.Feature(
            ee.Geometry.Point([130.95558301704227, -12.773322791741407]),
            {
              "label": 5,
              "system:index": "6"
            }),
        ee.Feature(
            ee.Geometry.Point([130.95888749854862, -12.773092598527372]),
            {
              "label": 5,
              "system:index": "7"
            }),
        ee.Feature(
            ee.Geometry.Point([130.95601217048466, -12.77445282811469]),
            {
              "label": 5,
              "system:index": "8"
            }),
        ee.Feature(
            ee.Geometry.Point([130.95306174056827, -12.77353205811768]),
            {
              "label": 5,
              "system:index": "9"
            }),
        ee.Feature(
            ee.Geometry.Point([131.01334267602468, -12.795998453965234]),
            {
              "label": 5,
              "system:index": "10"
            }),
        ee.Feature(
            ee.Geometry.Point([131.01412588105703, -12.797787515205936]),
            {
              "label": 5,
              "system:index": "11"
            }),
        ee.Feature(
            ee.Geometry.Point([131.09145183226164, -12.79123806990156]),
            {
              "label": 5,
              "system:index": "12"
            }),
        ee.Feature(
            ee.Geometry.Point([131.09232086798247, -12.791609491251194]),
            {
              "label": 5,
              "system:index": "13"
            }),
        ee.Feature(
            ee.Geometry.Point([131.0956921921419, -12.791511301031944]),
            {
              "label": 5,
              "system:index": "14"
            }),
        ee.Feature(
            ee.Geometry.Point([131.1990246967148, -12.920512110638699]),
            {
              "label": 5,
              "system:index": "15"
            }),
        ee.Feature(
            ee.Geometry.Point([131.19974352873078, -12.919612790259603]),
            {
              "label": 5,
              "system:index": "16"
            }),
        ee.Feature(
            ee.Geometry.Point([131.18337287059055, -12.9223040267928]),
            {
              "label": 5,
              "system:index": "17"
            })]);
print(ctv)



//1. ***DEFINE STUDY SITES *******************************************

//define validation site
var validationSite = 
    ee.Geometry.Polygon(
        [[[131.48332608933882, -12.836025763520011],
          [131.70442594285444, -12.841381595591324],
          [131.71541227097944, -12.653859887583241],
          [131.49156583543257, -12.65519981958509]]])

//define the project bounds or area
var projectArea = 
    ee.Geometry.Polygon(
        [[[130.84749234910444, -13.385733180411423],
          [131.29243863816694, -13.396420811767117],
          [131.30891813035444, -12.76237152348455],
          [130.8557320951982, -12.767728920232042]]]);

//*****************************************************************
//2. ***GET THE REMOTE SENSING DATA*******************************************

//get the satellite imagery
var s2 = ee.Image('COPERNICUS/S2_SR_HARMONIZED/20240428T013701_20240428T013722_T52LGL')

//create a region of interest from the extent of the image
var geom = s2.geometry()
var roi = ee.FeatureCollection(geom).style({
  fillColor: '00000000'})//;
print(roi, 'roi')
Map.addLayer(roi, {}, 'ROI')
//select the required bands
var s2 = s2.select(["B2","B3","B4","B5","B6","B7","B8","B11", "B12"])

//trim the image to the project area
var s2ProjectArea = s2.clip(projectArea)
print(s2ProjectArea, "s2ProjectArea")
Map.addLayer(s2ProjectArea, {bands:["B4", "B3", "B2"], min:200, max:2000}, "S2 TCC Project Area")

//my own code starts here
var samp1 =s2ProjectArea.sample({
  region:projectArea, 
  scale:20, 
  numPixels:120, 
  seed: 111,
  geometries: true})

print(samp1, 'samples 1')
Map.addLayer(samp1, {color: ' red'}, 'samp1')

//merge cover classes
var coverTypes = ntv.merge(as).merge(ns).merge(ctv).merge(nav).merge(water)
print(coverTypes, "coverTypes")

//remap classes, as some classes are spectrally similar and rare in the image
var coverTypes2 = coverTypes.remap([0, 1,2,3,4,5],[0,1,2,2,1,3], 'label')
//the labels then become:water(0), ntv and nav(1), as and ns(2), and ctv(3)

//print the sampleClass to the Console
print(coverTypes2, 'Cover Types 2')
Map.addLayer(coverTypes2, {color: 'black'}, 'Calibration Sites')

//create reference areas by assigning the spectral to sample points
var referencePoints = s2ProjectArea.sampleRegions({
  collection: coverTypes2, //the feature collection
  properties: ['label'],
  scale: 20 //pixel size for the output image
});

print(referencePoints, 'referencePoints')
//create a CART model
var cartClassifier = ee.Classifier.smileCart() //sets up the classifier

//trains the classifier 
.train({
  features: referencePoints,
  classProperty: 'label',
  inputProperties: s2ProjectArea.bandNames() //this is the predictor variables
});


//apply the trained CART classifier to the image
var s2ClassifiedCART = s2ProjectArea.classify(cartClassifier);

//define the palettes for the cover classes
var classColours = {
  min: 0,
  max: 3,
  palette: ['blue', 'green', 'purple', 'lightgreen']
};

//display the original imagery in true colour combination
Map.setCenter(131.3815, -12.9111, 10);
Map.addLayer(s2ProjectArea, {bands: ['B4', 'B3', 'B2'], min: 0, max: 3000}, 'Original S2 imagery');

//display the classified image
Map.addLayer(s2ClassifiedCART, classColours, 'Classified S2 CART imagery');

//create a random forest model
// parameterise a Random Forest classification algorithm
var rfClassification = ee.Classifier.smileRandomForest({
  numberOfTrees: 500,
  bagFraction: 0.6
}).train({
  features: referencePoints,  
  classProperty: 'label',
  inputProperties: s2ProjectArea.bandNames()
});


//apply the RF classifier to the image
var s2ClassifiedRF = s2ProjectArea.classify(rfClassification);
print(s2ClassifiedRF, "s2Classified Using RF")

//display the classified image
Map.addLayer(s2ClassifiedRF, classColours, 'Classified S2 imagery-RF');

Export.image.toDrive({
    image: s2ClassifiedRF.visualize(classColours), //keep colours for the cover classes
    description: 'RF_Classification', //file name, so you can readily identify this in your Google Drive
    scale: 20, //pixel size; this maybe increased if the file size is large 
    maxPixels: 1e13 //to increase the number of pixels allowed by default
});

```


## DIY


A Sentinel-2 image surface reflectance product obtained from the Earth Engine catalog is given as: **COPERNICUS/S2_SR_HARMONIZED/20240428T013701_20240428T013722_T52LGL**. A client interested in identifying cleared lands would like to understand the dsitribution of the dominant land cover classes. Classify the image into four land cover classes, NTV, water, bareland, CTV using k-nearest neighbor algorithm (k-NN) algorithms. 


## Conclusion and next activity

We have covered supervised classification of image pixels into land cover themes using CART and Random Forest. The classified images were qualitatively appraised using the original imagery. The next activity will cover the quantitative assessment of the RF model using a set of label data obtained through stratified random sampling. Accuracy assessment metrics, including overall accuracy, user's accuracy, and producer's accuracy, will be manually computed from an error matrix.
