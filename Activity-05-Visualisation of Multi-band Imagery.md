# Activity 5: Visualisation of Multi-band Imagery

## Introduction

Multi-band imagery is a common remote sensing data as many remote sensors acquire images from the visible and infrared regions of the electromagnetic spectrum. A multi-band image that contains visible and infrared bands makes it possible for humans to visualise features intuitively and non-intuitively through true colour composite and false colour composite, respectively. Since the spectral configurations of remote sensors can be highly variable (sensors having different arrangements/number of bands), the construction of true colour composite and false colour composite images may differ between sensors. Thus, the aim of this practical is to explain true colour composite (TCC) and false colour composite (FCC) and produce a TCC and FCC image using Landsat 9 data. The activity assumes that you are able to create a new script file.

## Learning Outcomes
- Select image bands
- Produce a true colour composite
- Produce a false colour composite
-  Differentiate between true colour and false colour images

## Activity
Landsat provides free-to-use satellite imagery for the public. The mission has existed since the early 70’s. Landsat 8 and 9 are the most recent missions and are strongly comparable in that both sensors have eleven standard bands, of which eight represent reflected energy. We will work through the standard reflective bands of a Landsat 9 image. The image file name is `LANDSAT/LC09/C02/T1_TOA/LC09_104069_20230511`. 
We would load the image into the Code Editor using the script below.

### Load a multispectral image 
```JavaScript
//load in the Landsat 9 image using ee.Image()
var lsat9 = ee.Image('LANDSAT/LC09/C02/T1_TOA/LC09_104069_20230511');
```
What is the name of the variable? You are right it is lsat9. Print this variable to the **`Console`** to explore the metadata of the image. In the Console, click the small triangle icon to expand and reveal the information associated with the data. Your result should be as shown below. 

