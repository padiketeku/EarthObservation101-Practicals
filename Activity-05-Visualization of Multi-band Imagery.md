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
```JavaScript`
//load in the Landsat 9 image using ee.Image()
var lsat9 = ee.Image('LANDSAT/LC09/C02/T1_TOA/LC09_104069_20230511');
```
What is the name of the variable? You are right it is lsat9. Print this variable to the `**Console**` to explore the metadata of the image. In the Console, click the small triangle icon to expand and reveal the information associated with the data. Your result should be as shown below (Figure 3.1). 

![image](https://github.com/user-attachments/assets/d9ef9a3e-1056-4529-96cf-113b0ce7221e)|




