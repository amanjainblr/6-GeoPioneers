**Drive link for the GIS dataset (Shapefiles & Raster files) :-**  https://drive.google.com/drive/folders/198lxpw8xbVF0W500W1fZD6gv-IBLFyOj?usp=sharing
# 6-GeoPioneers
**Topic Name:- Integrating PyQGIS & Geospatial Technology for Soil Erosion Modelling Using Revised Universal Soil Loss Equation (RUSLE)**

**Soil Loss at Upper Krishna Basin:-** The Upper Krishna Basin in Maharashtra and Karnataka, India, is a region plagued by soil erosion. The application of GIS technologies and the Revised Universal Soil Loss Equation (RUSLE) allows for the prediction and management of soil erosion in the region. According to our findings, the expected soil loss in the Upper Krishna Basin is roughly 2.02 tonnes per acre per year. This level of soil loss is worisome because it can result in decreased soil fertility and productivity, increased sedimentation in rivers and streams, and downstream flooding. Soil loss can also contribute to decreasing water quality, which can have an impact on agriculture and drinking water supplies in the region. As a result, there is an urgent need in the region for sustainable soil management practises. The RUSLE model, built with PyQGIS and geospatial technologies, is a useful tool for predicting soil erosion and assessing the efficacy of various soil conservation practises. The output of the model can be used to prioritise areas for soil conservation activities and indicate areas where soil erosion mitigation measures are required. Overall, the Upper Krishna Basin's estimated soil loss of 2.02 tonnes per acre per year underscores the region's need for appropriate soil management practises. The RUSLE model, when combined with GIS technology, provides an effective tool for predicting and regulating soil erosion in the region, thereby helping to the long-term management of soil resources and environmental protection.

**Data Sources**

**1. Land Use Land Cover** :- ESRI Sentinel Product
**2. Digital Elevation Model** :- NASA SRTM
**3. Soil Data** :- Food & Agriculture Organization Global Soil Data
**4. Rainfall Data** :- CHRIPS Rainfall Data

**Methodology:-**

