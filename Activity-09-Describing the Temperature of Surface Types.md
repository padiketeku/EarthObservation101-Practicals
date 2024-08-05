# Activity 9: Describing the Temperature of Surface Types

In this practical, you will explore MODIS land surface temperature (LST) to understand temperature of surface types. Also, you will explore the relationship between temperature and vegetation by plotting MODIS NDVI against MODIS LST. The MODIS NDVI will be derived from the surface reflectance product. The hypothesis behind this activity is that high vegetation areas can be shady and would have lower temperatures compared to deserts or low vegetation areas. 
Assumes knowledge of previous actvities and that you can create spectral indices, including NDVI.

## Introduction
Temperature is an environmental parameter useful for the monitoring of the biosphere, hydrosphere, atmosphere, and lithosphere. In-field measurement of temperature can be expensive and time-consuming, while thermal remote sensing provides a more cost-efficient method for a rapid collection of temperature data. Passive remote sensing systems utilising visible, near-infrared, and microwave sensors record reflected energy. In contrast, thermal sensors detect the temperature of materials by recording the emitted energy. The energy emitted is a product of the incident radiation the materials absorbed. Ideally, the more energy the materials absorb (hotter materials) the more energy they can emit.


![image](https://github.com/user-attachments/assets/c808f19c-dca0-4ab1-b787-62d953088403)




The materials we see in our environments, such as rocks, vegetation, buildings do not emit all the absorbed radiation as a theoretical blackbody (absorbs all incident radiation and emits all radiation) would. Surface types emit a fraction of the radiation emitted by a blackbody at the equivalent temperature. The fraction is called `emissivity` (ε); which ranges between 0 and 1. The imaginary blackbody has an emissivity of 1, while real-world materials have emissivity less than 1. The temperature a remote sensing detector records is not only driven by the **temperature** of the object but also **emissivity**.


E =  εσT<sup> 4</sup>  (Stefan-Boltzmann Law); 

where E = total energy radiated per unit surface area of a black body across all wavelengths per unit time, σ =  Stefan-Boltzmann Constant of 5.6697 x 10-8 W m<sup> -2</sup> K<sup>- 4</sup>, and T = absolute temperature (K). 

The temperature we measure in the field with thermometers is the true temperature, aka, kinetic temperature. The remote sensing system detects radiant temperature, a descriptive measure of radiation regarding the assumption that the temperature of a blackbody emits an identical amount of radiation at the same wavelength. There is a strong correlation between kinetic temperature and radiant temperature, which makes remote sensing an important technology for monitoring the temperature of surface materials. The temperature of surface materials, referred to as land surface temperature (LST), is a product from the remote sensing measurement. The remote sensing detector records radiance, which is converted to radiant temperature using Planck’s equation. The radiant temperature is converted to LST (in Kelvin) by accounting for emissivity and atmospheric effects.
Satellite remote sensing systems that collect temperature data for public use include Landsat, Moderate-resolution Imaging Spectroradiometer (MODIS), and Advanced Spaceborne Thermal Emission and Reflection Radiometer (ASTER).


## Learning Outcomes

At the end of this activity, you should be able to: <br>
- Define a polygonal region of interest using a list of coordinates
- Display temperature data for visualisation
- Convert absolute temperature to celsius 
- Change the projection and scale of images
- Perform correlation analysis
- Produce image histograms
- Produce image scatter plot


## Activity Begins

Thermal remote sensing has a wide application areas, including archaeology (e.g., https://weather.ndc.nasa.gov/archeology/chaco_compare.html), drought monitoring (e.g., https://lpdaac.usgs.gov/resources/data-action/two-sensors-are-greater-one-observing-drought-smap-and-modis/), and urban heat islands. 

In this practical you are required to use publicly available satellite imagery to confirm this hypothesis by exploring the relationship between vegetation and temperature. The region of interest is given by the list of coordinates below. <br>
[142.06594410835254,-17.94898241151206,
145.38930836616504,-17.94898241151206,
145.38930836616504,-16.005194143426337,
142.06594410835254,-16.005194143426337,
142.06594410835254,-17.94898241151206]

Also, you are required to restrict the analysis to September 2021 as the study area is on record for experiencing extremely high temperatures on this date.
