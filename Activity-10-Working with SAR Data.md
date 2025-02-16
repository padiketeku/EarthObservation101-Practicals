# Activity 10: Working with SAR Data

Synthetic Aperture Radar (SAR) is the manipulation of the movement of the remote sensing platform to prodice high resolution imagery. In this practical, you will search for and visualise Sentinel-1 SAR imagery. You will validate the Sentinel-1 SAR imagery using optical imagery. 


## Introduction


Satellite-based synthetic aperture radar (SAR) data complements multispectral data. However, the SAR data becomes more important for study areas that are frequently covered by clouds. The SAR systems have active sensors that employ microwave energy, which readily penetrates through cloud cover and provides data regardless of the time of the day. A spaceborne SAR system has an antenna(s) that transmits and receives artificial microwave energy. The microwave energy used by SAR systems is fully controlled or polarised. Currently the spaceborne SAR systems operate a single band of microwave light, but measure the different polarisations of the light waves. A linear polarisation detected by the SAR system is denoted by the transmitted and received orientations of the wave such as VV, HH, VH, or HV. The first letter is the orientation of the transmitted energy while the latter denotes  the orientation of the received energy. The figure below explains the four linear polarisation data a SAR system may produce.



![image](https://github.com/user-attachments/assets/7ab09a24-e2de-443e-a2c2-a1a9a71abd65)




For instance, a VH polarisation record means the system transmitted the microwave energy in a vertical orientation while the return energy was received horizontally. The intensity of the return energy is measured by the receiving antenna, and is computed as a *backscatter coefficient* usually transformed to decibels (dB) for the purposes of visualisation (contrast enhancement).



## Learning Outcomes

At the end of this acitivity you should be able

- Collect Sentinel-1 SAR imagery

- Visualise Sentinel-1 SAR imahery

- Validate/interppret Sentinel-1 SAR imagery



## Activity Begins


Because SAR is a single-band system, the images are grayscale with brighter and darker pixels showing high and low backscatter values, respectively. In this task you would visualise Sentinel-1 SAR image of Darwin city and your result is expected to be similar to the one below. Note, there might be variation in results as your region of interest geometry might not be the same as the one used to make this practical. Nevertheless, make sure Darwin city can be found in the SAR image you choose.


