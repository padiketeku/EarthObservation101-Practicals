# Activity 5: Visualization of Multi-band Imagery

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
print(lsat);
```


### Produce a true colour composite

Although a multispectral imagery would contain more than three bands, the computer can display a maximum of three bands at a time. The bands of the image can be displayed to reflect human vision in that we would perceive features in their natural colours. For example, if you look down on features from an airborne vehicle, the colours in which you see these features in real life would be presented in a true colour display. And this image visualisation method is referred to as true colour composite. In true colour composite, humans would see a healthy vegetation as green, soil might be reddish/brownish etc. This is because the true colour composite displays the light visible to humans. It is *true* colour composite because the image from the red channel of the mechanical sensor is assigned red, the green channel is assigned green, and the blue channel is assigned blue (Fig. 1).


![image](https://github.com/user-attachments/assets/f00db0a8-3e00-401d-9b88-6e257bfb4584)