![image](https://github.com/user-attachments/assets/d9ef9a3e-1056-4529-96cf-113b0ce7221e)|

### What is in the Landsat data?
It is important to always understand your data to be able to manipulate it. Landsat has a conventional filename, which includes satellite, collection number, collection category, and acquisition date. In this Landsat image, the filename is captured as “id” in Fig. L = Landsat, C = Combined, meaning data includes bands from the operational land imager (OLI) sensor and thermal infrared sensor (TIRS), 09 = data from the Landsat 9 satellite, C02= collection 2, T1 = Tier 1 and top of atmosphere (TOA) product. Landsat has a unique ID for every image scene; this ID is known as Path and Row. Path is column pixels and Row is horizontal pixels. In this data, the 104069 represents this path/row ID in that the first 3 numbers (104) is the path ID and the last 3 numbers (069) is the row ID. The date on which the image was acquired is given as YYYYMMDD. In this data, the image was acquired on 20230511 (i.e., 11th May 2023). 
The image has 17 bands in total, and if you expand the “bands” arrow, you should see this list of bands (shown in the figure below), including the quality assessment bands (QA), solar azimuth angle (SAA), view zenith angle (VAA), and view zenith angle (VZA) used for pre-processing analysis. 


![image](https://github.com/user-attachments/assets/138db4e9-e62c-4109-8acf-584befbba47a)


The image is also made of eleven standard bands (B1-B11), but in this activity, we will explore B2-B7 as they contain the reflected visible and infrared energy about the earth. Further information about the data can be seen if you expand the “properties” arrow. An important information in the “properties” is the percentage of cloud cover in the image scene.

### Select image bands

It is not always that all the image bands are required for a project. Thus, the `ee.Image.select()` method can be used to select relevant bands only. In this activity, we used this method to select `B2, B3, B4, B5, B6, B7`. The below code selects the bands, but keeps the same variable name (lsat9). This means that lsat9 was updated to contain 6 bands rather than 17, and thus further use of lsat9 shows only 6 bands.

```JavaScript
//select image bands
var lsat9 = lsat9.select('B2', 'B3', 'B4', 'B5', 'B6', 'B7');

//print the variable to the Console to confirm that the image now has 6 bands
print(lsat9);
```


### Produce a true colour composite

Although a multispectral imagery would contain more than three bands, the computer can display a maximum of three bands at a time. The bands of the image can be displayed to reflect human vision in that we would perceive features in their natural colours. For example, if you look down on features from an airborne vehicle, the colours in which you see these features in real life would be presented in a true colour display. And this image visualisation method is referred to as true colour composite. In true colour composite, humans would see a healthy vegetation as green, soil might be reddish/brownish etc. This is because the true colour composite displays the light visible to humans. It is *true* colour composite because the image from the red channel of the mechanical sensor is assigned red, the green channel is assigned green, and the blue channel is assigned blue (Fig. 1).




![image](https://github.com/user-attachments/assets/f00db0a8-3e00-401d-9b88-6e257bfb4584) |
|:--:|
| *Fig. 1. True colour composite.*|



For Landsat 8 and 9 the image captured using blue light is band 2 (B2), green light is band 3 (B3), and red light is band 4 (B4). More information on the bands of the Landsat family might be found [here](https://www.usgs.gov/faqs/what-are-band-designations-landsat-satellites). To construct a true colour composite for Landsat 9 data, you would assign B4 to the red channel, B3 to the green channel and B2 to the blue channel. The code below produces a true colour composite of our chosen image data. 


```JavaScript
Map.addLayer(lsat9,{
bands: ['B4', 'B3', 'B2'],
min:0,
max:1},
'True Colour Composite 1');
```


To explain the above code, the **lsat9** is the image to display while for the visualisation parameters the bands were defined in the order of RGB channels (B4, B3, B2, respectively) as this corresponds to the natural colour vision of humans if for example we look down at the objects from an aircraft 300 m aloft. The reflectance value range between 0 and 1, and you might be able to find the range of values using the Inspector tool. Unlike a single-band image, a palette is not required as the computer, by default, assigns red to B4, green to B3, and blue to B2. The layer name is specified as “True Colour Composite 1”. Run this code and explore the image displayed in the base map environment. Navigate to different surface types, including vegetation, water, and built-up areas. You might observe that the image scene has low contrast, and this is because the default reflectance range specified does not match the actual range of reflectance recorded by the sensor. You can correct this by modifying the min-max values. A trial and error method can be employed to achieve the optimal range, but in this activity, given the image acquisition date aligns with a dry season of the region of interest, the reflectance values, especially for vegetation, are envisaged to be low. Thus, we can edit the code, as shown below. 

```JavaScript
Map.addLayer(lsat9,{
bands: ['B4', 'B3', 'B2'],
min:0,
max:0.3},
'True Colour Composite 2');
```

The min-max values and the layer name were modified. Run the code and compare the two layers in the layer manager. The second layer was more visually enhanced as the contrast improved.


![image](https://github.com/user-attachments/assets/5ebf5a07-87c0-4da7-a489-c1fa51b7ae52) |
|:--:|
| *Fig. 2. A true colour composite of a Landsat 9 image (B4, B3, B2). The left image has a default range (not enhanced) while the right image is enhanced modifying the min-max values.*|



### Produce a false colour composite

To utilise the many capabilities of remote sensing, we cannot only rely on the light visible to the human eye (i.e., RGB light). Remote sensing detectors employ infrared light, which provides detailed and unique information about the environment, but unfortunately, this light is invisible to the human eye. Using computers, we can visualise infrared information about our environment through a technique known as false colour composite. False colour composite is a deviation from the display of features to match how humans see the features in real world. The false colour composite is carried out to leverage infrared information. Thus, a false colour composite always includes an infrared band, and a multitude of this composite can be produced. However, a standard or commonly used false colour composite is assigning the sensor’s green channel to blue, red channel to green, and near-infrared channel to red, as shown below. 


![image](https://github.com/user-attachments/assets/4cddcb75-bf8b-41e7-870a-9d01566599df) |
|:--:|
| *Fig. 3. A standard false colour band combination.*|



The standard false colour composite is usually produced to visually discriminate different types of vegetation features in the image, as vegetation is highly responsive to near-infrared light. The rationale for constructing a false colour composite should be focused on the surface type you aim to emphasise. To produce a standard false colour composite for the Landsat 9 data, you must assign B5 to red, B4 to green, and B3 to blue. This code is given below.

```JavaScript
Map.addLayer(lsat9,{
bands: ['B4', 'B3', 'B2'],
min:0,
max:0.3},
'True Colour Composite 2');
```

```JavaScript

//a false colour composite
Map.addLayer(lsat9,{
bands: ['B5', 'B4', 'B3'],
min:0,
max:0.3},
'False Colour Composite');
```

The code, if run, produces the following image with predominant shades of red suggesting that the scene is covered by different types or conditions of vegetation. Darker red pixels show low reflectance vegetation while the lighter red pixels show high reflectance.


![image](https://github.com/user-attachments/assets/8876a117-dcbe-4df1-bdd6-01de7b6b6315) |
|:--:|
| *Fig. 4. False colour composite of Landsat 9 (B5, B4, B3).*|




## DIY 
A Landsat 8 image file is given as: 
`LANDSAT/LC08/C02/T1_TOA/LC08_094074_20220611`. <br>
1, Load the image and select the first seven bands. <br>
2, Print your variable to the Console and explore the image properties. What is the image acquisition date and the percentage of cloud cover? <br>
3, If the range of reflectance is 0 and 0.5, produce a true colour composite (TCC) image and a standard false colour (FCC) image. <br>
4, Explain why the colour of the ocean water in TCC and FCC appear similar or different.




## Conclusion

In this chapter, we have learned about true colour composite and false composite images. Landsat 8 and 9 images were explored to explain the difference between true colour and false colour images. Although a single-date multispectral image was explored in this activity, the use of multi-temporal images is not uncommon. In the next chapter, we will work with a stack of images collected at different times.