![image](https://user-images.githubusercontent.com/132006776/235300022-75e20a86-5ea5-4e36-a9c9-93fca314b903.png)

**1. C Factor:-** The C factor in the RUSLE (Revised Universal Soil Loss Equation) model represents the Cover and is used to estimate the amount of soil erosion caused by rainfall and runoff. The C factor takes into account the soil's susceptibility to erosion, based on factors such as soil texture, structure, organic matter content, and permeability. A higher C factor indicates a greater potential for soil erosion, while a lower C factor indicates a lower potential for soil erosion. The C factor is expressed as a dimensionless value between 0 and 1, with higher values indicating greater erodibility. The C factor can be estimated based on soil type, land use, and management practices. The United States Department of Agriculture (USDA) has published tables and maps that provide estimates of the C factor for different soil types and land uses, which can be used in the RUSLE model. The detailed tabulated Values are submitted in Report.

**2. K Factor:-** The soil erodibility factor (K) indicates the degree of surface soil erosion. The most important parameter for determining the K factor is soil texture. In along with the permeability of the soil, organic matter, and soil structure, several elements are important when assessing the K factor. The value of K represents the rate of soil erosion per rainfall permeability index (R). The formula & detailed explanation is submitted in Report.

**3. LS Factor:-** In the revised universal sol loss equation (RUSLE), the LS factor describes the influence of slope as well as slope length upon soil erosion. It is the ratio of soil loss from a specific area of land to the quantity of soil loss from an even surface with the same vegetation and soil cover. The LS component is a dimensionless quantity with a value between 0 and infinity, with greater values suggesting a higher risk of soil loss. The LS factor can be determined using digital terrain models (DEMs) or topographical maps that include slope angle and slope length information. It should be noted that the LS factor is a location-specific characteristic that must be computed for each soil and landscape based on topographic aspects of the land surface. Furthermore, factors like land use, vegetation cover, and erosion control practises can all have an impact on the LS factor. As a result, proper measurement and evaluation of these factors are required for accurate determination of the LS factor.The formula & detailed explanation is submitted in Report.

**4. R Factor:-** The R factor in the Revised Universal Soil Loss Equation (RUSLE) is a rainfall erosivity factor that describes the potential of rainfall to cause soil erosion. It is a measure of the kinetic energy of raindrops, which is a primary driver of soil erosion. The R factor as mentioned in Equation is expressed in MJ mm ha-1 h-1 yr-1 units and determines the quantity of erosive power contained in a particular amount of rainfall. A greater R factor indicates rainfall is more erosive, making soil erosion more likely. Rainfall data, which includes rainfall records, intensity of rainfall data, or rainfall simulations, can be used to calculate the R factor. Furthermore, empirical formulae or geographic information systems (GIS) may be employed to calculate the R factor from local rainfall data or to calculate R factor values for locations where direct rainfall data is unavailable. It should be noted the R factor is a location-specific variable that must be computed for each individual place according to local rainfall variables. Furthermore, seasonality, hurricane duration, and distribution of intensity can all alter the R factor, which should be taken into account when employing the RUSLE equation to measure soil erosion.The formula & detailed explanation is submitted in Report.

**5. P Factor: -** The conservation support practise factor (P) compares the rate of soil loss from support practises up and down the slope to that of straight-row farming up and down the slope to account for the beneficial benefits of support practises. It reduces the possibility for runoff erosion by influencing runoff concentration, drainage pattern, runoff velocity, and runoff hydraulic pressures on the soil. P-factor values for various land uses were calculated using the standard table given below. The raster calculator has been used to assign values to the various LULC classes, then the P factor map has been created. P values vary from 0 to 1, with larger values indicating ineffective conservation efforts and lower values indicating effective conservation efforts.  The detailed tabulated Values are submitted in Report.

The following Raster files wer computed using the basic tools in QGIS such as Reclassify, Clip etc Then the following Raster files were encoded withing Python Code which uses PyQGIS library of QGIS for Computation of Soil loss which is given by following Formula
                                                            
                                                            Soil loss = C * K * LS * R * P
 
 **Python Code for Computation of Soil loss by Multiplying each factor using Raster Calculator**
 
 #Import necessary libraries
from qgis.analysis import QgsRasterCalculatorEntry, QgsRasterCalculator
from pathlib import Path

* *#Define the raster layers for each RUSLE factor
raster_factors = {
    'R': 'E:/Upper_Krishna_Basin_RUSLE_Model/Raster_files/Soil_Loss_Factors/R_Factor.tif',
    'K': 'E:/Upper_Krishna_Basin_RUSLE_Model/Raster_files/Soil_Loss_Factors/K_Factor.tif',
    'LS': 'E:/Upper_Krishna_Basin_RUSLE_Model/Raster_files/Soil_Loss_Factors/LS_Factor.tif',
    'C': 'E:/Upper_Krishna_Basin_RUSLE_Model/Raster_files/Soil_Loss_Factors/C_Factor.tif',
    'P': 'E:/Upper_Krishna_Basin_RUSLE_Model/Raster_files/Soil_Loss_Factors/P_Factor.tif'
}

#Define the path to the output raster
output_raster = 'E:/Upper_Krishna_Basin_RUSLE_Model/Raster_files/sl.tif'

#Create a list of QgsRasterCalculatorEntry objects for each RUSLE factor
raster_entries = []
for factor, layer_name in raster_factors.items():
    layer = QgsProject.instance().mapLayersByName(layer_name)[0]
    raster_entry = QgsRasterCalculatorEntry()
    raster_entry.ref = factor
    raster_entry.raster = layer
    raster_entry.bandNumber = 1
    raster_entries.append(raster_entry)

#Define the RUSLE equation as a string
rusle_expression = "R * K * LS * C * P"

#Get the dimensions of the first input layer
output_layer = QgsProject.instance().mapLayersByName(list(raster_factors.values())[0])[0]

#Create a QgsRasterCalculator object to calculate the soil loss raster
calc = QgsRasterCalculator(rusle_expression, output_raster, 'GTiff', output_layer.extent(), output_layer.width(), output_layer.height(), raster_entries)
calc.processCalculation()

**Explanation of Code**

**•	Import the appropriate libraries:** For raster calculations, the ‘qgis.analysis’ module from the QGIS library is imported, and the pathlib module is imported for working with file paths.

**•	Create raster layers for each RUSLE factor as follows:** For each RUSLE factor, a dictionary ‘raster_factors’ is formed to record the file paths of the input rasters. A Windows file path format is used to define the paths.

**•	Set the path to the output raster as follows:** The file path where the output raster will be saved is stored in the string ‘output_raster’. The path is specified in the Windows file path format.

**•	For each RUSLE factor, create a list of QgsRasterCalculatorEntry objects:** For each input raster layer, a list ‘raster_entries’ is generated to store ‘QgsRasterCalculatorEntry’ objects. For each input raster layer, a for loop is used to go over the ‘raster_factors’ dictionary and generate a ‘QgsRasterCalculatorEntry’ object. The ‘mapLayersByName()’ function is used to retrieve the ‘QgsRasterLayer’ object for each layer, and the ‘QgsRasterCalculatorEntry’ object is constructed with the layer's name as the reference and the layer as the raster.

**•	As a string, define the RUSLE equation:** The RUSLE equation is stored in a mathematical expression format utilising the factor names as variables in a string ‘rusle_expression’.

**•	Get the first input layer's dimensions:** The variable ‘output_layer’ is set to hold the ‘QgsRasterLayer’ object for the first input raster layer. This is used to obtain the output raster's dimensions (width and height) and extent.

**•	To compute the soil loss raster, create a QgsRasterCalculator object as follows:** The 'QgsRasterCalculator()' constructor returns a QgsRasterCalculator object calc. The RUSLE equation string, the output raster file location, the output raster format (GeoTiff), the extent and dimensions of the output layer, and a list of QgsRasterCalculatorEntry objects for each input raster layer are all initialised in the object.

**•	Carry out the calculation:** The ‘QgsRasterCalculator’ object's processCalculation() function is invoked to execute the raster calculation and store the output raster to the provided file directory.
This programme computes rasters based on the RUSLE equation and a set of input rasters for each RUSLE factor. The result is a single raster layer showing the predicted soil loss in the study region, which may be used to identify high-risk erosion locations and inform soil conservation strategies.


**Conclusion & Reccomendation**
The Upper Krishna Basin in Maharashtra and Karnataka, India, is a region plagued by soil erosion. The application of GIS technologies and the Revised Universal Soil Loss Equation (RUSLE) allows for the prediction and management of soil erosion in the region. According to our findings, the expected soil loss in the Upper Krishna Basin is roughly 2.02 tonnes per acre per year.

This level of soil loss is worisome because it can result in decreased soil fertility and productivity, increased sedimentation in rivers and streams, and downstream flooding. Soil loss can also contribute to decreasing water quality, which can have an impact on agriculture and drinking water supplies in the region. As a result, there is an urgent need in the region for sustainable soil management practises.

The RUSLE model, built with PyQGIS and geospatial technologies, is a useful tool for predicting soil erosion and assessing the efficacy of various soil conservation practises. The output of the model can be used to prioritise areas for soil conservation activities and indicate areas where soil erosion mitigation measures are required.

Overall, the Upper Krishna Basin's estimated soil loss of 2.02 tonnes per acre per year underscores the region's need for appropriate soil management practises. The RUSLE model, when combined with GIS technology, provides an effective tool for predicting and regulating soil erosion in the region, thereby helping to the long-term management of soil resources and environmental protection.

After analysing the soil erosion risk zones at the Upper Krishna Basin using the RUSLE model and GIS, the following conclusions and recommendations can be made:

**Conclusion: -** 
•	The slope, rainfall intensity, and land use/land cover were identified as the major factors contributing to soil erosion in the study area.

•	The high soil erosion risk zones were found in the western and southwestern parts of the study area, which are characterized by steep slopes and intensive agriculture activities.

**Recommendations: -** 
•	Implementation of suitable conservation measures such as contour bunding, terracing, and agroforestry practices in the high-risk areas to reduce soil erosion.

•	Promoting the use of organic farming practices, which can help to increase soil fertility and reduce soil erosion.

•	Promoting afforestation and reforestation programs in the high-risk areas, which can help to stabilize slopes and reduce soil erosion.

•	Improving the drainage systems in the study area to reduce the impact of high-intensity rainfall events on soil erosion.

Overall, the study highlights the need for a comprehensive approach to address the issue of soil erosion in the Upper Krishna Basin. The implementation of suitable conservation measures can help to reduce soil erosion and improve soil health, which can have significant positive impacts on the local environment, agriculture, and livelihoods of the people in the area.

**Final Map of Soil Loss at Upper Krishna Basin**
![Soil_Loss_Compress](https://user-images.githubusercontent.com/132006776/235300946-eddcd5fb-69c7-4ff6-ab9e-ddc89956737a.jpg)


                                                            
                        




